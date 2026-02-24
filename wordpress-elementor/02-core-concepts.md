# 核心概念

## 概念一：Widget 的程式碼結構

一個 Widget 是**繼承 `\Elementor\Widget_Base` 的 PHP 類別**，實作幾個固定方法，Elementor 會自動串接。

```php
class My_Widget extends \Elementor\Widget_Base {

    public function get_name() {
        return 'my-widget';  // 唯一 ID，小寫+連字號
    }

    public function get_title() {
        return '我的 Widget';  // 面板顯示名稱
    }

    public function get_icon() {
        return 'eicon-text';  // Elementor 或 Font Awesome 圖示
    }

    public function get_categories() {
        return [ 'general' ];  // 分類
    }

    protected function register_controls() {
        // 定義左側設定欄位
    }

    protected function render() {
        // 輸出 HTML
    }
}
```

> 💡 必填：`get_name()`、`get_title()`、`render()`。其他依需求實作。

---

## 概念二：Controls（設定欄位）

Controls 是左側面板的**設定項目**，在 `register_controls()` 裡用 `start_controls_section` + `add_control` + `end_controls_section` 定義。

```php
protected function register_controls() {
    $this->start_controls_section(
        'content_section',
        [
            'label' => '內容',
            'tab'   => \Elementor\Controls_Manager::TAB_CONTENT,
        ]
    );

    $this->add_control(
        'title',
        [
            'label'   => '標題',
            'type'    => \Elementor\Controls_Manager::TEXT,
            'default' => '預設標題',
        ]
    );

    $this->end_controls_section();
}
```

> 💡 `add_control` 的第一個參數是 control ID，之後在 `render()` 用 `$settings['title']` 取值。

---

## 概念三：Render（輸出 HTML）

`render()` 負責把設定值轉成實際 HTML。用 `get_settings_for_display()` 取得使用者填寫的值。

```php
protected function render() {
    $settings = $this->get_settings_for_display();
    $title = $settings['title'];
    ?>
    <div class="my-widget">
        <h2><?php echo esc_html( $title ); ?></h2>
    </div>
    <?php
}
```

> ⚠️ 輸出使用者資料一定要用 `esc_html()`、`esc_url()` 等 escape 函式，避免 XSS。

---

## 概念四：Widget 註冊流程

Widget 必須透過 `elementor/widgets/register` Hook 註冊，且需在 Elementor 載入後執行。

```php
add_action( 'elementor/widgets/register', function( $widgets_manager ) {
    require_once __DIR__ . '/widgets/my-widget.php';
    $widgets_manager->register( new My_Widget() );
});
```

> 💡 若外掛需在 Elementor 之前載入，可用 `plugins_loaded` 檢查 `did_action( 'elementor/loaded' )` 再掛 Hook。

---

## 概念關係圖

```
elementor/widgets/register
        ↓
Widgets_Manager::register( new My_Widget )
        ↓
Widget 類別（get_name, get_title, register_controls, render）
        ↓
使用者拖放 → 左側顯示 Controls → 填寫 → render() 輸出 HTML
```

---

## 下一步

日常開發常用的 Control 類型與實戰模式 → [03-common-patterns.md](03-common-patterns.md)
