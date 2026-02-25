# 02 — 基本使用

## 最常用的方式：直接輸入 `claude`

在你的專案資料夾裡執行：

```bash
claude
```

進入**互動模式**，你可以用自然語言跟 AI 對話：

```
> 幫我在 src/utils 底下新增一個 formatDate 工具函式
> 這個 bug 是怎麼回事？
> 解釋一下這個函式在做什麼
```

AI 會讀取目前目錄的程式碼，直接幫你修改檔案或解釋邏輯。

---

## 帶初始任務啟動

不想進去之後再打，可以直接在啟動時帶任務：

```bash
claude "幫我重構 auth 模組，改用 JWT"
```

---

## 繼續上次對話

關掉終端機再開，想接著上次講：

```bash
claude --continue
# 或縮寫
claude -c
```

要指定某次對話（用 session ID）：

```bash
claude --resume
```

會列出最近的對話清單，選一個繼續。

---

## 指定模型

預設使用最新的 Claude 模型，也可以手動指定：

```bash
claude --model claude-opus-4-6
claude --model claude-haiku-4-5-20251001
```

---

## 常用指令速查

| 指令 | 說明 |
|------|------|
| `claude` | 進入互動模式 |
| `claude "任務"` | 帶初始任務啟動 |
| `claude -c` | 繼續最近一次對話 |
| `claude --resume` | 選一個舊對話繼續 |
| `claude --model <模型>` | 指定使用的模型 |
| `claude --version` | 查看版本 |
| `claude --help` | 查看所有選項 |

---

## 小結

- 最常用：在專案目錄下打 `claude`，直接開始
- 帶任務啟動：`claude "你的任務"`
- 接上次：`claude -c`
- 換模型：`claude --model <模型名稱>`

---

> 下一步：`03-slash-commands.md` — 對話途中可以用的斜線指令
