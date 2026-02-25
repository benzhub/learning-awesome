# 05 — 小技巧與常見問題

## 提升效率的小技巧

### 1. 用 `CLAUDE.md` 讓 AI 記住你的專案

在專案根目錄建立 `CLAUDE.md`，寫下這個專案的重要背景：

```markdown
# CLAUDE.md

## 專案簡介
這是一個用 Next.js + Prisma 做的電商後台。

## 常用指令
- `pnpm dev` — 啟動開發伺服器
- `pnpm test` — 跑測試

## 架構重點
- API 路由在 `src/app/api/`
- 資料庫 Schema 在 `prisma/schema.prisma`
```

之後每次啟動 `claude`，AI 都會自動讀這份文件，不用每次重新介紹。

---

### 2. 開新專案先 `/init`

第一次在一個陌生專案用 Claude Code：

```
> /init
```

AI 會自動掃描並產生 `CLAUDE.md`，省下手寫的時間。

---

### 3. 對話太長用 `/compact` 不要 `/clear`

`/clear` 會清掉所有記憶，`/compact` 只是壓縮，AI 還記得你在做什麼。

- 任務還在進行中 → `/compact`
- 整個換話題或 AI 亂掉 → `/clear`

---

### 4. 用 `-c` 快速接回昨天的工作

早上打開終端機，接著昨天的任務繼續：

```bash
claude -c
```

---

### 5. 把常用的 prompt 存成 shell alias

```bash
# 加在 ~/.zshrc
alias cr="claude --resume"
alias cp="claude -p"
alias cc="claude -c"
```

---

## 常見問題

### Q: 啟動後說 API Key 無效？

確認 Key 是否正確設定：

```bash
echo $ANTHROPIC_API_KEY
```

沒有輸出代表沒設好。重新 export 或檢查 `~/.zshrc` 裡的寫法。

---

### Q: AI 修改了我不想改的檔案，怎麼辦？

Claude Code 會在修改前詢問確認。如果你按了確認但後悔了：

```bash
git diff        # 看改了什麼
git checkout .  # 還原所有改動（小心，這會丟掉未 commit 的修改）
```

> 💡 建議在讓 AI 做大改動前先 `git commit`，方便還原。

---

### Q: 回答變慢或卡住？

可能是 context 太長。試試：

```
> /compact
```

或退出重開：

```bash
claude -c  # 接回對話但重新連線
```

---

### Q: 如何讓 AI 只讀不改？

在啟動時加旗標：

```bash
claude --no-auto-approval  # 每個動作都要你手動確認
```

或在互動中只用問的方式：「解釋這段程式碼」、「找出這裡的問題」，不要說「幫我修改」，AI 就不會動你的檔案。

---

## 小結

| 情境 | 做法 |
|------|------|
| 讓 AI 記住專案背景 | 建立 `CLAUDE.md` 或用 `/init` |
| 對話太長、想省空間 | `/compact` |
| 完全重置 | `/clear` |
| 接回上次工作 | `claude -c` |
| 快速問不進互動模式 | `claude -p "問題"` |
| 防止 AI 亂改東西 | 每次改動前先 `git commit` |
