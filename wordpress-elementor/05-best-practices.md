# 最佳實踐與避坑指南

## ✅ 推薦做法

### 1. 輸出一定要 Escape

| 情境 | 函式 |
|------|------|
| 純文字 | `esc_html()` |
| 網址 | `esc_url()` |
| HTML 屬性值 | `esc_attr()` |
| 允許部分 HTML | `wp_kses_post()` |

```php
// ✅ 正確
echo esc_html( $settings['title'] );
echo '<a href="' . esc_url( $settings['link']['url'] ) . '">';

// ❌ 危險
echo $settings['title'];
```

---

### 2. Widget ID 加前綴

避免與其他外掛衝突，`get_name()` 回傳值加前綴：

```php
// ✅ 推薦
return 'myplugin-hello-widget';

// ❌ 易衝突
return 'hello-widget';
```

---

### 3. 使用 esc_html__ 做國際化

```php
'label' => esc_html__( '標題', 'my-plugin' ),
'title' => esc_html__( 'Hello Widget', 'my-plugin' ),
```

---

### 4. 連結用 add_link_attributes

不要手動拼 `target`、`rel`，用 Elementor 內建方法：

```php
// ✅ 推薦
if ( ! empty( $settings['link']['url'] ) ) {
    $this->add_link_attributes( 'link', $settings['link'] );
}
?>
<a <?php $this->print_render_attribute_string( 'link' ); ?>>

// ❌ 易漏
<a href="..." target="<?php echo $external ? '_blank' : ''; ?>">
```

---

### 5. 外掛宣告 Requires Plugins

在 Plugin Header 標註依賴，避免 Elementor 未啟用時出錯：

```php
/**
 * Plugin Name: My Elementor Widgets
 * Requires Plugins: elementor
 * Elementor tested up to: 3.25.0
 */
```

---

## ❌ 常見錯誤

### 錯誤 1：太早註冊 Widget

**現象**：Widget 找不到或報錯  
**原因**：在 `elementor/widgets/register` 之前就註冊  
**修正**：用 `add_action( 'elementor/widgets/register', ... )`，不要用 `init` 等過早的 Hook

---

### 錯誤 2：直接 echo 使用者輸入

**現象**：XSS 風險  
**原因**：未 escape  
**修正**：一律用 `esc_html()`、`esc_url()` 等

---

### 錯誤 3：content_template 未實作

**現象**：編輯器預覽與前台不一致  
**原因**：只實作 `render()`，沒有 `content_template()`  
**修正**：若需要即時預覽，兩者都要實作，且邏輯一致

---

### 錯誤 4：Control ID 與其他 Control 重複

**現象**：取值錯誤  
**原因**：同一 Section 內 ID 重複  
**修正**：每個 Control 用唯一 ID，如 `title`、`subtitle`、`button_text`

---

## 延伸閱讀

- [Elementor 開發者文件](https://developers.elementor.com/)
- [Elementor Widget 結構](https://developers.elementor.com/docs/widgets/)
- [Elementor Hooks](https://developers.elementor.com/docs/hooks/)

---

> 恭喜完成！你已掌握 Elementor 自訂 Widget 的完整開發流程。
