
# CSS学习笔记

## 学习背景
CSS从开始开始听到对我来说是一个非常陌生的概念，大概只知道在网页设计中需要它。
## 学习内容的目录
* **引入方式**
* **选择器**
* **常用属性分类**
* **盒模型**
* **布局基础**
* **响应式设计**
* **单位与颜色**
* **注意事项与常见错误**

## 学习内容

## 一、CSS 引入方式

| 方式 | 语法 | 特点 |
|------|------|------|
| **行内样式** | `<div style="color: red;">` | 直接作用于单个元素，优先级最高，但不利于维护 |
| **内部样式表** | `<style> ... </style>` 放入 `<head>` | 适用于单个页面，结构与样式未完全分离 |
| **外部样式表** | `<link rel="stylesheet" href="style.css">` | 推荐，复用性高，利于缓存和维护 |
* **感想**：要想CSS生效就必须将该其与HTML进行连接起来，但是我主要用的方式是分别创立CSS和HTML两个文件，对CSS文件进行对内容的美化和细致化的描述，对HTML进行需要部分的文件的搭建，后在在MTHL引入CSS的相关描述，赋予该部分的内部色彩而不仅仅是一个空虚的外壳。

---

## 二、选择器（核心）

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
[type="text"]       /* 精确匹配 */
[href^="https"]     /* 以 https 开头 */
[src$=".jpg"]       /* 以 .jpg 结尾 */
[class*="btn"]     /* 包含 btn */

**感想**：在我看来选择器顾名思义“选择两个字”诠释了他的含义，代码就是一个大的范围，但是有时需要对一些地方进行修改，这时选择器其中的一些相关的固定代码加上固定的描述就能更好的对这一部分进行修改，例如我要找到穿红色衣服的人就需要选择器的应用。在我对第一个前端的相关作业中看见了组合选择器的应用之广泛，还有属性选择器例如"btn"这类在CSS中是对按钮的修饰什么颜色多大的像素等等，在HTML中就是取消或者按下“按钮这一个键”，在这一部分让我对HTML的架构和CSS的细致化有了更为深刻的理解和印象。

---
## 三、常用属性分类

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

## 四、盒模型（Box Model）

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
**感想**：盒模型在我的实际操作应用是挺多的，在我看来是就像是给了一张写满字的A4纸但是这样太枯燥了并且没有新意，所以此时盒模型起到了非常重要的作用，我可以对边框进行修饰，颜色还有形状，还可以对字进行大小的修饰，“box-size、font-size"等等，在我写的任务中使用频率很高，赋予给多的色彩，所以“这张A4纸”会更好看，更吸引人注意力。

## 五、布局基础

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
**感想**：实现对页面的某部分板块位置的布局，使自己静态网站的页面布局更加灵活，例如flex,在打开页面的时候实现一种滚动展开的形式，不再是那么的死板。Flexbox只有行或者列，所以只能呈现向下或者向右的排列，Grid同时兼备行和列，所以定位更加的准确，类似于地球上的经度和纬度。大结构用Grid,小结构用Flexbox更好。

## 六、响应式设计

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
**感想**：响应式设计在于“响应”二字，不管是手机、平板、还是电脑都能进行**响应**，就像一些元素一些链接在电脑上能完整的打开但是在手机上需要进行横向的滚动打开，甚至有的打不开，这会导致极为不方便，不够完美，所以这个设计就是来打破这样的问题的，一般是用min-width或者max-width来进行像素的放大或者缩小，根据查阅的相关方法为：
1、先写好HTML结构。
2、用移动优先方式写基础CSS（手机版）。
3、不断拉宽浏览器窗口，观察何时布局变得难看，在那个宽度加min-width断点调整样式。
4、测试触摸和鼠标行为。
5、用真实手机或浏览器开发者工具的手机模拟模式测试。
突然想到我们学校的官网在手机打开不是那么方便和电脑上却可以显示全屏，是不是响应式不行呢？

## 七、单位与颜色

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
**感想**：相对单位让尺寸更适应，颜色就是对字体或者某一部分进行进行色彩的渲染，实际上十六进制运用更加易懂更加方便，rgb使得颜色不再是那么单一，具有透明度，HSL对色彩饱和度还有亮度有着更为丰富的点缀，所以综合上我认为颜色相关的表示需要各个表示间的相互应用和联系。

## 八、注意事项与常见错误

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
- `em` 可能因嵌套导致累加，易失控。

 ---
 # 参考资料
 1、哔哩哔哩**小枫学长**
 
 2 https://www.runoob.com/css/css-tutorial.html
 
 3 https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Responsive_Design
