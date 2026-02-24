# 07 — 無限除錯迴圈：讓 CLI 一直改到測試通過

## 一句話說明

> 寫一個 shell 腳本，讓 `cursor agent` 自動修 code、跑測試、測試失敗就再讓 AI 修，**直到測試全過才停下來**。

---

## 核心概念

```
while 測試沒過:
    跑測試 → 收集錯誤訊息
    把錯誤訊息餵給 cursor agent
    cursor agent 修 code
    重新跑測試
```

人不用盯著看，讓 AI 自己跑圈直到成功。

---

## Step 1：確認前置條件

- `cursor` 已安裝且登入（`cursor --version` 驗證）
- 專案有**可以在終端機跑的測試指令**，例如：
  - `npm test`
  - `pytest`
  - `go test ./...`
  - `cargo test`
- 測試失敗時 exit code 不為 0（幾乎所有測試工具預設就是這樣）

---

## Step 2：最簡單的腳本

建立 `debug-loop.sh`：

```bash
#!/bin/bash

MAX_LOOPS=10        # 最多跑幾圈，防止真的無限跑
TEST_CMD="npm test" # 換成你的測試指令
loop=0

while [ $loop -lt $MAX_LOOPS ]; do
  echo ""
  echo "=== 第 $((loop + 1)) 圈 ==="

  # 跑測試，把 stdout + stderr 都存起來
  TEST_OUTPUT=$(eval "$TEST_CMD" 2>&1)
  EXIT_CODE=$?

  if [ $EXIT_CODE -eq 0 ]; then
    echo "✅ 測試通過！共跑了 $((loop + 1)) 圈。"
    exit 0
  fi

  echo "❌ 測試失敗，讓 AI 來修..."
  echo "$TEST_OUTPUT" | tail -30  # 印出最後 30 行讓你看

  # 把錯誤訊息餵給 cursor agent
  cursor agent "測試失敗了，錯誤如下，請修 code 讓測試通過：

$TEST_OUTPUT" --model gpt-codex-5.3-high --no-interactive

  loop=$((loop + 1))
done

echo "⚠️  已達最大圈數 ($MAX_LOOPS)，停止。請手動檢查。"
exit 1
```

---

## Step 3：執行腳本

```bash
chmod +x debug-loop.sh
./debug-loop.sh
```

你可以去做別的事，等終端機出現 `✅ 測試通過！` 再回來。

---

## Step 4：根據專案調整

**換測試指令**（第 4 行）：
```bash
TEST_CMD="pytest -x"          # Python，-x 遇第一個錯就停
TEST_CMD="go test ./..."      # Go
TEST_CMD="cargo test"         # Rust
TEST_CMD="./vendor/bin/phpunit" # PHP
```

**調整最大圈數**：
```bash
MAX_LOOPS=5   # 保守，改不好早點停
MAX_LOOPS=20  # 問題複雜時多給幾圈
```

**讓 AI 看到更多上下文**（放在 `cursor agent` 指令裡）：
```bash
cursor agent "測試失敗，錯誤如下。
測試框架是 Jest，語言是 TypeScript。
不要改測試檔案，只改 src/ 底下的檔案。

錯誤訊息：
$TEST_OUTPUT" --model gpt-codex-5.3-high --no-interactive
```

---

## 加強版：保留每圈的日誌

```bash
#!/bin/bash

MAX_LOOPS=10
TEST_CMD="npm test"
LOG_DIR="debug-logs"
mkdir -p $LOG_DIR

loop=0

while [ $loop -lt $MAX_LOOPS ]; do
  echo ""
  echo "=== 第 $((loop + 1)) 圈 ==="

  LOG_FILE="$LOG_DIR/loop-$((loop + 1)).log"
  TEST_OUTPUT=$(eval "$TEST_CMD" 2>&1 | tee "$LOG_FILE")
  EXIT_CODE=${PIPESTATUS[0]}

  if [ $EXIT_CODE -eq 0 ]; then
    echo "✅ 測試通過！共跑了 $((loop + 1)) 圈。"
    exit 0
  fi

  echo "❌ 失敗，日誌存到 $LOG_FILE"

  cursor agent "測試失敗，錯誤如下，請修 code 讓測試通過：

$TEST_OUTPUT" --model gpt-codex-5.3-high --no-interactive

  loop=$((loop + 1))
done

echo "⚠️  達最大圈數，停止。請看 $LOG_DIR/ 裡的日誌。"
exit 1
```

---

## 安全注意事項

| 風險 | 怎麼防 |
|------|--------|
| AI 改爛更多檔案 | 先用 `git commit` 備份現狀，不對就 `git checkout .` 還原 |
| 跑太多圈浪費額度 | 設合理的 `MAX_LOOPS`（建議 5～15） |
| AI 改到測試檔本身 | 提示詞明確說「不要改測試檔案」 |
| 每圈失敗原因不同 | 保留日誌（加強版腳本），方便事後分析 |

---

## 小結

1. 腳本結構：`跑測試 → 失敗 → 餵給 AI → 重複`
2. 關鍵參數：`TEST_CMD`（測試指令）、`MAX_LOOPS`（最大圈數）
3. 提示詞要清楚：說清楚語言、限制改哪些目錄
4. 事前 `git commit`，壞了隨時還原

---

> 搭配 Worktree 用：在不同 branch 各跑一個除錯迴圈，可參考 `06-cursor-cli-with-worktree.md`。
