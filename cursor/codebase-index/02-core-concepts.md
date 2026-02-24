# Codebase Indexing 核心概念

## 概念一：索引不是全文檢索，而是語義檢索
Codebase Indexing 會把程式碼切成可理解的片段，轉成語義向量。你提問時，系統會用語義相似度召回片段，再交給模型生成答案。

```text
# 對照測試（可直接貼到 Chat）
@Codebase
請找「權限檢查」相關邏輯。

@Codebase
請找「role guard / permission middleware / ACL」相關邏輯。
```

> 💡 第二種通常更準，因為它同時給了業務語意與常見技術詞彙。

## 概念二：召回品質取決於「問題清晰度」
同一個索引，提問方式不同，結果會差很多。高品質提問通常包含：目標、範圍、輸出格式。

```text
# 模糊問法
@Codebase 登入在哪？

# 清晰問法
@Codebase
請找出登入流程，包含：
1) API 入口
2) 密碼驗證
3) token/session 產生
4) 失敗分支
輸出每一步的檔案路徑。
```

> 💡 若回答不完整，不一定是索引錯，先優化問題描述再判斷。

## 概念三：`@Codebase` 與 `@檔案` 要搭配用
最佳流程通常是先用 `@Codebase` 找候選檔案，再切換成 `@檔案` 做精讀與修改建議。這樣既快又準。

```text
# 第一步：廣搜
@Codebase 找出退款流程入口與核心邏輯。

# 第二步：精讀
@src/services/refund.service.ts
解釋這個服務的例外處理路徑，並指出可重構點。
```

## 下一步
把這些概念轉成可重複的日常工作流 → [03-common-patterns.md](03-common-patterns.md)
