# 常見使用模式

## Pattern 1：文字 + 連結按鈕

**問題**：需要可設定的標題、說明、按鈕文字與連結

**解法：**

```php
// register_controls()
$this->add_control( 'title', [
    'label'   => '標題',
    'type'    => Controls_Manager::TEXT,
    'default' => '歡迎',
] );
$this->add_control( 'button_text', [
    'label'   => '按鈕文字',
    'type'    => Controls_Manager::TEXT,
    'default' => '了解更多',
] );
$this->add_control( 'button_link', [
    'label' => '按鈕連結',
    'type'  => Controls_Manager::URL,
] );

// render()
$settings = $this->get_settings_for_display();
if ( ! empty( $settings['button_link']['url'] ) ) {
    $this->add_link_attributes( 'button_link', $settings['button_link'] );
}
?>
<a <?php $this->print_render_attribute_string( 'button_link' ); ?>>
    <?php echo esc_html( $settings['button_text'] ); ?>
</a>
```

> 💡 連結用 `add_link_attributes()` 會自動處理 `target="_blank"`、`rel="nofollow"`。

---

## Pattern 2：圖片選擇器

**問題**：讓使用者從媒體庫選圖片

**解法：**

```php
$this->add_control( 'image', [
    'label'   => '選擇圖片',
    'type'    => Controls_Manager::MEDIA,
    'default' => [
        'url' => \Elementor\Utils::get_placeholder_image_src(),
    ],
] );

// render()
$image_url = $settings['image']['url'];
if ( $image_url ) {
    echo '<img src="' . esc_url( $image_url ) . '" alt="">';
}
```

---

## Pattern 3：樣式分頁（顏色、對齊）

**問題**：在「Style」分頁加顏色、對齊等樣式設定

**解法：**

```php
$this->start_controls_section( 'style_section', [
    'label' => '樣式',
    'tab'   => Controls_Manager::TAB_STYLE,
] );

// 用 selectors 直接寫入 CSS，無需在 render 手動輸出
$this->add_control( 'text_color', [
    'label'     => '文字顏色',
    'type'      => Controls_Manager::COLOR,
    'selectors' => [
        '{{WRAPPER}} .my-title' => 'color: {{VALUE}}',
    ],
] );

$this->add_control( 'text_align', [
    'label'   => '對齊',
    'type'    => Controls_Manager::SELECT,
    'default' => 'left',
    'options' => [
        'left'   => '靠左',
        'center' => '置中',
        'right'  => '靠右',
    ],
    'selectors' => [
        '{{WRAPPER}} .my-title' => 'text-align: {{VALUE}}',
    ],
] );

$this->end_controls_section();
```

> 💡 `{{WRAPPER}}` 會替換成 Widget 外層的 class，確保樣式只作用於此 Widget。

---

## Pattern 4：開關（Switcher）控制顯示/隱藏

**問題**：依使用者開關決定是否顯示某區塊

**解法：**

```php
$this->add_control( 'show_subtitle', [
    'label'        => '顯示副標題',
    'type'         => Controls_Manager::SWITCHER,
    'label_on'     => '顯示',
    'label_off'    => '隱藏',
    'return_value' => 'yes',
    'default'      => 'yes',
] );

// render()
if ( $settings['show_subtitle'] === 'yes' ) {
    echo '<p>' . esc_html( $settings['subtitle'] ) . '</p>';
}
```

---

## Pattern 5：add_render_attribute 管理 class

**問題**：依設定動態組合多個 CSS class

**解法：**

```php
protected function render() {
    $settings = $this->get_settings_for_display();

    $this->add_render_attribute( 'wrapper', 'class', 'my-card-widget' );
    if ( ! empty( $settings['text_align'] ) ) {
        $this->add_render_attribute( 'wrapper', 'class', 'align-' . $settings['text_align'] );
    }
    ?>
    <div <?php $this->print_render_attribute_string( 'wrapper' ); ?>>
        <!-- 內容 -->
    </div>
    <?php
}
```

---

## 常用 Control 類型速查

| 類型 | 用途 |
|------|------|
| `TEXT` | 單行文字 |
| `TEXTAREA` | 多行文字 |
| `WYSIWYG` | 富文本 |
| `NUMBER` | 數字 |
| `SELECT` | 下拉選單 |
| `SWITCHER` | 開關 |
| `COLOR` | 顏色 |
| `MEDIA` | 圖片/媒體 |
| `URL` | 連結（含 target、nofollow） |
| `ICONS` | 圖示選擇 |

---

## 下一步

編輯器即時預覽、動態內容、Hooks → [04-advanced.md](04-advanced.md)
