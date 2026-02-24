# 06 — Cursor CLI 搭配 Worktree

## 一句話說明

> **Worktree** 讓同一個 repo 有多個目錄（各在不同 branch）；**Cursor CLI** 是「在哪個目錄跑，就改哪個目錄」。兩者搭在一起，就能用多個終端機、同時讓 CLI Agent 並行處理不同 branch。

---

## 為什麼要這樣搭？

| 做法 | 怎麼做 | 適合誰 |
|------|--------|--------|
| Worktree + 多個 Cursor **視窗** | 每個視窗開一個 worktree | 喜歡用 IDE、要看檔案樹與預覽 |
| Worktree + **CLI** | 多個終端機，各自 `cd` 到不同 worktree，跑 `cursor agent` | 習慣鍵盤流、或只在伺服器/SSH 環境 |

**共通點**：每個 worktree 是獨立目錄 → 各自有獨立上下文 → 可以並行開發，互不干擾。

---

## 基本用法：一個終端機對應一個 Worktree

假設你已經建好 worktree（可參考 `../worktree/02-setup-worktrees.md`）：

```
~/projects/
├── my-app/                  ← main
├── my-app--feature-auth/    ← feature/auth
└── my-app--feature-payment/ ← feature/payment
```

**終端機 1**（做 auth）：
```bash
cd ~/projects/my-app--feature-auth
cursor agent "實作 JWT 登入"
```

**終端機 2**（做 payment）：
```bash
cd ~/projects/my-app--feature-payment
cursor agent "加上 Stripe 結帳流程"
```

- CLI 預設以**目前工作目錄**為上下文，所以兩個終端機各自只會改自己的 worktree。
- 這樣就等於「兩個 CLI Agent 並行」，不用開兩個 Cursor 視窗。

---

## 不切目錄：用 `--path` 指定 Worktree

你人在主專案，但想叫 CLI 去改某個 worktree：

```bash
cd ~/projects/my-app
cursor agent "幫 feature/auth 加上單元測試" --path ../my-app--feature-auth
```

- `--path` 指定「要看的目錄」＝那個 worktree 的路徑。
- 適合：一次只處理一個 worktree、或寫腳本時固定路徑。

---

## 非互動 + Worktree（腳本 / 批次）

可以對多個 worktree 依序跑同一類任務（例如都加 README、都跑 linter 修復）：

```bash
# 範例：對兩個 worktree 都跑「修 lint」
cursor agent "依 .eslintrc 修復所有可自動修復的錯誤" --path ../my-app--feature-auth --no-interactive
cursor agent "依 .eslintrc 修復所有可自動修復的錯誤" --path ../my-app--feature-payment --no-interactive
```

或寫成迴圈（依你實際路徑調整）：

```bash
for dir in ../my-app--feature-auth ../my-app--feature-payment; do
  cursor agent "修復 linter 錯誤" --path "$dir" --no-interactive
done
```

---

## 和「Worktree + 多視窗」的關係

- **多視窗**：每個 Cursor 視窗開一個 worktree，用 IDE 裡的 Agent、Chat、Tab 補全。
- **CLI + Worktree**：用終端機 + `cursor agent`，一樣是「一個目錄一個上下文」，可多開幾個終端機並行。

可以混用：例如主開發用視窗，某個 branch 只在伺服器上用 CLI 修一點小東西。

---

## 小結

- Worktree = 多個目錄、多個 branch；CLI = 依「目前目錄」或 `--path` 做事 → 天然搭配。
- **並行**：多開終端機，各自 `cd` 到不同 worktree，再跑 `cursor agent`。
- **單一終端**：用 `--path 某個-worktree 路徑` 指定要改哪個 worktree。
- 腳本／批次：用 `--path` + `--no-interactive` 對多個 worktree 做同一類任務。

---

> 下一步：`07-unlimit-loop-debug.md` — 讓 CLI 自動迴圈修 code，直到測試通過。
