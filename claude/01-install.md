# 01 — 安裝 Claude Code CLI

## 一句話

> 用官方安裝腳本一鍵安裝，不需要 Node.js，裝完輸入 `claude` 就能開始。

---

## macOS / Linux / WSL

在終端機貼上並執行：

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

- 自動安裝獨立執行檔，**不需要 npm 或 Node.js**
- 安裝完畢後重開終端機，或執行 `source ~/.zshrc` 讓路徑生效
- 會在背景**自動更新**到最新版本

---

## Windows（PowerShell）

用**系統管理員**開 PowerShell，執行：

```powershell
irm https://claude.ai/install.ps1 | iex
```

> 💡 Windows 也可以直接跑，或在 WSL 環境裡安裝。

---

## 其他安裝方式

| 方式 | 指令 | 備註 |
|------|------|------|
| Homebrew（macOS） | `brew install claude-code` | 不會自動更新，需手動 `brew upgrade claude-code` |
| WinGet（Windows） | `winget install Anthropic.ClaudeCode` | 不會自動更新，需手動升級 |

---

## 驗證是否安裝成功

```bash
claude --version
```

有出現版本號就代表裝好了。

---

## 設定 API Key

Claude Code 需要 Anthropic API Key 才能運作。

### 方法一：環境變數（推薦）

在 `~/.zshrc` 或 `~/.bashrc` 加入：

```bash
export ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxxxxx"
```

然後重新載入：

```bash
source ~/.zshrc
```

### 方法二：`.env` 檔案

在專案根目錄建立 `.env`：

```
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxx
```

> 💡 API Key 在 [console.anthropic.com](https://console.anthropic.com) → API Keys 取得。

---

## 健康檢查

安裝完、設定好 Key 之後，執行：

```bash
claude doctor
```

會自動檢查環境與安裝類型是否都就緒，有問題會直接提示你怎麼修。

---

## 小結

```
curl -fsSL https://claude.ai/install.sh | bash   # 安裝（macOS/Linux）
claude --version                                   # 確認版本
export ANTHROPIC_API_KEY="sk-ant-..."             # 設定 Key
claude doctor                                      # 健康檢查
```

---

> 下一步：`02-basic-usage.md` — 學怎麼和 AI 互動、讓它幫你改程式碼
