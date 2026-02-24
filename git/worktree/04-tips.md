# Git Worktree 常見問題與注意事項

## 常見錯誤

### ❌ 錯誤：試圖 checkout 已被使用的分支

```
fatal: 'main' is already checked out at '/Users/you/my-app'
```

**原因**：同一個分支不能同時被兩個 worktree 使用。

**解法**：建立新分支，或換一個還沒被 checkout 的分支。

```bash
# 從 main 建一個新分支給 worktree 用
git worktree add -b hotfix/login ../my-app-hotfix main
```

---

### ❌ 錯誤：worktree 資料夾已存在

```
fatal: '../my-app-hotfix' already exists
```

**解法**：選一個不存在的目錄名，或先刪除舊資料夾。

---

### ❌ 錯誤：remove 失敗（有未 commit 改動）

```
fatal: '../my-app-hotfix' contains modified or untracked files
```

**解法**：commit 或放棄改動後再刪，或強制刪除：

```bash
git worktree remove --force ../my-app-hotfix
```

---

## 注意事項

### 1. `node_modules` 不共享

每個 worktree 資料夾都是獨立的，`node_modules` 需要各自安裝：

```bash
cd ../my-app-hotfix
npm install
```

> 💡 可以考慮用 [pnpm](https://pnpm.io/) 的 global store，安裝速度更快、共用 cache。

---

### 2. `.git` 是共享的

所有 worktree 共用同一個 `.git`，所以：

- 在任一 worktree 建立的 **tag、stash** 全部都能看到
- `git log` 在任一 worktree 看到的歷史都一樣

---

### 3. 不建議把 worktree 放在主目錄內

```bash
# ❌ 不好，容易混淆
git worktree add ./worktrees/hotfix hotfix/login

# ✅ 建議放在主目錄的平行位置
git worktree add ../my-app-hotfix hotfix/login
```

放在外面更清楚，也不會被主 worktree 的 `.gitignore` 規則影響。

---

### 4. 用完記得清理

```bash
# 列出目前所有 worktree
git worktree list

# 清理已刪除資料夾的殘留記錄
git worktree prune
```

養成習慣，用完就清，避免 `.git` 裡殘留大量失效的 worktree 記錄。

---

## 快速診斷清單

| 問題 | 解法 |
|------|------|
| 分支已被 checkout | 建新分支或改用其他分支 |
| 目錄已存在 | 換目錄名或先刪除 |
| remove 失敗 | commit 改動或加 `--force` |
| 看不到新的 worktree | 執行 `git worktree list` 確認 |
| 殘留記錄 | 執行 `git worktree prune` |

---

## 總結

Git Worktree 是個簡單但非常實用的功能。記住三件事：

1. **`add`** 建立新工作區
2. **`list`** 查看所有工作區
3. **`remove` + `prune`** 清理工作區

搭配好的目錄命名習慣，可以大幅提升多任務開發的效率！
