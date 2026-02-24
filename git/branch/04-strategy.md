# Git Branch 合併策略

## Merge vs Rebase

| 場景 | 推薦方式 | 命令 |
|------|----------|------|
| feature → main（公共分支） | **Merge**（保留歷史） | `git merge --no-ff feature/xxx` |
| 更新 feature 分支到最新 main | **Rebase**（保持線性） | `git rebase main` |
| 修復緊急 bug | **Cherry-pick** | `git cherry-pick <commit-hash>` |
| 整理 feature 分支 commit | **Interactive rebase** | `git rebase -i HEAD~N` |

---

## Merge（合併）

### 一般 merge

```bash
git checkout main
git merge feature/add-login
```

會產生一個 merge commit，保留分支歷史。

### 強制 no-ff（推薦）

```bash
git merge --no-ff feature/add-login
```

即使可以 fast-forward，也會產生 merge commit，讓分支歷史更清晰。

---

## Rebase（變基）

把目前分支的 commit「搬到」目標分支的最新 commit 之後：

```bash
git checkout feature/add-login
git rebase main
```

> ⚠️ **不要**對已經 push 到遠端的公共分支做 rebase，會改寫歷史。

---

## Cherry-pick（揀選）

只把某個 commit 套到目前分支：

```bash
git checkout main
git cherry-pick abc1234
```

適合緊急修復：在 hotfix 分支修好後，把對應 commit 揀到 main。

---

## 衝突解決

```bash
# 1. 查看衝突文件
git status

# 2. 手動編輯衝突標記，或使用
git mergetool

# 3. 標記已解決
git add <file>

# 4. 繼續 merge/rebase
git merge --continue
# 或
git rebase --continue

# 5. 放棄操作
git merge --abort
git rebase --abort
```

---

## 總結

- **Merge**：合併到 main 時用，保留完整歷史
- **Rebase**：整理 feature 分支時用，讓歷史更線性
- **Cherry-pick**：緊急修復時用，只選特定 commit
