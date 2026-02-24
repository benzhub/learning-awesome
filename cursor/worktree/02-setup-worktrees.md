# 02 — 實際建立 Worktree

## 場景設定

假設你有一個叫 `my-app` 的專案，你同時要：
- 開發 `feature/auth`（登入功能）
- 開發 `feature/payment`（付款功能）
- 修復 `bugfix/cart`（購物車 Bug）

---

## Step 1：確認你的 Git 倉庫

```bash
cd ~/projects/my-app
git status        # 確認在 main branch
git log --oneline -5  # 確認有 commit 歷史
```

---

## Step 2：建立 Worktree 資料夾

**建議的資料夾結構：**
```
~/projects/
├── my-app/                  ← 主倉庫（不要動這個）
├── my-app--feature-auth/    ← worktree 1
├── my-app--feature-payment/ ← worktree 2
└── my-app--bugfix-cart/     ← worktree 3
```

> 命名慣例：`專案名--branch名`，用 `--` 分隔，一眼就能看出對應關係

---

## Step 3：建立 Worktree

```bash
# 在主倉庫資料夾內執行

# 建立 worktree 1（同時建立新 branch）
git worktree add ../my-app--feature-auth -b feature/auth

# 建立 worktree 2
git worktree add ../my-app--feature-payment -b feature/payment

# 建立 worktree 3
git worktree add ../my-app--bugfix-cart -b bugfix/cart
```

執行後，每個指令會：
1. 建立對應的資料夾
2. 建立新的 branch
3. 把程式碼 checkout 到那個資料夾

---

## Step 4：確認建立成功

```bash
git worktree list
```

輸出範例：
```
/Users/you/projects/my-app                  abc1234 [main]
/Users/you/projects/my-app--feature-auth    abc1234 [feature/auth]
/Users/you/projects/my-app--feature-payment abc1234 [feature/payment]
/Users/you/projects/my-app--bugfix-cart     abc1234 [bugfix/cart]
```

---

## Step 5：在各個 Worktree 安裝依賴

如果你的專案需要安裝 npm/pip 等依賴，**每個 worktree 都要各自安裝**：

```bash
# 進入 worktree 1
cd ../my-app--feature-auth
npm install  # 或 pip install -r requirements.txt

# 進入 worktree 2
cd ../my-app--feature-payment
npm install
```

> `node_modules` 不會共用，每個 worktree 都是獨立的工作環境

---

## 刪除 Worktree（完成後清理）

```bash
# 方法一：從主倉庫刪除
git worktree remove ../my-app--feature-auth

# 方法二：如果已經 merge 完，也可以
git worktree prune  # 清理已刪除的 worktree 記錄
```

---

## 常見問題

**Q：建立時報錯 `fatal: 'feature/auth' is already checked out`？**
→ 這個 branch 已經在其他 worktree 或主倉庫中使用，換個 branch 名稱

**Q：worktree 資料夾可以放在哪裡？**
→ 任何地方都可以，建議放在主倉庫的**同一層目錄**，方便管理

---

> 下一步：`03-cursor-multi-agent.md` — 在 Cursor 中開啟多視窗 Agent
