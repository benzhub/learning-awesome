# 06 — Render：把設定值輸出成 HTML

## 一句話

> `render()` 是你的 Widget 輸出畫面的地方：從 `get_settings_for_display()` 取值，然後輸出乾淨、安全的 HTML。

---

## 完整的實戰範例

延續前幾章，我們來寫一個有「標題 + 說明文字 + 按鈕」的 Widget。

### register_controls()

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
            'default' => '歡迎來到我的網站',
        ]
    );

    $this->add_control(
        'description',
        [
            'label'   => '說明文字',
            'type'    => \Elementor\Controls_Manager::TEXTAREA,
            'default' => '這裡是說明文字內容。',
        ]
    );

    $this->add_control(
        'button_text',
        [
            'label'   => '按鈕文字',
            'type'    => \Elementor\Controls_Manager::TEXT,
            'default' => '了解更多',
        ]
    );

    $this->add_control(
        'button_link',
        [
            'label' => '按鈕連結',
            'type'  => \Elementor\Controls_Manager::URL,
        ]
    );

    $this->end_controls_section();
}
```

---

### render()

```php
protected function render() {
    $settings = $this->get_settings_for_display();

    $title       = $settings['title'];
    $description = $settings['description'];
    $button_text = $settings['button_text'];
    $button_url  = $settings['button_link']['url'];
    $is_external = $settings['button_link']['is_external'] ? 'target="_blank"' : '';
    $nofollow    = $settings['button_link']['nofollow'] ? 'rel="nofollow"' : '';
    ?>

    <div class="my-card-widget">

        <?php if ( $title ) : ?>
            <h2 class="my-card-title">
                <?php echo esc_html( $title ); ?>
            </h2>
        <?php endif; ?>

        <?php if ( $description ) : ?>
            <p class="my-card-description">
                <?php echo esc_html( $description ); ?>
            </p>
        <?php endif; ?>

        <?php if ( $button_text && $button_url ) : ?>
            <a href="<?php echo esc_url( $button_url ); ?>"
               class="my-card-button"
               <?php echo $is_external; ?>
               <?php echo $nofollow; ?>>
                <?php echo esc_html( $button_text ); ?>
            </a>
        <?php endif; ?>

    </div>

    <?php
}
```

---

## 輸出安全性：一定要 esc_*

直接 `echo $settings['xxx']` 是**危險的**，要用 WordPress 提供的 escape 函式：

| 函式 | 用途 |
|------|------|
| `esc_html()` | 輸出純文字（最常用） |
| `esc_url()` | 輸出網址 |
| `esc_attr()` | 輸出 HTML 屬性值 |
| `wp_kses_post()` | 允許部分 HTML 標籤（如 `<strong>`、`<a>`） |

```php
// 好的做法
echo esc_html( $settings['title'] );
echo esc_url( $settings['link']['url'] );

// 危險！不要這樣做
echo $settings['title'];
```

---

## 條件輸出：檢查有沒有值再輸出

```php
// 有值才輸出，避免輸出空標籤
<?php if ( $title ) : ?>
    <h2><?php echo esc_html( $title ); ?></h2>
<?php endif; ?>
```

---

## 用 `add_render_attribute` 加 CSS class

Elementor 提供一個方法讓你更乾淨地管理 HTML 屬性：

```php
protected function render() {
    $settings = $this->get_settings_for_display();

    // 設定屬性
    $this->add_render_attribute( 'wrapper', 'class', 'my-card-widget' );

    if ( $settings['text_align'] ) {
        $this->add_render_attribute( 'wrapper', 'class', 'align-' . $settings['text_align'] );
    }

    // 輸出時用 get_render_attribute_string
    ?>
    <div <?php echo $this->get_render_attribute_string( 'wrapper' ); ?>>
        <!-- 內容 -->
    </div>
    <?php
}
```

輸出結果會是：
```html
<div class="my-card-widget align-center">...</div>
```

---

## 完整的 Widget 程式碼（整合版）

```php
<?php
use Elementor\Widget_Base;
use Elementor\Controls_Manager;

class Card_Widget extends Widget_Base {

    public function get_name()       { return 'card-widget'; }
    public function get_title()      { return '卡片 Widget'; }
    public function get_icon()       { return 'eicon-image-box'; }
    public function get_categories() { return [ 'general' ]; }

    protected function register_controls() {
        $this->start_controls_section( 'content_section', [
            'label' => '內容',
            'tab'   => Controls_Manager::TAB_CONTENT,
        ]);

        $this->add_control( 'title', [
            'label'   => '標題',
            'type'    => Controls_Manager::TEXT,
            'default' => '歡迎來到我的網站',
        ]);

        $this->add_control( 'description', [
            'label'   => '說明文字',
            'type'    => Controls_Manager::TEXTAREA,
            'default' => '這裡是說明文字。',
        ]);

        $this->add_control( 'button_text', [
            'label'   => '按鈕文字',
            'type'    => Controls_Manager::TEXT,
            'default' => '了解更多',
        ]);

        $this->add_control( 'button_link', [
            'label' => '按鈕連結',
            'type'  => Controls_Manager::URL,
        ]);

        $this->end_controls_section();
    }

    protected function render() {
        $settings = $this->get_settings_for_display();
        $btn_url  = $settings['button_link']['url'];
        $external = $settings['button_link']['is_external'] ? ' target="_blank"' : '';
        $nofollow = $settings['button_link']['nofollow'] ? ' rel="nofollow"' : '';
        ?>
        <div class="my-card-widget">
            <?php if ( $settings['title'] ) : ?>
                <h2><?php echo esc_html( $settings['title'] ); ?></h2>
            <?php endif; ?>

            <?php if ( $settings['description'] ) : ?>
                <p><?php echo esc_html( $settings['description'] ); ?></p>
            <?php endif; ?>

            <?php if ( $settings['button_text'] && $btn_url ) : ?>
                <a href="<?php echo esc_url( $btn_url ); ?>"<?php echo $external . $nofollow; ?>>
                    <?php echo esc_html( $settings['button_text'] ); ?>
                </a>
            <?php endif; ?>
        </div>
        <?php
    }
}
```

---

## 小結

- `render()` 從 `get_settings_for_display()` 取值，輸出 HTML
- 輸出文字用 `esc_html()`，輸出網址用 `esc_url()`
- 有條件地輸出（`if ( $value )`），避免空標籤
- 用 `add_render_attribute()` 管理 class 和屬性更乾淨

---

## 你現在能做的事

恭喜！你已經學完 Elementor Widget 的完整開發流程：

```
外掛主檔（register Hook）
    → Widget 類別（extends Widget_Base）
        → get_name / get_title / get_icon / get_categories
        → register_controls（定義設定欄位）
        → render（輸出 HTML）
```

接下來你可以：
- 加更多 Controls（圖片、顏色、選單…）
- 在「Style 分頁」加樣式 Controls，搭配 CSS 輸出
- 把多個 Widget 整合進同一個外掛
