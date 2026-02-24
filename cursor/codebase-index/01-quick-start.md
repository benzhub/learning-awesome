# Codebase Indexing 快速上手

## 前置條件
- Cursor 專案已開啟且檔案已完成索引（建議看到 100%）
- 你已能在 Chat 使用 `@` 引用上下文
- 專案至少有幾個可讀程式檔（避免空倉庫造成無結果）

> ⚠️ 若索引未完成，回答可能遺漏關鍵檔案，先等待索引結束再問。

## 最小範例（10 分鐘）
把以下提示詞直接貼進 Cursor Chat：

```text
@Codebase
請幫我找出「訂單建立」相關程式碼，並依序回答：
1) API 入口
2) Service/UseCase
3) 資料庫存取層
4) 錯誤處理位置
輸出格式：用編號條列 + 每一項附檔案路徑。
```

**預期輸出：**

```text
1. API 入口：src/api/order.controller.ts
2. Service：src/services/order.service.ts
3. Repository：src/repositories/order.repository.ts
4. 錯誤處理：src/middleware/error-handler.ts
```

## 常見錯誤與修正
- ❌ 問題太大：`@Codebase 介紹整個專案`  
  ✅ 改為縮範圍：先問單一流程（登入、下單、通知）
- ❌ 只有關鍵字：`@Codebase auth`  
  ✅ 改為任務導向：`找出登入驗證流程與 token 產生位置`
- ❌ 一次要太多輸出格式  
  ✅ 先要路徑清單，再追問細節

## 下一步
理解為什麼同一句話有時找得到、有時不精準 → [02-core-concepts.md](02-core-concepts.md)
