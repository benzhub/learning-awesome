# Cursor 工作流模式與模型設定指南

## 快速解答

**Cursor 目前不支援** 透過 `.cursor` 設定檔自動為 Plan / Agent / Debug 三種模式指定不同的 AI 模型。  
模型選擇是 Chat 介面中**手動切換**的 per-session 設定。

但你可以透過以下方法達到**接近自動化**的效果：

1. [自訂 Slash Commands（最推薦）](#自訂-slash-commands)
2. [Rules 檔案引導模型行為](#rules-引導行為)
3. [模式切換最佳實踐](#手動切換的最佳實踐)

---

## 四種模式一覽

| 模式 | 最適合 | 工具權限 | 推薦模型 |
|------|--------|---------|---------|
| **Agent** | 複雜功能開發、多檔案重構 | 全開（讀寫＋執行指令） | Claude Sonnet（速度與品質平衡） |
| **Ask** | 探索程式碼、提問學習 | 唯讀（搜尋工具） | 較快的模型（Gemini Flash） |
| **Plan** | 架構設計、需要先規劃再動工 | 全開（但先產出計畫） | Claude Opus（深度推理） |
| **Debug** | 難以重現的 Bug、效能問題 | 全開＋debug server | Claude Opus（假設推理能力強） |

---

## 模式切換方式

### 鍵盤快捷鍵

```
Cmd+.          → 快速切換模式（輪換）
Shift+Tab      → 從 Agent 切換到 Plan 模式
```

### 模式選擇器

在 Chat 輸入框左側點選下拉選單，手動選擇模式。

---

## 自訂 Slash Commands

這是目前**最接近「自動設定模式行為」**的方法。

建立 `.cursor/commands/` 資料夾，為每個工作情境建立對應的指令檔。

### 目錄結構

```
.cursor/
└── commands/
    ├── plan-feature.md      # 規劃新功能
    ├── debug-issue.md       # 除錯模式
    ├── code-review.md       # 程式碼審查
    └── refactor.md          # 重構
```

### 範例：`plan-feature.md`

```markdown
切換到 Plan 模式後執行此命令。

你的任務是為新功能制定詳細計畫：

1. **需求分析**：先問我 3 個最關鍵的問題，確認需求邊界
2. **現有程式碼探索**：掃描相關目錄，理解現有架構
3. **產出計畫**：
   - 需要新建/修改哪些檔案
   - 技術選型建議（含優缺點）
   - 潛在風險點
   - 實作步驟（按優先序排列）
4. **等待確認**：計畫確認後再開始寫程式

請用繁體中文回覆，保持邏輯嚴謹。
```

**使用方式：** 在 Chat 輸入 `/plan-feature 實作使用者登入功能`

---

### 範例：`debug-issue.md`

```markdown
切換到 Debug 模式後執行此命令。

你是一位系統性除錯專家，請依以下流程處理：

1. **資訊收集**：詢問預期行為 vs 實際行為、錯誤訊息、重現步驟
2. **建立假設**：提出 3-5 個可能的根本原因，按可能性排序
3. **加入日誌**：加入最少量的 log 語句來驗證假設
4. **等待重現**：請我執行程式並提供日誌輸出
5. **分析 → 精準修復**：根據日誌定位問題，做最小範圍的修改
6. **清理**：確認修復後移除所有 debug log

不要在沒有日誌證據的情況下猜測修復方案。
```

**使用方式：** 在 Chat 輸入 `/debug-issue 購物車在加入第二件商品後總價計算錯誤`

---

## Rules 引導行為

雖然 Rules 無法自動切換模型，但可以讓 AI **在不同情境下表現得更像特定模式的行為**。

### 建立模式對應的 Rules

```
.cursor/
└── rules/
    ├── planning.mdc     # 規劃任務時觸發
    ├── debugging.mdc    # 除錯任務時觸發
    └── coding.mdc       # 寫程式時觸發
```

### `planning.mdc` 範例

```markdown
---
description: 當用戶要求規劃、設計架構、或討論技術方案時套用
alwaysApply: false
---

進入規劃模式：
- 先問清楚需求，不要假設
- 列出多個技術方案並比較優缺點
- 用 Mermaid 圖表視覺化架構
- 明確標示出你不確定的地方
- 計畫確認前不要寫任何程式碼
```

### `debugging.mdc` 範例

```markdown
---
description: 當用戶回報 Bug、錯誤或非預期行為時套用
alwaysApply: false
---

進入除錯模式：
- 先重現問題，不要直接修改程式
- 系統性建立假設清單
- 加 log 驗證假設，不要靠直覺猜
- 每次只驗證一個假設
- 找到根本原因後才修復，且修改範圍要最小
- 絕對不要在沒有確認根本原因的情況下做「防禦性修改」
```

---

## 手動切換的最佳實踐

既然需要手動選模型，建立一套**習慣化的流程**最有效：

### 推薦工作流程

```
任務開始
  │
  ├─ 新功能 / 不確定做法？
  │   → 切換 Plan 模式 → 選 Claude Opus → /plan-feature
  │
  ├─ 已知做法，直接開工？
  │   → 切換 Agent 模式 → 選 Claude Sonnet → 直接下指令
  │
  ├─ 有 Bug 要找原因？
  │   → 切換 Debug 模式 → 選 Claude Opus → /debug-issue
  │
  └─ 只是要問問題？
      → 切換 Ask 模式 → 選 Gemini Flash（省 token）
```

### 模型選擇速查

| 場景 | 模型 | 原因 |
|------|------|------|
| Plan / Debug（需要深度推理） | Claude Opus | 假設推理、架構思考能力最強 |
| Agent 日常開發 | Claude Sonnet | 速度與品質最佳平衡點 |
| Ask 純問答 | Gemini Flash | 速度快、成本低，問答夠用 |
| 大型程式碼庫分析 | 任意模型 + Max Mode | 擴展 context 視窗到最大 |

---

## 未來展望

Cursor 社群有呼聲希望支援「**per-mode 預設模型設定**」，讓你在 `.cursor/settings.json` 中設定類似：

```json
{
  "modeModels": {
    "agent": "claude-sonnet",
    "plan": "claude-opus",
    "debug": "claude-opus",
    "ask": "gemini-flash"
  }
}
```

這個功能**尚未推出**，可以在 [Cursor Forum](https://forum.cursor.com) 追蹤進度或 +1 投票支持。

---

## 完整 `.cursor` 設定建議

整合上述所有設定後，你的專案結構應該像這樣：

```
.cursor/
├── commands/
│   ├── plan-feature.md
│   ├── debug-issue.md
│   ├── code-review.md
│   └── refactor.md
└── rules/
    ├── planning.mdc
    ├── debugging.mdc
    └── coding.mdc
```

這個設定雖然不能**自動切換模型**，但能讓每個工作情境都有對應的：
- 標準化的工作流程（Commands）
- 一致的 AI 行為指引（Rules）

---

> 上一篇：`04-parallel-workflow.md` — 並行開發實戰  
> 下一篇：`05-best-practices.md` — 整體最佳實踐
