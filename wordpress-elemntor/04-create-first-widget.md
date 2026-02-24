# 04 — 建立第一個 Widget

## 一句話

> 建一個外掛資料夾、寫兩個 PHP 檔案（外掛主檔 + Widget 類別），啟用後就能在 Elementor 面板看到你的 Widget。

---

## 目標

做完這個章節，你會有一個能在 Elementor 拖放的 Widget，它會輸出：

```html
<div class="my-hello-widget">
  <p>Hello, Elementor!</p>
</div>
```

---

## 步驟一：建立外掛資料夾

在 `wp-content/plugins/` 裡建立以下結構：

```
wp-content/plugins/
└── my-elementor-widgets/          ← 外掛資料夾
    ├── my-elementor-widgets.php   ← 外掛主檔
    └── widgets/
        └── hello-widget.php       ← Widget 類別
```

---

## 步驟二：寫外掛主檔

建立 `my-elementor-widgets.php`：

```php
<?php
/**
 * Plugin Name: My Elementor Widgets
 * Description: 我自己的 Elementor Widget 練習
 * Version: 1.0.0
 * Author: 你的名字
 */

if ( ! defined( 'ABSPATH' ) ) exit; // 防止直接存取

// 確認 Elementor 有啟用才載入
add_action( 'plugins_loaded', function() {
    if ( ! did_action( 'elementor/loaded' ) ) {
        return; // Elementor 沒啟用，直接離開
    }

    // 在 Elementor 初始化後，載入我們的 Widget
    add_action( 'elementor/widgets/register', function( $widgets_manager ) {
        require_once __DIR__ . '/widgets/hello-widget.php';
        $widgets_manager->register( new Hello_Widget() );
    });
});
```

> **重點**：一定要等 `elementor/widgets/register` Hook 才註冊 Widget，不能太早。

---

## 步驟三：寫 Widget 類別

建立 `widgets/hello-widget.php`：

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
        return 'eicon-text';
    }

    public function get_categories() {
        return [ 'general' ];
    }

    protected function register_controls() {
        // 這個章節先不加設定，之後的章節再加
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

## 步驟四：啟用外掛

1. 進入 WordPress 後台 → **外掛**
2. 找到「My Elementor Widgets」→ 點「啟用」

---

## 步驟五：在 Elementor 裡找到你的 Widget

1. 打開任意一個頁面，點「用 Elementor 編輯」
2. 在左側搜尋框輸入「Hello」
3. 看到「Hello Widget」了！把它拖進頁面

你應該會看到 `Hello, Elementor!` 的文字出現在預覽區。

---

## 如果看不到 Widget？

| 問題 | 解法 |
|------|------|
| 外掛沒出現在後台 | 確認 PHP 檔案的 `Plugin Name:` 有寫 |
| 外掛啟用失敗 | 用瀏覽器看錯誤訊息，或開 WP Debug 模式 |
| Widget 找不到 | 確認 Elementor 有啟用、Hook 名稱有拼對 |
| 畫面沒有輸出 | 確認 `render()` 裡有輸出 HTML |

---

## 目前的檔案全貌

```php
// my-elementor-widgets.php
add_action( 'plugins_loaded', function() {
    if ( ! did_action( 'elementor/loaded' ) ) return;

    add_action( 'elementor/widgets/register', function( $widgets_manager ) {
        require_once __DIR__ . '/widgets/hello-widget.php';
        $widgets_manager->register( new Hello_Widget() );
    });
});
```

```php
// widgets/hello-widget.php
class Hello_Widget extends Widget_Base {
    public function get_name()       { return 'hello-widget'; }
    public function get_title()      { return 'Hello Widget'; }
    public function get_icon()       { return 'eicon-text'; }
    public function get_categories() { return [ 'general' ]; }

    protected function register_controls() {}

    protected function render() {
        echo '<div class="my-hello-widget"><p>Hello, Elementor!</p></div>';
    }
}
```

---

## 小結

- 外掛主檔：用 `elementor/widgets/register` Hook 來註冊 Widget
- Widget 類別：繼承 `Widget_Base`，實作必要方法
- 流程：建資料夾 → 寫 PHP → 啟用外掛 → 去 Elementor 找 Widget

---

> 下一步：`05-controls.md` — 讓使用者可以設定內容（文字、顏色、連結…）
