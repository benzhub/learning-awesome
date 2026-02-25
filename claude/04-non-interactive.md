# 04 — 非互動模式與腳本自動化

## 什麼是非互動模式？

預設的 `claude` 是**互動模式**：你打字、AI 回應，像在聊天。

**非互動模式**：用 `-p` 旗標，AI 回答完就自動退出，適合用在腳本、CI/CD、或直接在終端機取得一次性答案。

---

## 基本用法

```bash
claude -p "解釋 src/auth.ts 在做什麼"
```

等同於：

```bash
claude --print "解釋 src/auth.ts 在做什麼"
```

AI 的回答會直接輸出到 stdout，跑完就退出。

---

## 實用情境

### 快速問問題，不想進互動模式

```bash
claude -p "Git rebase 和 merge 的差別是什麼？"
```

### 讓 AI 產生檔案內容

```bash
claude -p "幫我寫一個 .gitignore，適合 Node.js + TypeScript 專案" > .gitignore
```

### 搭配 shell 腳本

```bash
#!/bin/bash
ERROR_LOG=$(cat ./logs/error.log)
claude -p "分析以下錯誤 log，找出可能的原因：\n$ERROR_LOG"
```

### 在 CI/CD 中自動 code review

```yaml
# GitHub Actions 範例
- name: AI Code Review
  run: |
    git diff HEAD~1 HEAD > diff.txt
    claude -p "審查以下 git diff，列出潛在問題：$(cat diff.txt)"
  env:
    ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

---

## 輸出格式選項

預設輸出是純文字，加上 `--output-format` 可以指定格式：

```bash
# JSON 格式輸出（方便腳本解析）
claude -p "列出這個專案的主要模組" --output-format json

# 串流輸出（邊產生邊顯示）
claude -p "幫我寫一份詳細的 README" --output-format stream-json
```

---

## 限制 AI 的權限

在腳本中，有時只想讓 AI **讀**，不想讓它**動**你的檔案：

```bash
# 不允許任何工具（純問答）
claude -p "解釋 bubble sort 的時間複雜度" --no-tools
```

---

## 互動模式 vs 非互動模式

| | 互動模式 | 非互動模式 (`-p`) |
|---|----------|-------------------|
| 啟動 | `claude` | `claude -p "..."` |
| 對話 | 多輪來回 | 一問一答就結束 |
| 適合 | 複雜任務、需要確認 | 腳本、CI、快速問題 |
| 輸出 | 互動式介面 | 純 stdout |

---

## 小結

```bash
claude -p "問題或任務"                # 一次性問答
claude -p "任務" > output.txt         # 輸出存檔
claude -p "問題" --output-format json # JSON 格式
claude -p "問題" --no-tools           # 禁止 AI 動檔案
```

---

> 下一步：`05-tips.md` — 提升效率的小技巧與常見問題
