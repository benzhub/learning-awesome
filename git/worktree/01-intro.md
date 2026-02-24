# Git Worktree 是什麼？

## 傳統切換分支的痛點

你有沒有遇過這種情況：

> 正在 `feature/new-ui` 分支開發新功能，突然被通知有緊急 bug 要修！
> 你必須先 `stash` 現有改動、切換到 `main` 分支、修 bug、commit、再切回來、再 `stash pop`……

這個流程很麻煩，而且容易出錯。

---

## Worktree 的解法

**Git Worktree** 讓你可以**同時 checkout 多個分支**，每個分支放在不同的資料夾，互不干擾。

```
my-project/              ← 主工作目錄（main 分支）
my-project-hotfix/       ← worktree（hotfix/login 分支）
my-project-feature/      ← worktree（feature/new-ui 分支）
```

每個資料夾都是獨立的工作區，可以同時開啟、同時編輯，**不需要切換分支**。

---

## 核心觀念

| 概念 | 說明 |
|------|------|
| **主 worktree** | `git clone` 或 `git init` 後的原始目錄 |
| **連結 worktree** | 用 `git worktree add` 額外建立的工作目錄 |
| **共用 .git** | 所有 worktree 共享同一個 `.git` 資料夾，不佔額外空間 |

> 💡 不同 worktree **不能 checkout 同一個分支**，Git 會報錯保護你。

---

## 適合使用的場景

- 🔥 緊急修 bug，不想中斷目前開發
- 👀 需要對照兩個分支的程式碼差異
- 🔨 同時進行多個功能開發
- 🚀 在不同分支跑不同的 build / 測試

---

## 下一步

👉 [02-basic.md](./02-basic.md) — 學習基本操作指令
