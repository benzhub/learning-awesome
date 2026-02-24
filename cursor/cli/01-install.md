# 01 — 安裝 Cursor CLI

## 一句話

> 在終端機執行官方安裝腳本，裝完用 `cursor` 指令就能用。

---

## macOS / Linux / WSL

在終端機貼上並執行：

```bash
curl https://cursor.com/install -fsS | bash
```

- 會自動下載並安裝 `cursor` 指令
- 裝完**關掉終端機再開**，或執行 `source ~/.bashrc` / `source ~/.zshrc` 讓路徑生效

---

## Windows（PowerShell）

用**系統管理員**開 PowerShell，執行：

```powershell
irm 'https://cursor.com/install?win32=true' | iex
```

---

## 驗證是否安裝成功

```bash
cursor --version
```

有出現版本號就代表裝好了。

---

## 第一次使用：登入

第一次執行時可能會要你登入 Cursor 帳號：

```bash
cursor auth login
```

照畫面指示用瀏覽器登入即可。之後在同一台機器上通常不用再登。

---

## 小結

- macOS/Linux：`curl ... | bash`
- Windows：PowerShell 執行 `irm ... | iex`
- 驗證：`cursor --version`
- 登入：`cursor auth login`

---

> 下一步：`02-basic-usage.md` — 學怎麼下指令、和 AI 對話
