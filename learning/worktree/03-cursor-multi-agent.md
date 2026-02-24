# 03 — Cursor 多視窗 Agent 設定

## 核心思路

每個 Worktree 對應一個 **Cursor 視窗**，每個視窗都有自己的 **Agent**。

```
Worktree 1 (feature/auth)    → Cursor 視窗 1 → Agent A
Worktree 2 (feature/payment) → Cursor 視窗 2 → Agent B
Worktree 3 (bugfix/cart)     → Cursor 視窗 3 → Agent C
```

Agent 之間完全獨立，互不干擾。

---

## 開啟多個 Cursor 視窗

### 方法一：從終端開啟（推薦）

```bash
# 開啟視窗 1（主倉庫）
cursor ~/projects/my-app

# 開啟視窗 2
cursor ~/projects/my-app--feature-auth

# 開啟視窗 3
cursor ~/projects/my-app--feature-payment
```

### 方法二：從 Cursor 選單開啟

1. 在 Cursor 中按 `Cmd+Shift+N`（新視窗）
2. 選擇 `Open Folder`
3. 選擇對應的 worktree 資料夾

---

## 啟動各視窗的 Agent

### 在每個視窗中：

1. 按 `Cmd+I` 開啟 Agent 模式（或點擊 Chat 面板）
2. 切換到 **Agent** 模式（不是 Ask 模式）
3. 給 Agent 明確的任務描述

---

## 給 Agent 的任務範本

**視窗 1 — 登入功能：**
```
你的任務：實作使用者登入功能
- 建立 /api/auth/login 的 POST endpoint
- 使用 JWT token 做驗證
- 相關文件在 src/auth/ 資料夾
請完成後告訴我測試方法
```

**視窗 2 — 付款功能：**
```
你的任務：整合 Stripe 付款
- 建立付款 session
- 處理 webhook 回調
- 相關文件在 src/payment/ 資料夾
遇到問題請暫停並說明
```

---

## 每個視窗的 Agent 設定

### 模型選擇建議

| 任務類型 | 建議模型 |
|---------|---------|
| 複雜功能開發 | Claude 3.5 Sonnet |
| 快速修 Bug | 預設模型即可 |
| 大型重構 | 開啟 Max Mode |

### 讓 Agent 只動自己的文件

在任務描述中明確說明範圍：
```
只修改 src/auth/ 資料夾內的文件
不要動 src/payment/ 和 src/cart/ 的任何文件
```

---

## 監控多個 Agent 的狀態

Cursor 的 Agent 執行時，Chat 面板會顯示：
- 目前在做什麼（讀取/寫入/執行指令）
- 已完成的步驟
- 遇到的錯誤

**切換視窗的方式：**
- Mac：`Cmd+Tab` 在應用程式間切換
- 或在 Dock 點選對應的 Cursor 視窗

---

## 視覺化你的工作狀態

在一張紙或便利貼上記錄：
```
視窗 1 (auth)    → [進行中] 做 JWT 驗證
視窗 2 (payment) → [等待]  還沒開始
視窗 3 (bugfix)  → [完成]  Bug 已修復，等待 review
```

---

> 下一步：`04-parallel-workflow.md` — 完整的並行開發實戰流程
