# Worktree 的 Commit 與 Merge 工作流程

## 情境說明

假設你用 Git Worktree 讓多個 AI Agent 同時開發不同功能：

```
my-project/               ← 主工作區（dev 分支）
my-project-cursor/        ← worktree（cursor 分支）← Agent A 做 Cursor 教學
my-project-git/           ← worktree（git 分支）  ← Agent B 做 Git 教學
```

每個 worktree 各自開發完後，需要把成果 **commit 到各自的分支**，再 **merge 回主分支**。

---

## 完整操作流程

### Step 1：在各 worktree 分別 commit

**進入 cursor worktree，commit Cursor 教學：**

```bash
cd my-project-cursor

git add cursor/commands/
git commit -m "docs(cursor): add Cursor commands tutorial series"
```

**進入 git worktree，commit Git 教學：**

```bash
cd my-project-git

git add git/branch/
git commit -m "docs(git): add Git branch naming and strategy tutorial"
```

> 💡 **Commit message 格式**：`type(scope): subject`
> - `docs` = 文件變更
> - `(cursor)` / `(git)` = 影響範圍
> - subject 用小寫祈使句

---

### Step 2：回到主工作區，merge 各分支

```bash
cd my-project    # 回到主工作區（dev 分支）

# merge cursor 分支
git merge cursor --no-ff -m "Merge branch 'cursor' into dev"

# merge git 分支
git merge git --no-ff -m "Merge branch 'git' into dev"
```

> 💡 **`--no-ff`（No Fast Forward）的意思**：
> 強制產生一個 merge commit，讓 Git 歷史明確記錄「這段改動來自哪個分支」。
> 不加的話，Git 可能會直接把 commit 接上去，看不出曾經分叉過。

---

### Step 3：確認歷史

```bash
git log --oneline --graph
```

正常的歷史長這樣：

```
*   b9f85c6 Merge branch 'git' into dev
|\
| * 0c8ea91 docs(git): add Git branch naming and strategy tutorial
* |   5d6b3a8 Merge branch 'cursor' into dev
|\ \
| * 8c74e0c docs(cursor): add Cursor commands tutorial series
|/
* 858753e 之前的 commit...
```

樹狀結構清楚顯示：每個功能從哪個分支來、在哪裡合入。

---

## 關鍵觀念：所有 worktree 共用同一個 `.git`

```
my-project/
├── .git/           ← 只有一個！所有 worktree 都共用
├── ...

my-project-cursor/  ← 沒有自己的 .git，連結到上方
my-project-git/     ← 沒有自己的 .git，連結到上方
```

這代表：
- 在任一 worktree commit，其他 worktree 的 `git log` 都能看到
- 在主工作區就能直接 merge 其他 worktree 的分支，**不需要 `git pull` 或 `git fetch`**

---

## 常見問題

### 不小心在錯誤的 worktree 新增了檔案怎麼辦？

先確認你在哪個分支：

```bash
git status   # 會顯示目前分支名稱
```

如果是「未追蹤的檔案」（untracked），可以手動複製到正確的 worktree：

```bash
cp -r ./錯誤的資料夾/ ../正確的worktree/對應路徑/
rm -rf ./錯誤的資料夾/   # 從原本的地方移除
```

---

### Cursor 自動建的 worktree 是什麼？

執行 `git worktree list` 時，你可能會看到：

```
/Users/you/.cursor/worktrees/my-project/ewg   858753e (detached HEAD)
/Users/you/.cursor/worktrees/my-project/fot   858753e (detached HEAD)
```

這是 **Cursor IDE 自動建立的臨時 worktree**，每次開啟 Agent 對話時建立，用完後可以清理：

```bash
git worktree prune   # 清理失效的 worktree 記錄
```

---

## 指令速查

| 操作 | 指令 |
|------|------|
| 在 worktree 暫存並 commit | `git add <路徑>` → `git commit -m "..."` |
| merge 分支並保留歷史 | `git merge <分支名> --no-ff` |
| 查看 commit 樹狀圖 | `git log --oneline --graph` |
| 清理失效 worktree 記錄 | `git worktree prune` |

---

## 下一步

👉 回到 [00-overview.md](./00-overview.md) 複習完整流程
