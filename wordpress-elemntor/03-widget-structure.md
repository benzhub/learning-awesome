# 03 — Widget 的程式碼結構

## 一句話

> 一個 Widget 就是**繼承 `Widget_Base` 的 PHP 類別**，你需要實作幾個固定的方法，Elementor 就會自動幫你串起來。

---

## 最精簡的 Widget 骨架

```php
<?php
use Elementor\Widget_Base;
use Elementor\Controls_Manager;

class My_Widget extends Widget_Base {

    // 1. Widget 的唯一識別碼
    public function get_name() {
        return 'my-widget';
    }

    // 2. 顯示在編輯器面板的名稱
    public function get_title() {
        return '我的 Widget';
    }

    // 3. 顯示的圖示（Font Awesome class）
    public function get_icon() {
        return 'eicon-text';
    }

    // 4. 歸類在哪個分類
    public function get_categories() {
        return [ 'general' ];
    }

    // 5. 設定項目（Controls）
    protected function register_controls() {
        // 之後在這裡加設定欄位
    }

    // 6. 輸出 HTML
    protected function render() {
        // 之後在這裡輸出畫面
    }
}
```

---

## 六個方法各自的職責

| 方法 | 必填 | 說明 |
|------|------|------|
| `get_name()` | ✅ | Widget 的**唯一 ID**，不能重複，用小寫加連字號 |
| `get_title()` | ✅ | 顯示在面板的**名稱**，使用者看到的 |
| `get_icon()` | 建議填 | 面板上的**圖示**，用 Elementor Icon 或 Font Awesome |
| `get_categories()` | 建議填 | 歸類到哪個**分類** |
| `register_controls()` | 功能需要 | 定義左側面板的**設定欄位** |
| `render()` | ✅ | 輸出**實際的 HTML** |

---

## 繼承關係

```
Elementor\Base_Object
    └── Elementor\Base_Widget
            └── Elementor\Widget_Base   ← 你繼承這個
                    └── My_Widget       ← 你寫的 Widget
```

你只需要繼承 `Widget_Base`，其他的 Elementor 都幫你處理好了。

---

## 常用的圖示（`get_icon`）

```php
// Elementor 自帶圖示（建議優先用）
'eicon-text'
'eicon-image'
'eicon-button'
'eicon-star'

// Font Awesome 圖示（也可以用）
'fa fa-user'
'fa fa-envelope'
```

在 Elementor Icons Reference 頁面可以查到所有可用圖示。

---

## 常用的分類（`get_categories`）

| 分類 ID | 說明 |
|---------|------|
| `general` | 一般（預設） |
| `basic` | 基本 |
| `pro-elements` | Pro 版專屬（需 Elementor Pro） |

> 你也可以用 `\Elementor\Plugin::$instance->elements_manager->add_category()` 自訂分類。

---

## 小結

- Widget 是繼承 `Widget_Base` 的 PHP 類別
- 必須實作：`get_name()`、`get_title()`、`render()`
- `register_controls()` 定義設定欄位
- `render()` 輸出 HTML

---

> 下一步：`04-create-first-widget.md` — 實際動手建立並註冊第一個 Widget
