# 05 — 最佳實踐與常見陷阱

## 最佳實踐

### 1. 任務拆分是關鍵

**好的任務拆分：** 讓每個 Agent 負責**獨立、邊界清楚**的部分

```
✅ 好的拆分：
- Agent A → src/auth/      （只動這個資料夾）
- Agent B → src/products/  （只動這個資料夾）
- Agent C → src/cart/      （只動這個資料夾）

❌ 壞的拆分：
- Agent A → 建立 API
- Agent B → 建立 API 的測試  ← 可能同時動到同一個文件
```

---

### 2. 給 Agent 明確的邊界

在任務描述中加上「禁止事項」：

```
請實作購物車功能
- 只修改 src/cart/ 資料夾內的文件
- 不要修改 package.json
- 不要更動任何 auth/ 或 products/ 的文件
- 如果需要共用的 helper，告訴我，不要自己建立
```

---

### 3. 使用 Cursor Rules 統一規範

在主倉庫建立 `.cursor/rules/project.mdc`，讓所有視窗共用相同規則：

```markdown
---
alwaysApply: true
---

# 專案規範
- 使用 TypeScript
- 函數命名用 camelCase
- 檔案命名用 kebab-case
- 每個 API 都要有錯誤處理
- 不要使用 any 型別
```

由於 worktree 共用同一個 `.git`（和 `.cursor/`），所有視窗都會套用這個規則。

---

### 4. 控制並行數量

不要同時開太多 Agent：

| 工作方式 | 建議並行數量 |
|---------|-------------|
| 初學者 | 2 個視窗 |
| 熟練後 | 3-4 個視窗 |
| 最多 | 5-6 個（超過反而來不及 review）|

---

### 5. Commit 頻率

讓 Agent 在完成每個小步驟後 commit：

```
任務：建立用戶 API
請每完成一個 endpoint 就 commit 一次，commit message 要清楚描述做了什麼
```

這樣如果 Agent 走錯方向，可以輕易回滾。

---

## 常見陷阱

### 陷阱 1：讓 Agent 修改共用文件

```
問題：兩個 Agent 同時修改 utils/helpers.ts
→ 可能產生衝突

解法：把共用文件列為「禁區」，讓一個 Agent 專門負責
```

### 陷阱 2：worktree 的 node_modules 沒裝

```bash
# 症狀：Agent 執行測試時報錯 "Cannot find module"
# 解法：進入 worktree 手動安裝
cd ~/projects/my-app--feature-auth
npm install
```

### 陷阱 3：忘記 Agent 還在跑就去 merge

```
問題：Agent 還沒完成就把 branch merge 回 main
→ 可能 merge 到不完整的程式碼

解法：確認 Agent 說「完成」且你 review 過後才 merge
```

### 陷阱 4：Worktree branch 名衝突

```bash
# 症狀：git worktree add 報錯 "already checked out"
# 解法：確認 branch 沒有被其他 worktree 使用
git worktree list  # 查看所有 worktree 和對應 branch
```

---

## 快速參考卡

```bash
# 建立 worktree
git worktree add ../專案名--branch名 -b branch/名稱

# 查看所有 worktree
git worktree list

# 刪除 worktree
git worktree remove ../專案名--branch名

# 清理失效的 worktree 記錄
git worktree prune
```

---

## 何時不要用 Worktree 多 Agent？

- 任務之間有**強依賴關係**（A 完成後 B 才能開始）
- 專案太小（只需要修改 1-2 個文件）
- 你對整個任務還不清楚（先用單個 Agent 探索）

---

## 總結：三個核心原則

1. **獨立**：拆分成互不依賴的任務
2. **邊界**：讓每個 Agent 知道自己的範圍
3. **監控**：你是指揮官，定期巡視各視窗

---

> 恭喜完成課程！實際動手試試看，從 2 個 worktree 開始練習。
