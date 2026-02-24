# 04 — 非互動模式（腳本與 CI）

## 什麼是非互動模式？

**互動模式**：AI 問你、你按 Y/N、一步步確認。  
**非互動模式**：你給一個指令，跑完就結束，**不需要人在旁邊按鍵**。適合寫在腳本或 CI 裡。

---

## 基本用法

加上 `--no-interactive`（或 `-n`）：

```bash
cursor agent "幫專案加上 .editorconfig" --no-interactive
```

- AI 會直接執行，不會停下來問你
- 適合：已知結果、可重複執行的任務

---

## 在腳本裡用

```bash
#!/bin/bash
cd /path/to/project
cursor agent "依照 CONTRIBUTING.md 跑 linter 並修復" --no-interactive
```

- 自動化 code 整理、簡單重構、加註解等
- 記得 CI 環境要有 Cursor 帳號授權（例如用 `cursor auth login` 或 token）

---

## 指定模型（可選）

若想指定用哪個模型：

```bash
cursor agent "重寫這段註解" --model claude-3-5-sonnet --no-interactive
```

實際可用模型名以官方文件為準。

---

## 雲端繼續跑（-c）

若任務很重、想丟到雲端跑：

```bash
cursor agent "分析整個 repo 並產出架構文件" -c
```

- `-c` 會把對話交給雲端 Agent 繼續跑
- 你關掉終端機也沒關係，之後可以再接回對話

---

## 小結

- 非互動：加 `--no-interactive` 或 `-n`
- 腳本/CI：同一指令，確保環境已登入 Cursor
- 指定模型：`--model 模型名`
- 長時間任務：用 `-c` 交給雲端跑

---

> 下一步：`05-tips.md` — 小技巧與常見問題
