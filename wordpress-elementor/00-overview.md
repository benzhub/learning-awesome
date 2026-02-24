# Elementor 自訂 Widget — 一句話介紹

> **Elementor** 是 WordPress 的拖放式頁面編輯器；**自訂 Widget** 讓你可以用 PHP 打造專屬的積木，在編輯器裡拖放使用。

---

## 這是什麼

- **解決什麼問題**：內建 Widget 不夠用時，用 PHP 擴充可拖放的區塊
- **和 Gutenberg Block 的差異**：Elementor Widget 用 **PHP 後端渲染**，不需 React；學習門檻較低

---

## 適用場景

| 適合 | 不適合 |
|------|--------|
| 整合特殊功能（計算機、表單、API 資料） | 需要複雜前端互動（用 JS 框架較佳） |
| 把重複 HTML 包成可重用積木 | 純靜態頁面（內建 Widget 已夠用） |
| 客戶需在編輯器直接設定客製區塊 | 完全不懂 PHP |

---

## 學習地圖

| 文件 | 主題 | 約需時間 |
|------|------|----------|
| [01-quick-start](01-quick-start.md) | 10 分鐘建立第一個 Widget | 10 分鐘 |
| [02-core-concepts](02-core-concepts.md) | Widget 結構、Controls、Render | 15 分鐘 |
| [03-common-patterns](03-common-patterns.md) | 常用 Control 類型與實戰模式 | 20 分鐘 |
| [04-advanced](04-advanced.md) | 編輯器預覽、動態內容、Hooks | 15 分鐘 |
| [05-best-practices](05-best-practices.md) | 安全性、命名、避坑指南 | 10 分鐘 |

---

## 前置需求

- WordPress（本機或遠端皆可）
- Elementor 外掛（**免費版即可**，自訂 Widget 不需 Pro）
- 基本 PHP 語法
- 會建立 WordPress 外掛（`wp-content/plugins/` 下建資料夾 + PHP 檔）

---

## 核心公式

> 一個 Widget = **登記自己** + **定義 Controls** + **輸出 HTML**

記住這三件事，其他都是細節。

---

> 從 [01-quick-start.md](01-quick-start.md) 開始！
