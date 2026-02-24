# Git Worktree 基本操作

## 新增 Worktree

### 從現有分支建立

```bash
git worktree add <目錄路徑> <分支名稱>
```

**範例**：

```bash
# 在上層目錄建立一個 hotfix 的工作區
git worktree add ../my-project-hotfix hotfix/login-bug
```

執行後，`../my-project-hotfix` 資料夾會自動建立，並已 checkout 到 `hotfix/login-bug` 分支。

---

### 同時建立新分支

加上 `-b` 參數，可以在建立 worktree 的同時新建分支：

```bash
git worktree add -b <新分支名稱> <目錄路徑> <起始點>
```

**範例**：

```bash
# 從 main 建立新分支 feature/dark-mode，並開一個新資料夾
git worktree add -b feature/dark-mode ../my-project-dark-mode main
```

---

## 列出所有 Worktree

```bash
git worktree list
```

**輸出範例**：

```
/Users/you/my-project           abc1234 [main]
/Users/you/my-project-hotfix    def5678 [hotfix/login-bug]
/Users/you/my-project-dark-mode ghi9012 [feature/dark-mode]
```

第一行永遠是主 worktree，後面是連結 worktree。

---

## 刪除 Worktree

### 步驟一：刪除資料夾

```bash
rm -rf ../my-project-hotfix
```

### 步驟二：清理 Git 紀錄

```bash
git worktree prune
```

> `prune` 會自動清除已不存在資料夾的 worktree 紀錄。

---

### 一行完成（Git 2.17+）

```bash
git worktree remove ../my-project-hotfix
```

> ⚠️ 如果資料夾內有未 commit 的改動，指令會拒絕執行。加上 `--force` 可強制刪除。

---

## 移動 Worktree 路徑

```bash
git worktree move ../my-project-hotfix ../hotfix-temp
```

---

## 快速記憶

| 指令 | 說明 |
|------|------|
| `git worktree add <路徑> <分支>` | 新增 worktree |
| `git worktree add -b <新分支> <路徑> <起始點>` | 新增 worktree 並建新分支 |
| `git worktree list` | 列出所有 worktree |
| `git worktree remove <路徑>` | 刪除 worktree |
| `git worktree prune` | 清理失效的 worktree 紀錄 |
| `git worktree move <舊路徑> <新路徑>` | 移動 worktree |

---

## 下一步

👉 [03-use-cases.md](./03-use-cases.md) — 實際使用情境示範
