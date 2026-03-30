
# CSS 基本语法及注意事项

## 一、CSS 基本语法

CSS 规则由 **选择器** 和 **声明块** 组成：

```css
选择器 {
    属性: 值;
    属性: 值;
}
```

**示例**：
```css
h1 {
    color: blue;
    font-size: 24px;
}
```

- 每条声明以分号 `;` 结尾，最后一个分号可省略但建议保留。
- 属性和值之间用冒号 `:` 分隔。
- 注释：`/* 注释内容 */`（不支持 `//` 单行注释）。

---

## 二、CSS 引入方式

| 方式 | 语法 | 特点 |
|------|------|------|
| **行内样式** | `<div style="color: red;">` | 直接作用于单个元素，优先级最高，但不利于维护 |
| **内部样式表** | `<style> ... </style>` 放入 `<head>` | 适用于单个页面，结构与样式未完全分离 |
| **外部样式表** | `<link rel="stylesheet" href="style.css">` | 推荐，复用性高，利于缓存和维护 |

---

## 三、选择器（核心）

### 1. 基础选择器
| 选择器 | 示例 | 说明 |
|--------|------|------|
| 通配符 | `*` | 所有元素 |
| 标签 | `div` | 所有 `<div>` 元素 |
| 类 | `.classname` | 所有 `class="classname"` 的元素 |
| ID | `#idname` | `id="idname"` 的元素（页面唯一） |

### 2. 组合选择器
| 选择器 | 示例 | 说明 |
|--------|------|------|
| 后代 | `div p` | `div` 内的所有 `<p>`（不限层级） |
| 子代 | `div > p` | 仅 `div` 的直接子元素 `<p>` |
| 相邻兄弟 | `h1 + p` | 紧接在 `<h1>` 后的第一个 `<p>` |
| 通用兄弟 | `h1 ~ p` | 所有与 `<h1>` 同级的 `<p>` |

### 3. 伪类与伪元素
**伪类**（状态）：
- `:hover`、`:active`、`:focus`（交互状态）
- `:first-child`、`:last-child`、`:nth-child(n)`（结构位置）
- `:not(selector)`（否定）

**伪元素**（生成内容）：
- `::before`、`::after`（配合 `content` 使用）
- `::first-line`、`::first-letter`

> ⚠️ 伪元素在 CSS3 中推荐双冒号 `::`，但浏览器兼容单冒号。

### 4. 属性选择器
```css
[type="text"]       /* 精确匹配 */
[href^="https"]     /* 以 https 开头 */
[src$=".jpg"]       /* 以 .jpg 结尾 */
[class*="btn"]      /* 包含 btn */
```

### 5. 优先级计算（重要）
规则：**内联样式 > ID > 类/伪类/属性 > 标签/伪元素**  
权重计算（四位数）：
- 内联样式：`1,0,0,0`
- ID 选择器：`0,1,0,0`
- 类、伪类、属性选择器：`0,0,1,0`
- 标签、伪元素：`0,0,0,1`
- 通配符、组合器、否定伪类本身：`0,0,0,0`

> 当权重相同时，**后出现的样式覆盖前者**。`!important` 可强制提升优先级，但应谨慎使用（破坏自然层叠）。

---

## 四、常用属性分类

### 1. 文本与字体
| 属性 | 示例 | 说明 |
|------|------|------|
| `color` | `color: #333;` | 文字颜色 |
| `font-family` | `font-family: "微软雅黑", sans-serif;` | 字体栈，后为后备字体 |
| `font-size` | `font-size: 16px;` | 字号 |
| `font-weight` | `font-weight: bold;` | 粗细（100~900） |
| `text-align` | `text-align: center;` | 水平对齐 |
| `line-height` | `line-height: 1.5;` | 行高（无单位时基于字体倍数） |

### 2. 背景
```css
background-color: #f0f0f0;
background-image: url('bg.png');
background-repeat: no-repeat;
background-position: center;
background-size: cover;
/* 简写 */
background: #f0f0f0 url('bg.png') no-repeat center/cover;
```

### 3. 边框与圆角
```css
border: 1px solid #ccc;
border-radius: 8px;   /* 圆角 */
```

### 4. 盒模型核心属性
- `width` / `height`
- `padding`（内边距）
- `border`（边框）
- `margin`（外边距）
- `box-sizing: border-box;`（常用，让宽高包含 padding 和 border）

---

## 五、盒模型（Box Model）

每个元素可视作矩形盒子：

- **内容区**：`width` / `height`
- **内边距**：`padding`
- **边框**：`border`
- **外边距**：`margin`

### 标准盒模型 vs IE 盒模型
- 标准：`width` = 内容宽度，总宽度 = `width + padding + border + margin`
- IE（怪异）：`width` = 内容 + padding + border，总宽度 = `width + margin`

**设置盒模型**：
```css
box-sizing: content-box;  /* 标准 */
box-sizing: border-box;   /* 怪异（推荐） */
```

> ✅ 全局重置：`* { box-sizing: border-box; }` 可避免宽度计算困扰。

---

## 六、布局基础

### 1. display 属性
| 值 | 说明 |
|----|------|
| `block` | 块级元素，独占一行，可设宽高 |
| `inline` | 内联元素，同行显示，宽高由内容撑开 |
| `inline-block` | 同行显示，可设宽高 |
| `none` | 隐藏元素，不占空间 |
| `flex` | 弹性盒布局 |
| `grid` | 网格布局 |

### 2. 定位（position）
| 值 | 参考系 |
|----|--------|
| `static` | 默认，文档流 |
| `relative` | 相对于自身正常位置偏移，仍占原空间 |
| `absolute` | 相对于最近的非 `static` 祖先定位，脱离文档流 |
| `fixed` | 相对于视口定位，脱离文档流 |
| `sticky` | 相对定位与固定定位的混合（滚动到阈值时固定） |

### 3. Flexbox 布局（一维）
常用属性：
- 容器：`display: flex;`、`flex-direction`、`justify-content`、`align-items`、`flex-wrap`
- 项目：`flex`、`order`、`align-self`

### 4. Grid 布局（二维）
- 容器：`display: grid;`、`grid-template-columns`、`grid-template-rows`、`gap`

---

## 七、响应式设计

### 媒体查询
```css
@media (max-width: 768px) {
    body {
        font-size: 14px;
    }
}
```
常用断点：手机（≤768px）、平板（769px~1024px）、桌面（≥1025px）

### 视口设置（移动端）
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

---

## 八、单位与颜色

### 常用单位
- **绝对**：`px`（像素）、`pt`（点）
- **相对**：
  - `%`（相对父元素）
  - `em`（相对当前元素字体大小）
  - `rem`（相对根元素字体大小，推荐用于响应式）
  - `vw` / `vh`（视口宽度/高度的百分比）
  - `vmin` / `vmax`

### 颜色表示
- 关键字：`red`、`transparent`
- 十六进制：`#ff0000`、`#f00`
- RGB/RGBA：`rgb(255,0,0)`、`rgba(255,0,0,0.5)`
- HSL/HSLA：`hsl(0,100%,50%)`

---

## 九、注意事项与常见错误

### 1. 选择器特异性混乱
- 避免滥用 `!important`，破坏层叠规则。
- 不要过度使用 ID 选择器编写样式，推荐使用类。

### 2. 盒模型误解
- 未设置 `box-sizing: border-box` 时，宽高易超出预期。
- 外边距折叠（垂直相邻 margin 合并）注意处理（可通过 padding、border、overflow 等触发 BFC 解决）。

### 3. 浮动未清除
- 使用 `float` 后需清除浮动，推荐使用 Flex 或 Grid 替代传统浮动布局。
- 清除浮动方法：`clear: both` 或父元素 `overflow: hidden` / 伪元素清除。

### 4. 绝对定位的参照物
- `absolute` 相对于最近的非 `static` 祖先，若未设置则相对根元素（`<html>`）。

### 5. 移动端适配缺失
- 未添加 `viewport` meta 标签，导致移动端缩放异常。
- 使用 `px` 硬编码，未考虑不同屏幕尺寸。

### 6. 简写属性覆盖问题
- 简写（如 `background`）会重置未指定的子属性（如 `background-size`），注意顺序。

### 7. 层叠上下文与 z-index
- `z-index` 只在定位元素（`position` 非 `static`）上生效。
- 不同层叠上下文中 `z-index` 不可直接比较。

### 8. 性能与兼容
- 避免使用过多复杂选择器（如 `div ul li p span`），影响渲染性能。
- 需考虑浏览器兼容性，对于新特性可使用 Autoprefixer 自动添加前缀。

### 9. 字体单位选择
- 全局使用 `rem` 便于统一缩放，但注意 fallback 方案。
- `em` 可能因嵌套导致累加，容易失控。

---

## 十、CSS 书写规范建议

- 使用 **BEM** 或其他命名规范，避免全局污染。
- 属性按类型分组（布局 → 盒模型 → 文本 → 背景 → 其他）。
- 使用 **CSS 预处理器**（Sass、Less）提高可维护性。
- 关键样式优先加载（内联首屏 CSS），非关键样式异步加载。

---
