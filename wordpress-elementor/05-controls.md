# 05 — Controls：讓使用者可以設定內容

## 一句話

> Controls 是你在 `register_controls()` 裡加的**設定欄位**，使用者在左側面板填寫，你在 `render()` 裡用 `$this->get_settings_for_display()` 取得值。

---

## 加 Controls 的標準流程

```php
protected function register_controls() {

    // 1. 開一個 Section（分組）
    $this->start_controls_section(
        'content_section',           // section 的 ID
        [
            'label' => '內容設定',   // 顯示的標題
            'tab'   => \Elementor\Controls_Manager::TAB_CONTENT,
        ]
    );

    // 2. 在 Section 裡加 Control（設定欄位）
    $this->add_control(
        'greeting_text',             // control 的 ID（之後取值用）
        [
            'label'   => '問候文字',
            'type'    => \Elementor\Controls_Manager::TEXT,
            'default' => 'Hello, World!',
        ]
    );

    // 3. 關閉 Section
    $this->end_controls_section();
}
```

---

## 常用的 Control 類型

| 類型常數 | 呈現方式 | 適合用途 |
|---------|---------|---------|
| `TEXT` | 單行文字輸入框 | 標題、按鈕文字 |
| `TEXTAREA` | 多行文字輸入框 | 說明文字、描述 |
| `NUMBER` | 數字輸入框 | 數量、px 大小 |
| `SELECT` | 下拉選單 | 對齊方式、樣式選擇 |
| `SWITCHER` | 開關（Toggle） | 顯示/隱藏某功能 |
| `COLOR` | 顏色選擇器 | 文字顏色、背景色 |
| `MEDIA` | 圖片選擇器 | 背景圖、Logo |
| `URL` | 連結輸入框 | 按鈕連結 |
| `ICONS` | Icon 選擇器 | 顯示圖示 |

---

## 各類型的使用範例

### TEXT — 文字輸入

```php
$this->add_control(
    'title',
    [
        'label'   => '標題',
        'type'    => \Elementor\Controls_Manager::TEXT,
        'default' => '我的標題',
    ]
);
```

---

### SELECT — 下拉選單

```php
$this->add_control(
    'text_align',
    [
        'label'   => '文字對齊',
        'type'    => \Elementor\Controls_Manager::SELECT,
        'default' => 'left',
        'options' => [
            'left'   => '靠左',
            'center' => '置中',
            'right'  => '靠右',
        ],
    ]
);
```

---

### SWITCHER — 開關

```php
$this->add_control(
    'show_subtitle',
    [
        'label'        => '顯示副標題',
        'type'         => \Elementor\Controls_Manager::SWITCHER,
        'label_on'     => '顯示',
        'label_off'    => '隱藏',
        'return_value' => 'yes',  // 開啟時的值
        'default'      => 'yes',
    ]
);
```

---

### COLOR — 顏色

```php
$this->add_control(
    'text_color',
    [
        'label'   => '文字顏色',
        'type'    => \Elementor\Controls_Manager::COLOR,
        'default' => '#333333',
    ]
);
```

---

### MEDIA — 圖片

```php
$this->add_control(
    'image',
    [
        'label'   => '選擇圖片',
        'type'    => \Elementor\Controls_Manager::MEDIA,
        'default' => [
            'url' => \Elementor\Utils::get_placeholder_image_src(),
        ],
    ]
);
```

---

### URL — 連結

```php
$this->add_control(
    'link',
    [
        'label'       => '連結網址',
        'type'        => \Elementor\Controls_Manager::URL,
        'placeholder' => 'https://example.com',
        'default'     => [
            'url'         => '',
            'is_external' => false,
            'nofollow'    => false,
        ],
    ]
);
```

---

## 兩個分頁：Content vs Style

Controls 可以放在不同分頁：

```php
// 內容分頁（使用者填文字、選圖片…）
'tab' => \Elementor\Controls_Manager::TAB_CONTENT,

// 樣式分頁（使用者調顏色、字型…）
'tab' => \Elementor\Controls_Manager::TAB_STYLE,
```

---

## 在 render() 裡取得設定值

```php
protected function render() {
    $settings = $this->get_settings_for_display();

    // 取得文字
    $title = $settings['title'];

    // 取得開關狀態
    if ( $settings['show_subtitle'] === 'yes' ) {
        // 要顯示副標題
    }

    // 取得圖片 URL
    $image_url = $settings['image']['url'];
}
```

---

## 小結

- Controls 在 `register_controls()` 裡用 `add_control()` 加入
- 一定要先 `start_controls_section()` → 加 Control → `end_controls_section()`
- 用 `$this->get_settings_for_display()` 取得使用者填寫的值
- 常用類型：`TEXT`、`SELECT`、`SWITCHER`、`COLOR`、`MEDIA`、`URL`

---

> 下一步：`06-render.md` — 把設定值正確輸出成 HTML
