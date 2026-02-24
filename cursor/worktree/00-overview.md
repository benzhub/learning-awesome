# Cursor Worktree 多 Agent 開發 — 課程總覽

## 你會學到什麼

用 **Git Worktree + Cursor 多視窗** 讓多個 AI Agent 同時幫你開發不同功能，大幅提升開發速度。

---

## 核心概念（先看這個）

```
一個 Git 倉庫
├── 主視窗 (main branch)   → Agent A 做功能 X
├── worktree-1 (feature/A) → Agent B 做功能 Y
└── worktree-2 (feature/B) → Agent C 修 Bug Z
```

**傳統開發**：一個人做完 A → 再做 B → 再做 C（串行）

**Worktree 多 Agent**：三個 Agent 同時做 A、B、C（並行）

---

## 課程地圖

| 文件 | 主題 | 需要時間 |
|------|------|----------|
| `01-git-worktree-basics.md` | Git Worktree 是什麼 | 5 分鐘 |
| `02-setup-worktrees.md` | 實際建立 Worktree | 10 分鐘 |
| `03-cursor-multi-agent.md` | Cursor 多視窗 Agent 設定 | 10 分鐘 |
| `04-parallel-workflow.md` | 並行開發實戰流程 | 15 分鐘 |
| `05-best-practices.md` | 最佳實踐與常見問題 | 5 分鐘 |

---

## 為什麼要這樣做？

### 痛點：傳統方式的問題
- 同一時間只能在一個 branch 上工作
- 切換 branch 時要 stash 或 commit 未完成的工作
- AI Agent 做事的時候你只能等它

### 解法：Worktree + 多 Agent
- 每個 worktree 是**獨立的工作目錄**，不互相干擾
- 每個 Cursor 視窗開一個 worktree → 各自有獨立 Agent
- Agent 在跑的時候，你可以去看另一個視窗的進度

---

## 前置需求

- Git 2.5+（`git --version` 確認）
- Cursor IDE（最新版）
- 基本的 Git 操作知識（commit、branch）

---

> 準備好了嗎？從 `01-git-worktree-basics.md` 開始！
