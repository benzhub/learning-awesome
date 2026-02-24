# 進階用法

## 編輯器即時預覽：content_template()

**何時需要**：希望使用者在編輯器右側調整設定時，能即時看到變化（而非重新載入）。

`render()` 負責前台輸出；`content_template()` 負責**編輯器預覽**，使用 Underscore.js 語法。

```php
protected function content_template() {
    ?>
    <#
    if ( '' === settings.title ) return;
    #>
    <p class="hello-world">{{{ settings.title }}}</p>
    <?php
}
```

> 💡 `{{{ }}}` 會輸出未 escape 的 HTML；`{{ }}` 會 escape。純文字用 `{{{ }}}` 或 `{{ }}` 皆可，有 HTML 時用 `{{{ }}}`。

---

## 動態內容（Dynamic Tags）

**何時需要**：讓 Control 可綁定動態來源（文章標題、自訂欄位、ACF 等）。

在 Control 加上 `'dynamic' => [ 'active' => true ]`：

```php
$this->add_control( 'title', [
    'label'   => '標題',
    'type'    => Controls_Manager::TEXT,
    'dynamic' => [ 'active' => true ],
] );
```

支援的類型：TEXT、TEXTAREA、NUMBER、URL、MEDIA 等。

---

## 自訂 Widget 分類

**何時需要**：多個自訂 Widget 想歸到同一分類。

```php
add_action( 'elementor/elements/categories_registered', function( $elements_manager ) {
    $elements_manager->add_category( 'my-widgets', [
        'title' => '我的 Widget',
        'icon'  => 'eicon-folder',
    ] );
} );

// 在 Widget 的 get_categories() 使用
public function get_categories() {
    return [ 'my-widgets' ];
}
```

---

## Hooks 擴充

**何時需要**：修改 Elementor 預設行為，或擴充功能。

| Hook | 用途 |
|------|------|
| `elementor/widgets/register` | 註冊 Widget |
| `elementor/elements/categories_registered` | 新增分類 |
| `elementor/widget/print_template` | 修改 Widget 的 JS 模板 |
| `elementor/mask_shapes/additional_shapes` | 新增遮罩形狀（SVG） |

範例：新增自訂 Mask Shape

```php
add_filter( 'elementor/mask_shapes/additional_shapes', function( $shapes ) {
    $shapes['my-shape'] = [
        'title' => '我的形狀',
        'image' => get_stylesheet_directory_uri() . '/assets/shape.svg',
    ];
    return $shapes;
} );
```

---

## 內聯編輯（Inline Editing）

**何時需要**：讓使用者在畫布上直接點擊文字編輯，而非只從左側面板改。

```php
// render()
$this->add_inline_editing_attributes( 'title', 'basic' );
?>
<h2 <?php $this->print_render_attribute_string( 'title' ); ?>>
    <?php echo esc_html( $settings['title'] ); ?>
</h2>

// content_template()
<#
view.addInlineEditingAttributes( 'title', 'basic' );
#>
<h2 {{{ view.getRenderAttributeString( 'title' ) }}}>{{{ settings.title }}}</h2>
```

`basic` 可換成 `none`（無工具列）或 `advanced`（完整工具列）。

---

## 下一步

安全性、命名規範、避坑指南 → [05-best-practices.md](05-best-practices.md)
