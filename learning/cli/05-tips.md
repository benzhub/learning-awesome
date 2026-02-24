# 05 — 小技巧與常見問題

## 小技巧

### 1. 接續上次對話

想延續之前的對話、不用從頭講：

```bash
cursor agent --resume
```

會載入最近一次對話，上下文還在。

---

### 2. 先確認再執行指令

Agent 有時會想執行 `npm install`、`git` 等指令。  
- 互動模式：會問你要不要執行  
- 若你擔心，可在對話裡說：「執行指令前都要先問我」

---

### 3. 和 IDE 一起用

CLI 和 Cursor 視窗**可以同時用**：
- 視窗裡做主要開發
- 終端機用 CLI 做小任務、查文件、跑自動化

---

## 常見問題

**Q：`cursor: command not found`？**  
A：安裝後要重開終端機，或執行 `source ~/.zshrc`（或 `~/.bashrc`）。確認安裝路徑有在 `PATH` 裡。

**Q：CLI 要付費嗎？**  
A：要。需要 Cursor 訂閱才能使用 CLI。

**Q：在伺服器 / Docker 裡能用嗎？**  
A：可以。要能跑安裝腳本、能登入 Cursor（或設定 token）。注意網路與授權設定。

**Q：和 Cursor 視窗的 Agent 一樣嗎？**  
A：背後是同一套能力，但 CLI 是終端機介面、可非互動、可接腳本與 CI。

---

## 小結

- 接續對話：`cursor agent --resume`
- 指令執行前可要求先確認
- CLI 與 IDE 可並用
- 遇到問題先查 `cursor --help` 與官方文件

---

> 下一步：`06-cursor-cli-with-worktree.md` — 用 CLI 搭配 Worktree 並行開發。或回頭看 `00-overview.md` 複習。
