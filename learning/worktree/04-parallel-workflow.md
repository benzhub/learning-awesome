# 04 — 並行開發實戰流程

## 完整流程總覽

```
1. 規劃任務（拆分成互不依賴的子任務）
        ↓
2. 建立 Worktree（每個任務一個 worktree）
        ↓
3. 開啟多個 Cursor 視窗（每個視窗對應一個 worktree）
        ↓
4. 同時啟動多個 Agent（各自執行任務）
        ↓
5. 輪流監控進度（在視窗間切換）
        ↓
6. 各自 Review & Merge（完成後合併回 main）
```

---

## 實戰範例：電商網站後端開發

### 任務拆分（關鍵步驟！）

拆分的原則：**任務之間不互相依賴**

```
❌ 錯誤拆法（有依賴關係）：
任務 A：建立 User 資料庫 schema
任務 B：用 User schema 建立登入 API   ← B 依賴 A，不能並行

✅ 正確拆法（互相獨立）：
任務 A：建立商品列表 API
任務 B：建立購物車 API
任務 C：修復搜尋功能的 Bug           ← 三個互相獨立，可以並行
```

---

### Phase 1：準備（5 分鐘）

```bash
# 在主倉庫建立三個 worktree
cd ~/projects/my-shop

git worktree add ../my-shop--products -b feature/product-api
git worktree add ../my-shop--cart     -b feature/cart-api
git worktree add ../my-shop--bugfix   -b bugfix/search

# 各自安裝依賴
cd ../my-shop--products && npm install
cd ../my-shop--cart     && npm install
cd ../my-shop--bugfix   && npm install
```

---

### Phase 2：啟動 Agent（各自 2 分鐘）

**開啟三個 Cursor 視窗：**
```bash
cursor ~/projects/my-shop--products
cursor ~/projects/my-shop--cart
cursor ~/projects/my-shop--bugfix
```

**在各視窗給 Agent 任務：**

視窗 1（products）：
```
請建立商品列表 API：
- GET /api/products - 取得所有商品（支援分頁）
- GET /api/products/:id - 取得單一商品
- 使用 src/models/Product.js 已有的 model
- 在 src/routes/products.js 新增路由
- 完成後寫簡單的使用說明
```

視窗 2（cart）：
```
請建立購物車 API：
- POST /api/cart/add - 加入購物車
- GET /api/cart - 取得購物車內容
- DELETE /api/cart/:itemId - 移除商品
- 在 src/routes/cart.js 新增路由
- 不要動 products 相關的任何文件
```

視窗 3（bugfix）：
```
搜尋功能有 Bug：搜尋中文關鍵字時回傳空結果
- 問題應該在 src/services/search.js
- 請找出並修復問題
- 修復後說明原因
```

---

### Phase 3：監控與介入（進行中）

**輪流查看各視窗的狀態：**

| 狀態 | 你需要做的 |
|------|-----------|
| Agent 正在執行 | 可以去看其他視窗 |
| Agent 問問題 | 回答後讓它繼續 |
| Agent 遇到錯誤 | 看錯誤訊息，給指示 |
| Agent 完成 | Review 程式碼 |

**有效的監控節奏（範例）：**
```
09:00 → 啟動三個 Agent
09:05 → 視窗 1 進度（正在寫 route）→ 切到視窗 2
09:05 → 視窗 2 進度（正在建 model）→ 切到視窗 3
09:05 → 視窗 3 進度（已找到 Bug）→ 回覆問題
09:10 → 視窗 3 完成 → 開始 review code
09:15 → 視窗 1 完成 → 開始 review code
09:20 → 視窗 2 完成 → 開始 review code
```

---

### Phase 4：Merge 回 Main

```bash
# 切回主倉庫
cd ~/projects/my-shop

# Merge 第一個完成的功能
git merge feature/product-api
git merge feature/cart-api
git merge bugfix/search

# 清理 worktree
git worktree remove ../my-shop--products
git worktree remove ../my-shop--cart
git worktree remove ../my-shop--bugfix

# 刪除已 merge 的 branch
git branch -d feature/product-api feature/cart-api bugfix/search
```

---

## 時間對比

| 方式 | 估計時間 |
|------|---------|
| 傳統串行（自己寫） | 4-6 小時 |
| 單個 Agent | 2-3 小時 |
| Worktree + 3 個 Agent | 45-90 分鐘 |

---

> 下一步：`05-best-practices.md` — 最佳實踐與常見陷阱
