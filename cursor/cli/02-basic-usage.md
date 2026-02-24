# 02 — 基本使用

## 最常用的方式：`cursor agent`

在專案資料夾裡執行：

```bash
cursor agent "幫我把這個專案加上 README"
```

- **互動式**：AI 會問你、給建議，你可以同意或拒絕修改
- **有上下文**：會看目前資料夾的檔案來回答

---

## 一句話 vs 多句話

**一句話直接做：**
```bash
cursor agent "重構 auth 模組，改用 JWT"
```

**想先討論再做：** 只打 `cursor agent` 不帶參數，會進入**對話模式**，你可以慢慢描述需求，AI 會一步步問你、再改 code。

---

## 指定「在哪個目錄」做事

預設是**目前目錄**。若要指定路徑：

```bash
cursor agent "檢查並修復 TypeScript 型別" --path ./src
```

---

## 常用指令速查

| 指令 | 說明 |
|------|------|
| `cursor agent "做某事"` | 讓 AI 幫你做（可改檔案） |
| `cursor agent` | 進入對話模式，不帶任務 |
| `cursor ask "問題"` | 只問問題，不改檔案（唯讀） |
| `cursor --help` | 看所有選項 |

---

## 小結

- 最常用：`cursor agent "你的指令"`
- 想慢慢聊：只打 `cursor agent`
- 只問不改：用 `cursor ask "問題"`
- 指定目錄：加 `--path 路徑`

---

> 下一步：`03-modes.md` — 認識 Agent / Plan / Ask 三種模式
