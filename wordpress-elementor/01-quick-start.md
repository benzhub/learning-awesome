# 快速上手 — 10 分鐘建立第一個 Widget

## 前置條件

- WordPress 已安裝並啟用
- Elementor 外掛已安裝並啟用
- 可編輯 `wp-content/plugins/` 目錄

---

## 最小範例

只用最核心的 API，建立一個會輸出「Hello, Elementor!」的 Widget。

### 步驟一：建立外掛結構

```
wp-content/plugins/
└── my-elementor-widgets/
    ├── my-elementor-widgets.php   ← 外掛主檔
    └── widgets/
        └── hello-widget.php       ← Widget 類別
```

### 步驟二：外掛主檔

```php
<?php
/**
 * Plugin Name: My Elementor Widgets
 * Description: 自訂 Elementor Widget 練習
 * Version: 1.0.0
 * Requires Plugins: elementor
 */

if ( ! defined( 'ABSPATH' ) ) exit;

add_action( 'elementor/widgets/register', function( $widgets_manager ) {
    require_once __DIR__ . '/widgets/hello-widget.php';
    $widgets_manager->register( new Hello_Widget() );
});
```

### 步驟三：Widget 類別

```php
<?php
use Elementor\Widget_Base;

class Hello_Widget extends Widget_Base {

    public function get_name() {
        return 'hello-widget';
    }

    public function get_title() {
        return 'Hello Widget';
    }

    public function get_icon() {
        return 'eicon-code';
    }

    public function get_categories() {
        return [ 'general' ];
    }

    protected function register_controls() {
        // 暫不加入設定
    }

    protected function render() {
        ?>
        <div class="my-hello-widget">
            <p>Hello, Elementor!</p>
        </div>
        <?php
    }
}
```

---

## 啟用與驗證

1. 後台 → **外掛** → 啟用「My Elementor Widgets」
2. 開啟任一頁面 → **用 Elementor 編輯**
3. 左側搜尋「Hello」→ 拖放「Hello Widget」到畫布

**預期輸出：** 畫面上出現 `Hello, Elementor!` 文字。

---

## 常見錯誤

| 現象 | 解法 |
|------|------|
| 外掛沒出現在後台 | 確認 `Plugin Name:` 有寫、檔名正確 |
| Widget 找不到 | 確認 Elementor 已啟用、Hook 為 `elementor/widgets/register` |
| 啟用外掛報錯 | 開 WP Debug 模式，檢查 PHP 語法 |
| 畫面空白 | 確認 `render()` 有輸出 HTML |

> ⚠️ 一定要在 `elementor/widgets/register` 之後才註冊，不能太早。

---

## 下一步

理解這個範例背後的原理 → [02-core-concepts.md](02-core-concepts.md)
