# 02 — 什麼是 Widget？

## 一句話

> Widget 是 Elementor 編輯器裡**可以拖放的功能積木**，每個 Widget 有自己的設定面板和輸出的 HTML。

---

## 從使用者角度看 Widget

當你在 Elementor 編輯器左側看到「標題」「文字」「圖片」，那每一個都是一個 Widget。

拖到畫面上之後，左側面板會出現這個 Widget 的**設定選項**（文字內容、字型大小、顏色…），右側**即時更新預覽**。

---

## Widget 能做什麼？

一個 Widget 負責兩件事：

| 職責 | 說明 | 在哪裡設定 |
|------|------|-----------|
| **Controls（設定項）** | 讓使用者填寫的欄位 | 左側面板 |
| **Render（輸出）** | 根據設定產生的 HTML | 右側預覽 / 前台頁面 |

> 簡單說：使用者在左邊填寫 → 右邊馬上顯示結果。

---

## Widget 和 WordPress Block 有什麼不同？

很多人會搞混 Elementor Widget 和 Gutenberg Block：

| 比較項目 | Elementor Widget | Gutenberg Block |
|----------|-----------------|-----------------|
| 編輯器 | Elementor | WordPress 內建 Block 編輯器 |
| 撰寫語言 | **PHP**（後端渲染） | **JavaScript / React**（前端渲染） |
| 設定方式 | 左側面板（Control） | 右側 Inspector 面板 |
| 學習門檻 | 只要懂 PHP | 要懂 React + WP Block API |

> 如果你比較熟 PHP，Elementor Widget 會比 Gutenberg Block 親切很多。

---

## 一個 Widget 的生命週期

```
使用者拖放 Widget 到頁面
        ↓
Elementor 呼叫這個 Widget 的 Controls
        ↓
左側面板顯示設定欄位（文字輸入、顏色選擇…）
        ↓
使用者填寫設定
        ↓
Elementor 呼叫這個 Widget 的 render()
        ↓
輸出 HTML → 顯示在右側預覽 + 前台頁面
```

---

## 常見的內建 Widget 範例

| Widget 名稱 | 功能 |
|-------------|------|
| Heading | 輸出 `<h1>`～`<h6>` 標題 |
| Text Editor | 富文本編輯器 |
| Image | 顯示圖片 |
| Button | 可設定連結的按鈕 |
| Video | 嵌入 YouTube / Vimeo |
| Icon | 顯示圖示 |

這些都是 PHP 寫成的 Widget，和你要學寫的是一模一樣的東西。

---

## 小結

- Widget = 可拖放的積木，有**設定面板（Controls）**和**輸出（Render）**
- 和 Gutenberg Block 最大差異：**Elementor Widget 用 PHP 寫**
- 每個 Widget 的核心工作：接收設定值 → 輸出 HTML

---

> 下一步：`03-widget-structure.md` — 看看一個 Widget 的程式碼長什麼樣
