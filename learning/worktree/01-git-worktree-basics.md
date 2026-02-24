# 01 — Git Worktree 是什麼？

## 用一句話解釋

> Git Worktree 讓你**同一個 Git 倉庫**可以同時在**多個資料夾**中工作，每個資料夾可以在不同的 branch 上。

---

## 類比：想像一個辦公室

**沒有 Worktree（傳統）：**
```
一張桌子，一次只能處理一個專案
→ 做完 A 才能做 B
```

**有 Worktree：**
```
多張桌子，每張桌子放不同的專案
→ A、B、C 同時進行
```

---

## 實際的資料夾結構

```
~/projects/
├── my-app/              ← 主倉庫（main branch）
├── my-app-feature-auth/ ← worktree（feature/auth branch）
└── my-app-bugfix-login/ ← worktree（bugfix/login branch）
```

這三個資料夾共用**同一個 Git 歷史**（.git），但各自在不同 branch。

---

## 和 `git clone` 有什麼不同？

| | git clone | git worktree |
|--|-----------|--------------|
| .git 資料夾 | 各自獨立 | 共用同一個 |
| 磁碟空間 | 佔多份 | 只佔一份 |
| 推送/拉取 | 各自管理 | 統一管理 |
| 適合場景 | 完全不同的專案 | 同專案的不同功能 |

---

## Worktree 的限制（先知道）

1. **同一個 branch 不能同時被兩個 worktree 使用**
   - ✅ worktree-1 用 `feature/auth`，worktree-2 用 `feature/payment`
   - ❌ 兩個都用 `main`（會報錯）

2. **每個 worktree 是獨立的工作目錄**
   - 在 worktree-1 修改的文件不會影響 worktree-2

---

## 常用指令速查

```bash
# 查看目前所有 worktree
git worktree list

# 建立新 worktree（同時建立新 branch）
git worktree add ../my-app-feature-auth -b feature/auth

# 建立新 worktree（使用已有的 branch）
git worktree add ../my-app-bugfix bugfix/login

# 刪除 worktree
git worktree remove ../my-app-feature-auth
```

---

## 小結

- Worktree = 同一個 repo 的多個工作目錄
- 每個目錄各自在不同 branch
- 不需要 stash、不需要切換 branch

---

> 下一步：`02-setup-worktrees.md` — 實際動手建立 Worktree
