# Git Worktree 實際使用情境

## 情境一：緊急 Bug 修復

你正在 `feature/checkout-flow` 開發新功能，突然收到通知：**線上登入壞了！**

### 傳統做法（麻煩）

```bash
git stash                        # 先藏起來
git checkout main                # 切換分支
git checkout -b hotfix/login     # 建新分支
# 修 bug...
git commit -m "fix(auth): ..."
git checkout feature/checkout-flow
git stash pop                    # 還原
```

### Worktree 做法（順暢）

```bash
# 不離開目前資料夾，直接開一個新工作區
git worktree add -b hotfix/login ../my-app-hotfix main

# 進入新工作區修 bug
cd ../my-app-hotfix
# 修 bug...
git commit -m "fix(auth): fix login null pointer"

# 回去繼續開發，完全不受影響
cd ../my-app
```

> ✅ 原本的工作區**一行改動都沒有動到**。

---

## 情境二：對照兩個版本的程式碼

想知道 `v1` 和 `v2` 的某個檔案差在哪裡？

```bash
git worktree add ../my-app-v1 v1.0.0
git worktree add ../my-app-v2 v2.0.0
```

現在用編輯器同時打開兩個資料夾，直接並排對比。

---

## 情境三：同時跑不同分支的測試

```bash
git worktree add ../app-staging staging
git worktree add ../app-prod    production
```

```bash
# Terminal 1
cd ../app-staging && npm test

# Terminal 2
cd ../app-prod && npm test
```

兩個測試同時跑，不互相等待。

---

## 情境四：Code Review 時不打斷開發

```bash
# checkout PR 分支到獨立資料夾做 review
git worktree add ../review-pr-123 pr/123-new-payment-flow
cd ../review-pr-123
# 看 code、執行、測試...
```

Review 完直接刪掉這個 worktree，不影響你正在進行的工作。

```bash
git worktree remove ../review-pr-123
```

---

## 情境五：搭配 tmux / 多視窗開發

```
┌─────────────────────┬─────────────────────┐
│  Terminal 1          │  Terminal 2          │
│  ~/my-app            │  ~/my-app-hotfix     │
│  (feature/dark-mode) │  (hotfix/login-bug)  │
│                      │                      │
│  npm run dev         │  npm run dev         │
└─────────────────────┴─────────────────────┘
```

兩個 dev server 同時跑，各自對應不同分支。

---

## 下一步

👉 [04-tips.md](./04-tips.md) — 常見問題與注意事項
