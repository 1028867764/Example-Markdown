# 前言
虽然Flutter开发主要存在于移动端和桌面端，但是，对于Flutter开发者而言，补充Web前端开发知识是很有必要的。

# **HTML5 和 CSS3 基础（快速入门）**
在开始学习 Vue3 之前，理解一些 HTML 和 CSS 基础是必要的。主要包括：

## **1. HTML5**：

### 1.1 常见标签

#### 1.1.1 `<div>`：用于将页面元素分组，通常用于布局。

```html
<div class="container">
    <p>这是一个段落。</p>
    <p>这是另一个段落。</p>
</div>
```

* **解释**：`<div>` 是一个块级元素，用来包裹其他元素。通常用来做布局和样式调整。

#### 1.1.2 `<span>`：用于包装文本或其他行内元素，常用于样式设置。

```html
<p>我喜欢 <span style="color: blue;">蓝色</span> 和 <span style="color: green;">绿色</span>。</p>
```

* **解释**：`<span>` 是一个行内元素，用来包裹文本或其他行内元素，通常用于对部分文本进行样式修改。

#### 1.1.3 `<a>`：创建超链接。

```html
<a href="https://www.example.com" target="_blank">点击这里访问 Example</a>
```

* **解释**：`<a>` 标签用于创建超链接，`href` 属性定义目标 URL，`target="_blank"` 表示在新窗口打开链接。

#### 1.1.4 `<p>`：用于定义段落。

```html
<p>这是一个段落。</p>
```

* **解释**：`<p>` 是段落标签，用来显示一段文字内容。

#### 1.1.5 `<ul>`：定义无序列表。

```html
<ul>
    <li>苹果</li>
    <li>香蕉</li>
    <li>橘子</li>
</ul>
```

* **解释**：`<ul>` 定义一个无序列表，`<li>` 定义列表项。无序列表的项通常会显示为带圆点的列表。

#### 1.1.6 `<li>`：定义列表项。

```html
<ul>
    <li>Python</li>
    <li>JavaScript</li>
    <li>Java</li>
</ul>
```

* **解释**：`<li>` 标签表示一个列表项。它可以用在有序列表（`<ol>`）或无序列表（`<ul>`）中。

### 1.2 表单元素（`<input>`, `<textarea>`, `<select>` 等）

#### 1.2.1 `<input>`：定义表单控件，如文本框、单选框、复选框等。

```html
<form>
    <label for="name">姓名：</label>
    <input type="text" id="name" name="name">
    
    <label for="email">邮箱：</label>
    <input type="email" id="email" name="email">
    
    <label for="age">年龄：</label>
    <input type="number" id="age" name="age">
    
    <input type="submit" value="提交">
</form>
```

* **解释**：`<input>` 可以有多种类型，`type="text"` 表示文本框，`type="email"` 表示邮箱输入框，`type="number"` 表示数字输入框，`type="submit"` 表示提交按钮。

#### 1.2.2 `<textarea>`：用于多行文本输入。

```html
<form>
    <label for="message">留言：</label>
    <textarea id="message" name="message" rows="4" cols="50"></textarea>
    <input type="submit" value="提交">
</form>
```

* **解释**：`<textarea>` 用来创建一个多行文本框，`rows` 和 `cols` 属性控制文本框的行数和列数。

#### 1.2.3 `<select>`：定义下拉选择框。

```html
<form>
    <label for="color">选择颜色：</label>
    <select id="color" name="color">
        <option value="red">红色</option>
        <option value="green">绿色</option>
        <option value="blue">蓝色</option>
    </select>
    <input type="submit" value="提交">
</form>
```

* **解释**：`<select>` 创建下拉框，`<option>` 定义下拉框中的每个选项。

### 1.3 块级元素与行内元素

#### 1.3.1 块级元素：

* 块级元素占据整行，从左到右填充屏幕，通常用于布局和分组。例如：`<div>`、`<p>`、`<ul>`。

```html
<div>
    <h1>标题</h1>
    <p>这是一个段落。</p>
</div>
```

#### 1.3.2 行内元素：

* 行内元素只占据其内容所需的空间，不会换行。例如：`<span>`、`<a>`、`<strong>`。

```html
<p>这是 <span style="color: red;">红色文本</span> 的一部分。</p>
```

* **总结**：块级元素会占据整个可用宽度并强制换行，行内元素只占用其内容的宽度。

### 1.4 常用属性（`class`, `id`, `href`, `src`, `alt`）

#### 1.4.1 `class`：用于定义元素的类，方便通过 CSS 或 JavaScript 操控。

```html
<div class="container">
    <p class="highlight">这是带有样式的段落。</p>
</div>
```

* **解释**：`class` 是 HTML 元素的一个属性，用来指定元素的类，可以在 CSS 中定义样式或在 JavaScript 中通过类名访问元素。

#### 1.4.2 `id`：定义元素的唯一标识符。

```html
<p id="intro">这是唯一的段落。</p>
```

* **解释**：`id` 属性用于为元素指定唯一标识符，在一个页面中，`id` 必须是唯一的。

#### 1.4.3 `href`：指定超链接的目标地址。

```html
<a href="https://www.example.com">访问 Example 网站</a>
```

* **解释**：`href` 用于定义链接的目标 URL，点击链接时会跳转到指定页面。

#### 1.4.4 `src`：定义图片或其他媒体资源的路径。

```html
<img src="image.jpg" alt="描述图片内容">
```

* **解释**：`src` 用来指定图片或其他媒体文件的路径，`alt` 属性用于提供图片的替代文本，方便屏幕阅读器或在图片无法显示时使用。

#### 1.4.5 `alt`：为图片提供描述性文本。

```html
<img src="logo.png" alt="网站Logo">
```

* **解释**：`alt` 是 `img` 标签的属性，它用于提供图片的描述性文字，帮助搜索引擎理解图片内容，也在图片加载失败时显示。

### 1.5 新增的 HTML5 标签（如 `<header>`, `<footer>`, `<article>`, `<section>` 等）

#### 1.5.1 `<header>`：定义文档的头部区域，通常包含标题、导航等内容。

```html
<header>
    <h1>网站标题</h1>
    <nav>
        <ul>
            <li><a href="#">首页</a></li>
            <li><a href="#">关于</a></li>
            <li><a href="#">联系</a></li>
        </ul>
    </nav>
</header>
```

* **解释**：`<header>` 用来定义页面的头部区域，通常包含文档的标题、导航链接等。

#### 1.5.2 `<footer>`：定义文档的底部区域，通常包含版权、联系信息等。

```html
<footer>
    <p>&copy; 2026 网站版权</p>
    <p>联系邮箱：info@example.com</p>
</footer>
```

* **解释**：`<footer>` 用来定义页面的底部区域，通常包含版权信息、联系方式等。

#### 1.5.3 `<article>`：定义一篇独立的文章或内容块。

```html
<article>
    <h2>文章标题</h2>
    <p>这是文章的内容。</p>
</article>
```

* **解释**：`<article>` 用来定义一篇文章或内容块，适用于新闻、博客等内容。

#### 1.5.4 `<section>`：定义文档中的一个区域或部分。

```html
<section>
    <h2>部分标题</h2>
    <p>这是部分的内容。</p>
</section>
```

* **解释**：`<section>` 用来定义文档中的一个区域或部分，通常包含一组相关的内容。


## **2. CSS3**

### 2.1 常见选择器

在 CSS 中，选择器用于选中 HTML 元素并对其应用样式。常见的选择器有以下几种：

#### **2.1.1 元素选择器（Element Selector）**：直接通过元素的标签名来选中 HTML 元素。

   ```css
   /* 选择所有 <p> 标签 */
   p {
       color: blue; /* 设置文本颜色为蓝色 */
   }
   ```

#### **2.1.2 类选择器（Class Selector）**：通过 HTML 元素的 `class` 属性来选中元素。类选择器以 `.` 开头。

   ```css
   /* 选择所有 class 为 'highlight' 的元素 */
   .highlight {
       background-color: yellow; /* 设置背景颜色为黄色 */
   }
   ```

#### **2.1.3 ID选择器（ID Selector）**：通过 HTML 元素的 `id` 属性来选中元素。ID选择器以 `#` 开头。

   ```css
   /* 选择 id 为 'header' 的元素 */
   #header {
       font-size: 24px; /* 设置字体大小为24px */
   }
   ```

#### **2.1.4 组合选择器**：可以组合多个选择器来选择元素。例如，选中所有 `p` 标签且具有 `highlight` 类的元素。

   ```css
   p.highlight {
       color: red; /* 设置文本颜色为红色 */
   }
   ```

### 2.2 布局基础

CSS 提供了强大的布局工具，如 `flexbox` 和 `grid`。这两者的目标是让页面布局更容易、更灵活。

#### **2.2.1 Flexbox**

`flexbox` 是一种一维布局模型，可以很方便地控制元素的排列方向、对齐方式等。

1. **简单的 `flexbox` 布局**

   ```css
   .container {
       display: flex; /* 启用 flexbox 布局 */
       justify-content: space-between; /* 主轴上子项间隔均匀 */
   }
   .item {
       width: 100px;
       height: 100px;
       background-color: lightblue;
   }
   ```

   ```html
   <div class="container">
       <div class="item">Item 1</div>
       <div class="item">Item 2</div>
       <div class="item">Item 3</div>
   </div>
   ```

2. **主轴与交叉轴的对齐**

   ```css
   .container {
       display: flex;
       align-items: center; /* 交叉轴（垂直方向）居中对齐 */
       justify-content: center; /* 主轴（水平方向）居中对齐 */
   }
   ```

#### **2.2.2 Grid**

`grid` 是二维布局模型，可以同时控制行和列。

1. **简单的 `grid` 布局**

   ```css
   .grid-container {
       display: grid;
       grid-template-columns: repeat(3, 1fr); /* 三列，每列等宽 */
       gap: 10px; /* 列与列之间的间隙 */
   }
   .grid-item {
       background-color: lightcoral;
       padding: 20px;
   }
   ```

   ```html
   <div class="grid-container">
       <div class="grid-item">Item 1</div>
       <div class="grid-item">Item 2</div>
       <div class="grid-item">Item 3</div>
   </div>
   ```

2. **更复杂的 `grid` 布局**

   ```css
   .grid-container {
       display: grid;
       grid-template-columns: 1fr 2fr 1fr; /* 三列，第二列宽度是第一列和第三列的两倍 */
       grid-template-rows: 100px auto 100px; /* 三行，第一行和第三行高100px，中间行自动高度 */
       gap: 15px;
   }
   ```


### 2.3 样式设置

CSS 允许你自定义网页元素的各种样式，包括字体、颜色、边框、背景等。

####  **2.3.1 字体样式**

   ```css
   p {
       font-family: 'Arial', sans-serif; /* 设置字体 */
       font-size: 16px; /* 设置字体大小 */
       font-weight: bold; /* 设置字体加粗 */
       line-height: 1.5; /* 设置行高 */
   }
   ```

####  **2.3.2 文本颜色和背景**

   ```css
   h1 {
    color: #333; /* 设置文本颜色 */
    background-color: #f4f4f4; /* 设置背景颜色 */
    padding: 10px; /* 设置内边距 */
    margin: 20px; /* 设置外边距，使h1与其他元素之间有20px的间距 */
   }
   ```

####  **2.3.3 边框设置**

   ```css
   .box {
       border: 2px solid black; /* 设置边框为黑色，宽度为2px */
       border-radius: 8px; /* 设置圆角边框 */
   }
   ```

####  **2.3.4 背景图片**

   ```css
   .background {
       background-image: url('image.jpg'); /* 设置背景图片 */
       background-size: cover; /* 背景图片覆盖整个区域 */
       background-position: center; /* 背景图片居中 */
   }
   ```


### 2.4 响应式设计

响应式设计是为了让网页在不同设备和屏幕尺寸下都能良好展示。常用的工具包括 `@media` 查询、`rem` 和 `em` 单位。

####  **2.4.1 `@media` 查询**

   `@media` 允许你为不同的屏幕尺寸和设备类型设置不同的样式。

   ```css
   @media screen and (max-width: 768px) {
       body {
           background-color: lightgreen; /* 当屏幕宽度小于768px时，背景变为绿色 */
       }
   }
   ```



#### **2.4.2 `rem` 和 `em` 单位**

在 CSS 中，`rem` 和 `em` 是相对单位，用于指定元素的尺寸、间距、字体大小等。它们的工作原理有很大不同，因此理解它们的差异对于设计灵活且可扩展的页面非常重要。

##### **2.4.2.1 `rem`（root em）**

`rem` 是 "root em" 的缩写，表示相对于根元素（通常是 `<html>` 标签）字体大小的倍数。其优势是它提供了一种简单且一致的方式来控制页面的布局和字体大小。

**特点：**

* `rem` 总是相对于根元素的字体大小。
* 如果根元素的字体大小被改变，所有使用 `rem` 单位的元素都会相应调整。
* 适用于全局布局和响应式设计。

**示例代码：**

```css
html {
    font-size: 16px; /* 设置根元素字体大小为 16px */
}

.container {
    width: 50rem; /* 50 * 16px = 800px */
    padding: 2rem; /* 2 * 16px = 32px */
}

.text {
    font-size: 1.5rem; /* 1.5 * 16px = 24px */
}
```

**解释：**

* 根元素的字体大小设置为 16px，因此 `1rem = 16px`。
* `.container` 的宽度设置为 `50rem`，即 `50 * 16px = 800px`。通过使用 `rem`，你可以确保 `.container` 的宽度始终根据根元素字体大小进行调整。
* `.text` 的字体大小设置为 `1.5rem`，即 `1.5 * 16px = 24px`，如果根元素字体大小改变，这个值也会相应缩放。

**响应式设计应用：**

```css
/* 默认根元素字体大小 */
html {
    font-size: 16px;
}

/* 屏幕宽度小于 768px 时，调整根元素字体大小 */
@media (max-width: 768px) {
    html {
        font-size: 14px; /* 更小的字体大小 */
    }
}

/* .container 和 .text 会自动适应根元素的字体大小变化 */
.container {
    width: 50rem; /* 50 * 16px = 800px --> 50 * 14px = 700px */
}

.text {
    font-size: 1.5rem; /* 1.5 * 16px = 24px --> 1.5 * 14px = 21px */
}
```

**优点：**

* **统一性**：通过改变根元素的字体大小，可以全局控制页面的缩放，使得页面在不同设备上的适应性更强。
* **方便的响应式设计**：根元素的字体大小可以根据屏幕尺寸变化，进而影响整个页面的布局和字体大小，减少了为每个元素单独设置尺寸的麻烦。

---

##### **2.4.2.2 `em`（相对字体大小）**

`em` 单位表示相对于当前元素的字体大小。每个元素的 `em` 值是基于其父元素的字体大小计算的，因此，`em` 是一种相对的单位，适用于局部布局中需要继承和调整的场景。

**特点：**

* `em` 基于当前元素的字体大小进行计算，它的值是相对于父元素的字体大小的倍数。
* `em` 的应用是层级性的，子元素的字体大小会受父元素字体大小的影响。

**示例代码：**

```css
/* 设置父元素的字体大小 */
.parent {
    font-size: 20px;
}

.child {
    font-size: 2em; /* 子元素字体大小为父元素的2倍，即 20px * 2 = 40px */
    margin-top: 1em; /* 设置上边距为父元素字体大小的1倍，即 20px */
}
```

**解释：**

* `.parent` 元素的字体大小为 `20px`。
* `.child` 元素的字体大小是 `.parent` 的 `2em`，即 `20px * 2 = 40px`。
* `.child` 的 `margin-top` 也是基于父元素的字体大小设置的，即 `20px`。

**嵌套使用 `em`：**

```css
/* 父元素字体大小 */
.container {
    font-size: 20px;
}

/* 子元素的字体大小相对于父元素 */
.child {
    font-size: 1.5em; /* 1.5倍父元素字体大小，即 20px * 1.5 = 30px */
}

/* 更深层次的嵌套 */
.grandchild {
    font-size: 1.2em; /* 1.2倍父元素字体大小，即 30px * 1.2 = 36px */
}
```

**解释：**

* `.container` 的字体大小为 `20px`。
* `.child` 的字体大小是 `.container` 的 `1.5em`，即 `20px * 1.5 = 30px`。
* `.grandchild` 的字体大小是 `.child` 的 `1.2em`，即 `30px * 1.2 = 36px`。

**优点：**

* **灵活性**：`em` 可以根据父元素的字体大小动态调整子元素的尺寸，非常适合需要根据层级关系调整布局的情况。
* **继承性**：`em` 继承父元素的字体大小，因此可以通过修改父元素来影响子元素的大小，特别适用于组件化开发。

---
##### **2.4.2.3 总结**

**`rem` 和 `em` 的比较**

| 特性   | `rem`                 | `em`           |
| ---- | --------------------- | -------------- |
| 参考对象 | 根元素 (`<html>`)        | 当前元素的父元素       |
| 应用场景 | 页面布局、全局字体大小控制         | 局部布局、继承调整      |
| 影响范围 | 所有使用 `rem` 的元素都受根元素影响 | 每个元素的大小受父元素影响  |
| 适应性  | 适合全局调整页面布局、响应式设计      | 适合基于父元素的局部调整   |
| 调整方式 | 改变根元素字体大小，整体缩放        | 改变父元素字体大小，局部调整 |

**总结与建议**

* **使用 `rem`**：当你需要统一控制页面的布局和字体大小，尤其是在响应式设计中，`rem` 会非常有用。通过调整根元素的字体大小，你可以让页面上的所有元素自动缩放，确保一致性。
* **使用 `em`**：当你需要某个元素的大小相对于其父元素的大小调整时，`em` 是最佳选择。它非常适合在组件设计中使用，尤其是当父元素的字体大小可能动态变化时。

**最佳实践：**

* 使用 `rem` 来设置全局布局、字体大小、间距等，确保页面整体的一致性和响应式适配。
* 使用 `em` 来处理嵌套元素，尤其是在组件内部，当你需要根据父元素的大小动态调整子元素时，`em` 是一个灵活的选择。

通过这种方式，你可以实现更加灵活和响应式的页面设计，提升页面的可访问性和用户体验！


#### **2.4.3 Viewport 设置**

**Viewport** 是浏览器用来显示网页的可视区域。它的大小决定了网页内容的缩放和显示方式。尤其在移动设备（如手机、平板）上，如何正确地设置 viewport 是保证网页在不同设备上显示良好的关键。

通过在 HTML 文档的 `<head>` 部分添加 `<meta>` 标签来定义 viewport 设置，可以控制网页的缩放和显示方式。

##### **2.4.3.1 常见的 viewport 设置**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

* **`width=device-width`**：设置页面宽度等于设备的宽度。这是为了确保页面的内容不会被缩放，且页面的布局会根据设备的屏幕宽度自适应。

* **`initial-scale=1.0`**：设置页面初始加载时的缩放比例。`1.0` 表示页面显示时没有缩放，用户看到的就是网页的原始比例。

##### **2.4.3.2 解释：**

1. **`width=device-width`**：

   * 这意味着页面的宽度将与设备的屏幕宽度相等。例如，假设你的网页宽度是 1200px，如果用户使用的是一个屏幕宽度为 375px 的手机，那么 `width=device-width` 会让浏览器将页面缩放，使页面的内容自适应设备屏幕的宽度，而不需要水平滚动条。
2. **`initial-scale=1.0`**：

   * 这指定页面在加载时的初始缩放比例是 100%（即没有缩放）。如果你希望在初次加载时缩放页面，设置 `initial-scale` 为其他值（如 `1.5`）会使页面内容变大，`0.5` 会使页面变小。

##### **2.4.3.3 为什么需要设置 viewport？**

在没有设置 viewport 的情况下，浏览器默认会将网页的宽度设置为桌面端的宽度，这通常会导致移动设备上的网页显示不正常，可能会出现页面内容太宽，或者缩放不合适的情况。设置 viewport 后，网页能够根据不同设备的屏幕宽度进行缩放和布局调整，使得网页能够更好地适应各种设备。

##### **2.4.3.4 常见的 viewport 设置选项**

除了最基础的设置外，`viewport` 还可以包括其他属性来控制页面的缩放行为：

1. **禁止缩放**

如果你希望用户不能缩放网页，可以使用 `user-scalable=no`。这对于某些不希望被缩放的应用（如移动端游戏、一些表单页等）非常有用。

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
```

* **`user-scalable=no`**：禁止用户手动缩放网页内容。

2. **设置最大/最小缩放比例**

你可以控制页面缩放的最大和最小值，防止用户放大或缩小到某些不合适的比例。

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

* **`maximum-scale=1.0`**：设置页面的最大缩放比例为 `1.0`，用户无法进一步放大。
* **`minimum-scale=1.0`**：设置页面的最小缩放比例为 `1.0`，用户无法缩小页面。

##### **2.4.3.5 更多例子和说明**

1. **基本响应式布局**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

这是最常见的设置方式，适用于绝大多数网站。在这种情况下，页面将自动根据设备的屏幕宽度来调整，确保页面在手机、平板等不同设备上都能正确显示。

2. **自适应布局（禁止缩放）**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
```

这种设置适用于一些特定的应用场景，如全屏的移动网站、游戏、或具有特定交互的应用。在这些页面上，通常不希望用户进行缩放操作，因此设置了 `user-scalable=no`。

3. **自适应布局与缩放限制**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.5, minimum-scale=0.5">
```

在这个例子中，页面的缩放比例被限制在 `0.5` 到 `1.5` 之间。用户无法缩放得过小或过大，适合一些需要控制缩放范围的页面。

4. **复杂布局和定制化**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=2.0, minimum-scale=1.0, user-scalable=yes">
```

这种设置允许用户在页面上进行缩放，但限制了缩放的最大值为 `2.0`，最小值为 `1.0`。用户可以在一定范围内自由缩放页面。

##### **2.4.3.6 如何使用 viewport 设置来改善用户体验**

1. **响应式设计的基础**：通过设置 `viewport`，你可以让网页根据用户设备的屏幕宽度自动调整布局。例如，网页的导航栏、图片、按钮、文字等内容会根据设备的宽度自适应，确保在手机、平板和桌面设备上都能良好展示。

2. **改善触摸体验**：禁止缩放（`user-scalable=no`）可以帮助提高某些交互性较强的网页（如移动游戏或交互式表单）的用户体验，避免用户不小心缩放页面导致的布局问题。

3. **避免横向滚动**：通过设置 `width=device-width`，可以确保页面宽度与设备屏幕匹配，避免在移动设备上出现水平滚动条，从而提高用户浏览体验。

4. **提供适当的缩放范围**：通过 `maximum-scale` 和 `minimum-scale`，你可以为页面设置合适的缩放范围，防止用户缩放得过大或过小。这样可以确保页面在视觉上保持一致性，同时避免过度缩放导致的内容错位或布局失衡。

---

##### **2.4.3.7 总结**

`viewport` 元素是响应式设计的一个重要组成部分，通过它可以控制页面如何在不同设备上进行缩放和适配。设置好 viewport 能让你的网页在各种屏幕尺寸的设备上显示得更加完美。

* **`width=device-width`** 使页面宽度与设备屏幕宽度一致，防止出现水平滚动条。
* **`initial-scale=1.0`** 保证页面在加载时没有缩放，用户看到的就是页面的原始比例。
* **`user-scalable=no`** 可以禁止缩放。
* **`maximum-scale` 和 `minimum-scale`** 可以控制页面缩放的范围。

掌握 viewport 的使用，可以帮助你打造更加适应不同设备、提升用户体验的网页！


### 2.5 动画与过渡

CSS 动画和过渡可以为页面添加动感效果，让用户界面更加生动。

####  **2.5.1 过渡（`transition`）**

   `transition` 用于在元素的状态变化时提供平滑的过渡效果。

   ```css
   .box {
       width: 100px;
       height: 100px;
       background-color: red;
       transition: all 0.3s ease-in-out; /* 所有属性变化，持续时间为0.3秒 */
   }

   .box:hover {
       width: 200px; /* 鼠标悬停时改变宽度 */
       background-color: blue; /* 鼠标悬停时改变背景颜色 */
   }
   ```

####  **2.5.2 动画（`animation`）**

   `animation` 可以让元素进行更复杂的动画效果，通过关键帧（`@keyframes`）控制动画过程。

   ```css
   @keyframes move {
       0% {
           transform: translateX(0); /* 动画开始时位置 */
       }
       50% {
           transform: translateX(200px); /* 动画中间位置 */
       }
       100% {
           transform: translateX(0); /* 动画结束时位置 */
       }
   }

   .box {
       width: 100px;
       height: 100px;
       background-color: green;
       animation: move 2s infinite; /* 执行名为 'move' 的动画，持续时间2秒，循环播放 */
   }
   ```

# **Vue3.js 学习精简大纲**

下面是精简版本的 Vue3 必学核心知识点：

## **1. 快速上手：CDN 引入 Vue**

你可以直接通过 CDN 引入 Vue.js，这样不用安装任何环境，直接在 HTML 文件中使用。

```html
<!-- 在 HTML 文件中引入 Vue3 -->
<script src="https://cdn.jsdelivr.net/npm/vue@3.2.37/dist/vue.global.js"></script>
```

## **2. 模板语法：插值与指令**

### **2.1 插值**：用来显示数据。

  ```html
  <div id="app">
    <p>{{ message }}</p>
  </div>

  <script>
    const app = Vue.createApp({
      data() {
        return {
          message: 'Hello Vue 3!'
        }
      }
    });
    app.mount('#app');
  </script>
  ```

  解释：`{{ message }}` 用来显示 `data` 中的 `message` 数据。

### **2.2 常用指令**：

  * `v-if`：条件渲染
  * `v-for`：列表渲染
  * `v-bind`：绑定属性
  * `v-on`：事件监听



## **3. 响应式数据：`ref` / `reactive`**

在 Vue 3 中，响应式数据是一个非常重要的概念，它帮助我们在数据变化时自动更新视图。Vue 提供了两种主要的方式来创建响应式数据：`ref` 和 `reactive`。

### 3.1 `ref` 用于简单的基本类型数据

`ref` 主要用于简单的基本数据类型（如：`number`、`string`、`boolean`）。它使数据变得响应式，并且通过 `.value` 属性访问其值。

#### 示例：使用 `ref` 定义一个计数器

```javascript
// 导入 Vue
import { ref } from 'vue';

// 创建一个响应式的基本类型数据，初始值为 0
const count = ref(0);

// 在控制台输出 count 的值
console.log(count.value);  // 输出 0

// 修改 count 的值
count.value = 10;

// 在控制台输出修改后的值
console.log(count.value);  // 输出 10
```

#### 示例：`ref` 用于布尔值

```javascript
import { ref } from 'vue';

// 创建一个布尔值，表示是否已登录
const isLoggedIn = ref(false);

// 输出 isLoggedIn 的初始值
console.log(isLoggedIn.value);  // 输出 false

// 修改为 true
isLoggedIn.value = true;

// 输出修改后的值
console.log(isLoggedIn.value);  // 输出 true
```

#### 示例：`ref` 和模板的结合使用

```html
<template>
  <div>
    <p>当前计数：{{ count }}</p>
    <button @click="increment">增加计数</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// 使用 ref 定义响应式数据
const count = ref(0);

// 定义增加计数的函数
const increment = () => {
  count.value++;
};
</script>
```

### 3.2 `reactive` 用于对象或数组等复杂类型

`reactive` 主要用于复杂的数据类型（如对象、数组等），它会使整个对象变得响应式，直接访问和修改对象的属性时，视图会自动更新。

#### 示例：使用 `reactive` 定义一个对象

```javascript
import { reactive } from 'vue';

// 创建一个响应式对象，包含多个属性
const state = reactive({
  count: 0,
  message: 'Hello, Vue!',
  isActive: true
});

// 修改对象属性的值
state.count = 5;
state.message = 'Vue 3 is awesome!';
state.isActive = false;

// 输出修改后的对象属性
console.log(state.count);  // 输出 5
console.log(state.message);  // 输出 'Vue 3 is awesome!'
console.log(state.isActive);  // 输出 false
```

#### 示例：`reactive` 和模板的结合使用

```html
<template>
  <div>
    <p>计数：{{ state.count }}</p>
    <p>消息：{{ state.message }}</p>
    <p>状态：{{ state.isActive ? '活跃' : '不活跃' }}</p>
    <button @click="increment">增加计数</button>
    <button @click="toggleActive">切换状态</button>
  </div>
</template>

<script setup>
import { reactive } from 'vue';

// 使用 reactive 定义响应式对象
const state = reactive({
  count: 0,
  message: 'Hello, Vue!',
  isActive: true
});

// 增加计数的函数
const increment = () => {
  state.count++;
};

// 切换 isActive 状态的函数
const toggleActive = () => {
  state.isActive = !state.isActive;
};
</script>
```

#### 示例：使用 `reactive` 定义数组

```javascript
import { reactive } from 'vue';

// 创建一个响应式数组
const items = reactive([1, 2, 3]);

// 修改数组元素
items.push(4);
items[0] = 10;

// 输出修改后的数组
console.log(items);  // 输出 [10, 2, 3, 4]
```

### 3.3 `ref` 和 `reactive` 的比较

| 特性     | `ref`                                | `reactive`     |
| ------ | ------------------------------------ | -------------- |
| 适用数据类型 | 基本数据类型（`string`、`number`、`boolean`等） | 复杂数据类型（对象、数组等） |
| 数据访问方式 | 使用 `.value`                          | 直接访问属性         |
| 用法简便性  | 更简洁，适用于简单数据                          | 用于复杂数据时更方便     |

**使用场景：**

* **`ref`**：当你需要处理简单的基本类型（如计数器、布尔值等）时，可以使用 `ref`。
* **`reactive`**：当你需要处理对象或数组等复杂数据时，可以使用 `reactive`，因为它能够自动为对象的每个属性创建响应式引用。

### 3.4 小结

* **`ref`** 适用于处理基本类型的数据，使用 `.value` 来访问和修改数据。
* **`reactive`** 适用于处理对象或数组，可以直接操作对象的属性，Vue 会自动追踪数据的变化。

这些响应式数据是 Vue 3 中非常强大和重要的概念。掌握它们后，你可以更轻松地构建动态交互的 Vue 应用。

## **4. 事件处理：点击、输入等交互**

在 Vue.js 中，通过 `v-on` 指令来绑定事件处理函数。当用户与页面上的元素交互时，事件处理函数会被调用，通常用于修改数据、触发其他操作等。

### **4.1 绑定点击事件 (`v-on:click`)**

```html
<div id="app">
  <button v-on:click="increment">Increase</button>
  <p>Count: {{ count }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        count: 0 // 初始值为0
      };
    },
    methods: {
      increment() {
        this.count++; // 每点击一次按钮，count增加
      }
    }
  });

  app.mount('#app');
</script>
```

**解释：**

* `v-on:click="increment"`：当用户点击按钮时，触发 `increment` 方法。
* `increment()` 方法会将 `count` 增加 1。


### **4.2 事件处理函数中的参数传递**

你可以在事件处理函数中传递参数。在 Vue 中，`v-on` 指令可以通过 `v-on:eventName="method(param)"` 的方式传递参数。

```html
<div id="app">
  <button v-on:click="incrementBy(5)">Increase by 5</button>
  <p>Count: {{ count }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        count: 0
      };
    },
    methods: {
      incrementBy(amount) {
        this.count += amount; // 根据传入的参数增加count
      }
    }
  });

  app.mount('#app');
</script>
```

**解释：**

* `v-on:click="incrementBy(5)"`：点击按钮时，调用 `incrementBy(5)` 方法，将 `5` 作为参数传入方法。
* `incrementBy(amount)`：该方法通过传入的参数增加 `count` 值。


### **4.3 输入事件（`v-on:input`）**

当用户在输入框中输入内容时，可以使用 `v-on:input` 来监听 `input` 事件。

```html
<div id="app">
  <input v-on:input="updateMessage" placeholder="Type something...">
  <p>Your input: {{ message }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        message: '' // 初始为空字符串
      };
    },
    methods: {
      updateMessage(event) {
        this.message = event.target.value; // 更新 message 为输入框中的内容
      }
    }
  });

  app.mount('#app');
</script>
```

**解释：**

* `v-on:input="updateMessage"`：监听用户输入事件，每次输入时，都会调用 `updateMessage` 方法。
* `updateMessage(event)`：该方法接受输入事件对象 `event`，并通过 `event.target.value` 获取输入框的值，更新 `message`。

---

### **4.4. 使用 `v-model` 实现双向数据绑定**

在 Vue 中，`v-model` 可以实现输入框和数据的双向绑定，简化代码。

```html
<div id="app">
  <input v-model="message" placeholder="Type something...">
  <p>Your input: {{ message }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        message: '' // 初始为空字符串
      };
    }
  });

  app.mount('#app');
</script>
```

**解释：**

* `v-model="message"`：这行代码使输入框和 `message` 数据双向绑定。任何时候用户输入的内容都会自动更新 `message`，而 `message` 的变化也会反映到输入框中。


### **4.5 事件修饰符**

Vue 允许你使用事件修饰符来修改事件的行为，例如防止事件冒泡、取消默认行为等。

* `.prevent`：调用 `event.preventDefault()`，阻止默认事件行为。
* `.stop`：调用 `event.stopPropagation()`，阻止事件冒泡。
* `.capture`：指定事件在捕获阶段触发。
* `.once`：事件只能触发一次，触发后会自动解绑。

#### **示例：使用 `.prevent` 和 `.stop`**

```html
<div id="app">
  <form v-on:submit.prevent="submitForm">
    <button type="submit">Submit</button>
  </form>
  <p>{{ message }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        message: 'Please submit the form.'
      };
    },
    methods: {
      submitForm() {
        this.message = 'Form Submitted!';
      }
    }
  });

  app.mount('#app');
</script>
```

**解释：**

* `v-on:submit.prevent="submitForm"`：`.prevent` 修饰符会阻止表单的默认提交行为（即页面刷新），然后调用 `submitForm` 方法。
* 提交按钮点击后，`message` 会被更新为 `'Form Submitted!'`。


### **4.6 事件处理中的 `this` 上下文**

在 Vue 中，事件处理函数的 `this` 指向当前 Vue 实例，这样我们可以通过 `this` 来访问组件的 `data` 和 `methods`。

```html
<div id="app">
  <button v-on:click="resetCount">Reset Count</button>
  <p>Count: {{ count }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        count: 0
      };
    },
    methods: {
      resetCount() {
        this.count = 0; // 使用 this 来访问和修改 count
      }
    }
  });

  app.mount('#app');
</script>
```

**解释：**

* `resetCount()` 方法通过 `this.count` 将 `count` 重置为 0。

---

### **4.7 事件绑定与修饰符结合使用**

你还可以将事件修饰符与事件处理函数结合使用来增强功能。

```html
<div id="app">
  <button v-on:click.stop="clickHandler">Click Me</button>
  <p>{{ message }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        message: 'No click yet.'
      };
    },
    methods: {
      clickHandler() {
        this.message = 'Button clicked!';
      }
    }
  });

  app.mount('#app');
</script>
```

**解释：**

* `v-on:click.stop="clickHandler"`：使用 `.stop` 修饰符来阻止事件冒泡。点击按钮时，`clickHandler` 会被调用，并且点击事件不会冒泡到父级元素。

## **5. 条件渲染：`v-if` / `v-show`**

在 Vue.js 中，`v-if` 和 `v-show` 都是用于实现条件渲染的指令。它们的作用是根据某个条件判断，决定元素是否渲染到页面中。但它们之间有一些关键的区别，使用时需要根据实际场景来选择。

### 5.1 `v-if` 会根据条件决定是否渲染该元素

`v-if` 是 Vue 提供的一种指令，用于控制某个元素是否渲染。如果条件为 `true`，则渲染该元素；如果条件为 `false`，则完全不渲染该元素。

#### 示例 1：基础用法

```html
<template>
  <div>
    <!-- 当 isVisible 为 true 时，渲染该 <p> 元素 -->
    <p v-if="isVisible">This is visible</p>
    <!-- 当 isVisible 为 false 时，不会渲染该 <p> 元素 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 控制是否显示文本
      isVisible: true
    };
  }
};
</script>
```

#### 示例 2：条件为动态变量

你可以根据一个动态变量来控制 `v-if` 渲染与否。

```html
<template>
  <div>
    <button @click="toggleVisibility">Toggle Visibility</button>
    <!-- 当 isVisible 为 true 时，渲染该 <p> 元素 -->
    <p v-if="isVisible">This element can be toggled</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: false
    };
  },
  methods: {
    toggleVisibility() {
      // 切换 isVisible 的值
      this.isVisible = !this.isVisible;
    }
  }
};
</script>
```

在上面的例子中，当用户点击按钮时，`isVisible` 的值会在 `true` 和 `false` 之间切换，从而决定 `<p>` 元素是否渲染。

#### 示例 3：`v-if` 的延迟渲染

`v-if` 只会在条件为 `true` 时渲染该元素。如果条件变为 `false`，该元素会从 DOM 中移除，并且如果条件再变回 `true`，该元素会重新渲染。

```html
<template>
  <div>
    <button @click="isVisible = !isVisible">Toggle Element</button>
    <p v-if="isVisible">This element will be re-rendered each time it becomes visible.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: false
    };
  }
};
</script>
```

### 5.2 `v-show` 通过 CSS 控制元素的显示与隐藏（但始终会渲染到 DOM 中）

`v-show` 也用于控制元素的显示与隐藏，但它与 `v-if` 的工作原理不同。`v-show` 不会在条件为 `false` 时销毁该元素，而是通过 CSS 的 `display` 属性控制其显示与隐藏。元素始终会存在于 DOM 中，只是通过 `display: none` 隐藏。

#### 示例 1：基础用法

```html
<template>
  <div>
    <!-- 使用 v-show 控制显示 -->
    <p v-show="isVisible">This text is conditionally shown using v-show</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: true
    };
  }
};
</script>
```

#### 示例 2：`v-show` 动态控制显示与隐藏

```html
<template>
  <div>
    <button @click="toggleVisibility">Toggle Visibility</button>
    <p v-show="isVisible">This element can be shown or hidden without being re-rendered.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: true
    };
  },
  methods: {
    toggleVisibility() {
      // 切换 isVisible 的值
      this.isVisible = !this.isVisible;
    }
  }
};
</script>
```

在这个例子中，`v-show` 通过 `display: none` 来控制元素的显示与隐藏，当 `isVisible` 为 `false` 时，元素会被隐藏，但它依然在 DOM 中。

#### 示例 3：`v-show` 性能优化

由于 `v-show` 不会销毁元素，仅仅是隐藏或显示，适用于频繁切换显示与隐藏的场景。这样可以避免频繁地销毁和重新渲染 DOM 元素，提升性能。

```html
<template>
  <div>
    <button @click="toggleVisibility">Toggle Element</button>
    <p v-show="isVisible">This element is always in the DOM, just hidden or shown.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: true
    };
  },
  methods: {
    toggleVisibility() {
      // 切换 isVisible 的值
      this.isVisible = !this.isVisible;
    }
  }
};
</script>
```

### 5.3 `v-if` 与 `v-show` 的区别

1. **渲染机制：**

   * `v-if` 会根据条件决定是否渲染元素。条件为 `false` 时，元素不会渲染到 DOM 中。
   * `v-show` 则始终会渲染元素，但通过 CSS 控制其显示与隐藏，条件为 `false` 时，元素的 `display` 属性设置为 `none`。

2. **性能差异：**

   * `v-if` 适用于不频繁切换的条件渲染，因为它涉及到元素的销毁和重新渲染，性能开销较大。
   * `v-show` 适用于频繁切换显示/隐藏的场景，因为它只控制 CSS 样式，不会销毁和重建 DOM 元素，性能较好。

3. **使用场景：**

   * `v-if` 用于场景中，元素的显示与隐藏是较为罕见的，或者需要销毁和重新创建的情况。
   * `v-show` 用于场景中，元素需要频繁地切换显示与隐藏。

#### 总结：

* 使用 `v-if` 当：

  * 元素条件渲染较少，且需要销毁与重新渲染。
* 使用 `v-show` 当：

  * 元素条件渲染频繁，且只需通过 CSS 控制显示/隐藏，不需要销毁和重建。

## **6. 列表渲染：`v-for`**

`v-for` 是 Vue.js 中用于渲染列表的指令。它可以用于数组或对象，并且支持循环渲染列表项。

### 6.1 基本用法：遍历数组

```html
<ul>
  <!-- v-for 用于循环渲染 items 数组中的每一项 -->
  <li v-for="item in items" :key="item.id">{{ item.name }}</li>
</ul>

<script>
  const app = Vue.createApp({
    data() {
      return {
        items: [
          { id: 1, name: 'Item 1' },
          { id: 2, name: 'Item 2' },
          { id: 3, name: 'Item 3' }
        ]
      };
    }
  });

  app.mount('#app');
</script>
```

#### 解释：

* `v-for="item in items"`：表示遍历 `items` 数组，`item` 是数组中每一项的变量名称。
* `:key="item.id"`：为每个渲染的元素提供唯一的 `key`，这样 Vue 可以高效地更新和重排元素。这里使用 `item.id` 作为唯一标识。
* `{{ item.name }}`：在模板中插入 `item` 对象的 `name` 属性，显示每个元素的名字。

### 6.2 使用 `v-for` 遍历对象

如果你想遍历一个对象的属性，可以使用 `v-for` 遍历对象的键值对：

```html
<ul>
  <!-- v-for 用于遍历对象的键值对 -->
  <li v-for="(value, key) in user" :key="key">{{ key }}: {{ value }}</li>
</ul>

<script>
  const app = Vue.createApp({
    data() {
      return {
        user: {
          name: 'John',
          age: 25,
          email: 'john@example.com'
        }
      };
    }
  });

  app.mount('#app');
</script>
```

#### 解释：

* `v-for="(value, key) in user"`：遍历 `user` 对象，`value` 是对象的值，`key` 是对象的键。
* `:key="key"`：这里使用 `key` 作为每个列表项的唯一标识。
* `{{ key }}: {{ value }}`：显示键和值。

### 6.3 遍历数组并显示多个属性

你也可以在每个列表项中显示多个属性。假设你有一个包含多个属性的对象数组：

```html
<ul>
  <li v-for="(item, index) in items" :key="item.id">
    {{ index + 1 }}. {{ item.name }} (Age: {{ item.age }}, Email: {{ item.email }})
  </li>
</ul>

<script>
  const app = Vue.createApp({
    data() {
      return {
        items: [
          { id: 1, name: 'John', age: 25, email: 'john@example.com' },
          { id: 2, name: 'Jane', age: 30, email: 'jane@example.com' },
          { id: 3, name: 'Joe', age: 22, email: 'joe@example.com' }
        ]
      };
    }
  });

  app.mount('#app');
</script>
```

#### 解释：

* `index + 1`：输出列表的序号（从 1 开始）。`index` 是 `v-for` 提供的每项的索引。
* `{{ item.name }}`、`{{ item.age }}`、`{{ item.email }}`：显示每个 `item` 的不同属性。

### 6.4 条件渲染：`v-if` 与 `v-for` 结合使用

你可以在 `v-for` 中结合 `v-if` 来进行条件渲染。比如只渲染年龄大于 25 的用户：

```html
<ul>
  <li v-for="item in items" :key="item.id" v-if="item.age > 25">
    {{ item.name }} (Age: {{ item.age }})
  </li>
</ul>

<script>
  const app = Vue.createApp({
    data() {
      return {
        items: [
          { id: 1, name: 'John', age: 25 },
          { id: 2, name: 'Jane', age: 30 },
          { id: 3, name: 'Joe', age: 22 }
        ]
      };
    }
  });

  app.mount('#app');
</script>
```

#### 解释：

* `v-if="item.age > 25"`：只有年龄大于 25 的用户会被渲染出来。

### 6.5 在 `v-for` 中使用组件

你可以将 `v-for` 用于渲染组件列表，这样可以灵活管理每个项的显示。

```html
<user-card v-for="item in items" :key="item.id" :user="item"></user-card>

<script>
  const app = Vue.createApp({
    components: {
      'user-card': {
        props: ['user'],
        template: `
          <div>
            <h3>{{ user.name }}</h3>
            <p>Age: {{ user.age }}</p>
            <p>Email: {{ user.email }}</p>
          </div>
        `
      }
    },
    data() {
      return {
        items: [
          { id: 1, name: 'John', age: 25, email: 'john@example.com' },
          { id: 2, name: 'Jane', age: 30, email: 'jane@example.com' },
          { id: 3, name: 'Joe', age: 22, email: 'joe@example.com' }
        ]
      };
    }
  });

  app.mount('#app');
</script>
```

#### 解释：

* 在 `v-for` 中渲染 `user-card` 组件。每个 `item` 被传递给组件的 `user` prop。
* 组件通过 `props` 接收数据并渲染内容。

### 6.6 使用 `v-for` 渲染多维数组

如果你有一个包含子数组的数组，可以嵌套使用 `v-for` 来渲染多维数组：

```html
<ul>
  <li v-for="(group, index) in groups" :key="index">
    Group {{ index + 1 }}:
    <ul>
      <li v-for="item in group" :key="item.id">{{ item.name }}</li>
    </ul>
  </li>
</ul>

<script>
  const app = Vue.createApp({
    data() {
      return {
        groups: [
          [
            { id: 1, name: 'Item 1' },
            { id: 2, name: 'Item 2' }
          ],
          [
            { id: 3, name: 'Item 3' },
            { id: 4, name: 'Item 4' }
          ]
        ]
      };
    }
  });

  app.mount('#app');
</script>
```

#### 解释：

* 外层 `v-for="(group, index) in groups"`：遍历 `groups` 数组中的每个子数组（`group`）。
* 内层 `v-for="item in group"`：遍历每个子数组中的项（`item`）。

### 6.7 动态添加和删除列表项

你可以使用 `v-for` 渲染的列表项动态地添加和删除：

```html
<ul>
  <li v-for="(item, index) in items" :key="item.id">
    {{ item.name }} 
    <button @click="removeItem(index)">Remove</button>
  </li>
</ul>
<button @click="addItem">Add Item</button>

<script>
  const app = Vue.createApp({
    data() {
      return {
        items: [
          { id: 1, name: 'Item 1' },
          { id: 2, name: 'Item 2' }
        ]
      };
    },
    methods: {
      addItem() {
        const newItem = { id: Date.now(), name: `Item ${this.items.length + 1}` };
        this.items.push(newItem);
      },
      removeItem(index) {
        this.items.splice(index, 1);
      }
    }
  });

  app.mount('#app');
</script>
```

#### 解释：

* `addItem` 方法用于向 `items` 数组中添加新项。
* `removeItem` 方法用于从 `items` 数组中移除指定的项。


## **7. 表单绑定：`v-model`**

`v-model` 是 Vue.js 提供的一个指令，用于在表单输入元素（如 `<input>`、`<textarea>` 和 `<select>`）与 Vue 实例的数据之间建立双向绑定。通过 `v-model`，你可以简化表单元素和数据之间的交互，无需手动更新 DOM 和数据。

### **7.1 基本使用**

**双向绑定输入框的值**

```html
<div id="app">
  <!-- 双向绑定，输入框的值会自动与 message 数据绑定 -->
  <input v-model="message" placeholder="Type something">
  
  <!-- 显示 message 数据的实时变化 -->
  <p>{{ message }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        message: ''  // 初始化 message 数据为空字符串
      }
    }
  });
  app.mount('#app');
</script>
```

**解释：**

* `v-model="message"`：在 `<input>` 标签中，`v-model` 会将输入框的值与 `message` 数据进行双向绑定。
* 每当用户在输入框中输入内容，`message` 数据会随之变化。
* 每当 `message` 数据发生变化时，输入框的值会自动更新。

### **7.2 使用 `v-model` 与 `textarea`**

除了 `<input>` 标签外，`v-model` 同样适用于 `<textarea>` 标签，它会将多行文本框的值与 Vue 实例中的数据进行双向绑定。

```html
<div id="app">
  <!-- 多行文本输入框，绑定到 message -->
  <textarea v-model="message" placeholder="Type something here"></textarea>

  <!-- 显示 message 数据的实时变化 -->
  <p>{{ message }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        message: ''  // 初始化 message 数据为空字符串
      }
    }
  });
  app.mount('#app');
</script>
```

**解释：**

* `v-model="message"` 绑定到 `<textarea>` 标签，用户输入的内容会更新 `message` 数据，`message` 数据也会影响文本框内容。

### **7.3 使用 `v-model` 与 `select` 下拉框**

`v-model` 也可以与 `<select>` 标签配合使用，用于绑定选中的值。

```html
<div id="app">
  <!-- 下拉框绑定到 selectedOption -->
  <select v-model="selectedOption">
    <option disabled value="">Please select one</option>
    <option>Option 1</option>
    <option>Option 2</option>
    <option>Option 3</option>
  </select>

  <!-- 显示用户选择的选项 -->
  <p>Selected: {{ selectedOption }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        selectedOption: ''  // 初始化为空，表示没有选项被选中
      }
    }
  });
  app.mount('#app');
</script>
```

**解释：**

* `<select v-model="selectedOption">`：绑定 `<select>` 标签的选中值到 `selectedOption` 数据。
* 当用户选择不同的选项时，`selectedOption` 的值会更新。
* 你可以通过 `selectedOption` 来获取当前选中的值。

### **7.4 与复选框 (`checkbox`) 和单选框 (`radio`) 一起使用**

#### 7.4.1 复选框（Checkbox）

```html
<div id="app">
  <!-- 单个复选框绑定到 isChecked -->
  <input type="checkbox" v-model="isChecked"> Check me

  <!-- 显示复选框的选中状态 -->
  <p>Checked: {{ isChecked }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        isChecked: false  // 初始化复选框为未选中状态
      }
    }
  });
  app.mount('#app');
</script>
```

**解释：**

* `<input type="checkbox" v-model="isChecked">`：复选框的选中状态会与 `isChecked` 数据绑定。
* 当用户勾选或取消勾选时，`isChecked` 的值会自动更新为 `true` 或 `false`。

#### 7.4.2 单选框（Radio）

```html
<div id="app">
  <!-- 单选框组绑定到 selectedRadio -->
  <label>
    <input type="radio" v-model="selectedRadio" value="Option 1"> Option 1
  </label>
  <label>
    <input type="radio" v-model="selectedRadio" value="Option 2"> Option 2
  </label>
  <label>
    <input type="radio" v-model="selectedRadio" value="Option 3"> Option 3
  </label>

  <!-- 显示当前选择的单选框值 -->
  <p>Selected Radio: {{ selectedRadio }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        selectedRadio: ''  // 初始化为没有选中的单选框
      }
    }
  });
  app.mount('#app');
</script>
```

**解释：**

* 使用 `v-model="selectedRadio"` 将单选框的选中值绑定到 `selectedRadio` 数据。
* 当用户选择不同的选项时，`selectedRadio` 的值会更新为选中的 `value` 值。

### **7.5 自定义组件中的 `v-model`**

Vue.js 允许在自定义组件中使用 `v-model`，并且可以指定绑定的属性名。

**基本自定义组件**

```html
<div id="app">
  <!-- 自定义组件，使用 v-model 绑定 message -->
  <my-input v-model="message"></my-input>

  <!-- 显示 message -->
  <p>{{ message }}</p>
</div>

<script>
  const app = Vue.createApp({
    data() {
      return {
        message: 'Hello from parent!'  // 初始化数据
      }
    }
  });

  // 定义自定义组件
  app.component('my-input', {
    props: ['modelValue'],  // 接收父组件传递的 modelValue
    template: `
      <input :value="modelValue" @input="$emit('update:modelValue', $event.target.value)">
    `
  });

  app.mount('#app');
</script>
```

**解释：**

* 在自定义组件 `my-input` 中，使用 `props: ['modelValue']` 来接收父组件传递的 `v-model` 绑定值。
* 在模板中通过 `:value="modelValue"` 显示当前值，并使用 `@input="$emit('update:modelValue', $event.target.value)"` 在输入框内容改变时向父组件发出 `update:modelValue` 事件，从而实现双向绑定。

---

### **7.6 总结**

* `v-model` 是 Vue.js 中用于表单输入元素和 Vue 数据之间的双向绑定的指令。
* 它可以用于常见的表单元素，如 `<input>`、`<textarea>`、`<select>`、`<checkbox>` 和 `<radio>`，以及自定义组件中。
* 在使用 `v-model` 时，Vue 会自动处理数据和 DOM 之间的同步，无需手动更新。

## **8. 组件基础**

组件是 Vue 中的一个重要概念，用于将界面拆分成独立的小部分。通过组件化，可以提高代码的可维护性和重用性。Vue 组件可以封装模板、逻辑和样式，让开发者更加高效地开发复杂的界面。

### **8.1 创建组件**

组件是一个包含模板、逻辑和样式的小模块。你可以通过 Vue 的 `Vue.createApp` 创建一个应用，然后将组件注册到应用中。

#### **8.1.1 基本组件的创建**

首先，我们可以创建一个简单的 Vue 组件，包含一个简单的模板：

```javascript
// 定义一个名为 MyComponent 的组件
const MyComponent = {
  template: `<p>Hello from component!</p>`  // 组件的 HTML 模板
};

// 创建 Vue 应用
const app = Vue.createApp({
  components: {
    'my-component': MyComponent  // 注册组件
  }
});

// 挂载 Vue 应用到页面中的 #app 元素
app.mount('#app');
```

**解释：**

* `template`：Vue 组件的模板部分，定义了该组件的 HTML 结构。
* `components`：在 Vue 应用中注册子组件。在这里，我们将 `MyComponent` 注册为 `'my-component'`，然后在模板中使用 `<my-component></my-component>`。

#### **8.1.2 在模板中使用组件**

在 Vue 中注册组件后，你就可以在父组件的模板中使用它了：

```html
<div id="app">
  <!-- 使用已注册的子组件 -->
  <my-component></my-component>
</div>

<script>
  // 省略前面的代码，确保注册了 MyComponent
</script>
```

### **8.2 组件的 Props（传递数据）**

Vue 组件支持通过 `props` 来接收父组件传递的数据。可以将父组件的数据传递给子组件。

**在组件中使用 props**

```javascript
// 定义一个带有 props 的子组件
const MyComponent = {
  // 定义该组件接收一个名为 'message' 的 prop
  props: ['message'],
  template: `<p>{{ message }}</p>`  // 在模板中显示传递的 message
};

// 创建 Vue 应用
const app = Vue.createApp({
  components: {
    'my-component': MyComponent
  },
  data() {
    return {
      msg: 'Hello from parent component!'
    };
  }
});

// 挂载 Vue 应用
app.mount('#app');
```

**在模板中传递数据：**

```html
<div id="app">
  <!-- 传递 msg 给子组件 MyComponent -->
  <my-component :message="msg"></my-component>
</div>
```

**解释：**

* `props`：在子组件中声明用于接收数据的属性。
* `:message="msg"`：父组件通过 `v-bind`（简写为 `:`）将 `msg` 传递给子组件的 `message` prop。

### **8.3 组件的事件（自定义事件）**

子组件可以向父组件发送事件。通过 `$emit` 方法，子组件触发自定义事件，父组件可以监听这些事件并做出反应。

**子组件触发事件**

```javascript
const MyComponent = {
  template: `
    <button @click="sendMessage">Click me to send a message!</button>
  `,
  methods: {
    sendMessage() {
      // 使用 $emit 触发自定义事件
      this.$emit('message-sent', 'Hello from child component!');
    }
  }
};

const app = Vue.createApp({
  components: {
    'my-component': MyComponent
  },
  methods: {
    handleMessage(message) {
      console.log(message);  // 处理子组件发送的消息
    }
  }
});

app.mount('#app');
```

**在父组件中监听事件：**

```html
<div id="app">
  <!-- 监听来自子组件的 'message-sent' 事件 -->
  <my-component @message-sent="handleMessage"></my-component>
</div>
```

**解释：**

* `$emit('事件名', 数据)`：子组件触发名为 `message-sent` 的事件并传递数据。
* `@message-sent="handleMessage"`：父组件监听子组件触发的 `message-sent` 事件，并调用 `handleMessage` 方法。

### **8.4 组件的插槽（Slots）**

插槽是 Vue 提供的一种内容分发机制。允许在父组件模板中插入自定义内容，传递给子组件进行渲染。

#### **8.4.1 基本插槽**

```javascript
const MyComponent = {
  template: `
    <div>
      <h2>Child Component</h2>
      <slot></slot>  <!-- 默认插槽，显示父组件传入的内容 -->
    </div>
  `
};

const app = Vue.createApp({
  components: {
    'my-component': MyComponent
  }
});

app.mount('#app');
```

**父组件中插入内容：**

```html
<div id="app">
  <my-component>
    <p>This content is passed from the parent component!</p>
  </my-component>
</div>
```

**解释：**

* `<slot></slot>`：这是一个插槽，它允许父组件传递内容到子组件中显示。
* 父组件通过 `<my-component>...</my-component>` 传递内容到插槽中。

#### **8.4.2 具名插槽**

如果你有多个插槽，可以为每个插槽起个名字，来区分它们。

```javascript
const MyComponent = {
  template: `
    <div>
      <h2>Child Component</h2>
      <slot name="header"></slot>  <!-- 具名插槽 -->
      <slot></slot>  <!-- 默认插槽 -->
    </div>
  `
};

const app = Vue.createApp({
  components: {
    'my-component': MyComponent
  }
});

app.mount('#app');
```

**父组件中使用具名插槽：**

```html
<div id="app">
  <my-component>
    <template v-slot:header>
      <h3>This is a custom header!</h3>
    </template>
    <p>This is the default slot content.</p>
  </my-component>
</div>
```

**解释：**

* `v-slot:header` 用来给具名插槽传递内容。

### **8.5 组件的生命周期钩子**

Vue 组件有一系列的生命周期钩子，在组件的不同阶段会自动触发。常用的生命周期钩子有 `created`、`mounted`、`updated`、`destroyed` 等。

**使用生命周期钩子**

```javascript
const MyComponent = {
  data() {
    return {
      message: 'Hello!'
    };
  },
  created() {
    console.log('Component created');  // 组件创建时调用
  },
  mounted() {
    console.log('Component mounted');  // 组件挂载到 DOM 后调用
  },
  updated() {
    console.log('Component updated');  // 组件数据更新时调用
  },
  destroyed() {
    console.log('Component destroyed');  // 组件销毁时调用
  },
  template: `<p>{{ message }}</p>`
};

const app = Vue.createApp({
  components: {
    'my-component': MyComponent
  }
});

app.mount('#app');
```

**解释：**

* `created()`：组件实例化后立即调用，但此时组件的 DOM 还没有生成。
* `mounted()`：组件挂载到 DOM 后调用。
* `updated()`：组件的数据发生变化时调用。
* `destroyed()`：组件销毁时调用。

### **8.6 总结**

1. **组件化开发**：通过将界面拆分成组件，提升代码的组织性和可复用性。
2. **Props 和事件**：通过 `props` 传递数据，子组件通过 `$emit` 触发自定义事件与父组件通信。
3. **插槽**：使用插槽实现父组件向子组件传递自定义内容，支持默认插槽和具名插槽。
4. **生命周期钩子**：Vue 提供了丰富的生命周期钩子，帮助我们在组件的不同阶段执行特定操作。


# **Element Plus**

学会了 Vue 3 以后，恭喜你已经掌握了现代前端开发的“灵魂”。但是，如果所有的按钮、输入框、表格都从零开始写 CSS 和逻辑，那开发效率就太低了。

这时候，**Element Plus** 就像是一盒已经分类好的“高级乐高组件”。你不需要从塑料原料开始造积木，只需要学习如何把这些零件拼接在一起。

接下来，我们一步一步，从安装到常用组件，带你快速入门 🚀

## 1. 安装 Element Plus

### 1.1 安装依赖

```bash
npm install element-plus
```

### 1.2 在 main.js 中引入

```javascript
import { createApp } from 'vue'
import App from './App.vue'

// 引入 Element Plus
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

const app = createApp(App)

// 全局注册
app.use(ElementPlus)

app.mount('##app')
```

## 2. 按钮（Button）

### 2.1 基础按钮

```html
<template>
  <!-- type 控制按钮样式 -->
  <el-button>默认按钮</el-button>
  <el-button type="primary">主要按钮</el-button>
  <el-button type="success">成功按钮</el-button>
  <el-button type="danger">危险按钮</el-button>
</template>
```

### 2.2 带点击事件

```html
<template>
  <el-button type="primary" @click="handleClick">
    点击我
  </el-button>
</template>

<script setup>
const handleClick = () => {
  alert('按钮被点击了！')
}
</script>
```

## 3. 图标（Icon）

### 3.1 安装图标库

```bash
npm install @element-plus/icons-vue
```

### 3.2 使用图标

```html
<script setup>
// 导入图标
import { Edit, Delete } from '@element-plus/icons-vue'
</script>

<template>
  <!-- :icon 绑定图标 -->
  <el-button :icon="Edit">编辑</el-button>
  <el-button type="danger" :icon="Delete">删除</el-button>
</template>
```


## 4. 提示框（Message）

```html
<script setup>
import { ElMessage } from 'element-plus'

const showMessage = () => {
  ElMessage({
    message: '操作成功',
    type: 'success'
  })
}
</script>

<template>
  <el-button @click="showMessage">显示提示</el-button>
</template>
```

## 5. 导航菜单（Menu）

```html
<template>
  <el-menu default-active="1" class="el-menu-demo">
    
    <!-- 一级菜单 -->
    <el-menu-item index="1">首页</el-menu-item>

    <!-- 子菜单 -->
    <el-sub-menu index="2">
      <template ##title>商品管理</template>
      <el-menu-item index="2-1">商品列表</el-menu-item>
      <el-menu-item index="2-2">添加商品</el-menu-item>
    </el-sub-menu>

  </el-menu>
</template>
```

## 6. 标签页（Tabs）

```html
<script setup>
import { ref } from 'vue'

const activeName = ref('first')
</script>

<template>
  <el-tabs v-model="activeName">
    
    <el-tab-pane label="用户" name="first">
      用户内容
    </el-tab-pane>

    <el-tab-pane label="配置" name="second">
      配置内容
    </el-tab-pane>

  </el-tabs>
</template>
```

## 7. 输入框（Input）

```html
<script setup>
import { ref } from 'vue'

const input = ref('')
</script>

<template>
  <!-- v-model 双向绑定 -->
  <el-input v-model="input" placeholder="请输入内容" />
</template>
```

## 8. 单选框（Radio）

```html
<script setup>
import { ref } from 'vue'

const radio = ref('1')
</script>

<template>
  <el-radio-group v-model="radio">
    <el-radio label="1">选项一</el-radio>
    <el-radio label="2">选项二</el-radio>
  </el-radio-group>
</template>
```

## 9. 复选框（Checkbox）

```html
<script setup>
import { ref } from 'vue'

const checkList = ref(['A'])
</script>

<template>
  <el-checkbox-group v-model="checkList">
    <el-checkbox label="A">选项A</el-checkbox>
    <el-checkbox label="B">选项B</el-checkbox>
  </el-checkbox-group>
</template>
```


## 10. 下拉框（Select）

```html
<script setup>
import { ref } from 'vue'

const value = ref('')
</script>

<template>
  <el-select v-model="value" placeholder="请选择">
    
    <el-option label="苹果" value="apple" />
    <el-option label="香蕉" value="banana" />

  </el-select>
</template>
```

## 11. 日期选择器（DatePicker）

```html
<script setup>
import { ref } from 'vue'

const date = ref('')
</script>

<template>
  <el-date-picker
    v-model="date"
    type="date"
    placeholder="选择日期"
  />
</template>
```

## 12. 表单（Form）

```html
<script setup>
import { reactive } from 'vue'

const form = reactive({
  username: '',
  password: ''
})
</script>

<template>
  <el-form :model="form" label-width="80px">

    <el-form-item label="用户名">
      <el-input v-model="form.username" />
    </el-form-item>

    <el-form-item label="密码">
      <el-input v-model="form.password" type="password" />
    </el-form-item>

    <el-form-item>
      <el-button type="primary">提交</el-button>
    </el-form-item>

  </el-form>
</template>
```

## 13. 对话框（Dialog）

```html
<script setup>
import { ref } from 'vue'

const dialogVisible = ref(false)
</script>

<template>
  <!-- 打开按钮 -->
  <el-button @click="dialogVisible = true">打开对话框</el-button>

  <!-- 对话框 -->
  <el-dialog v-model="dialogVisible" title="提示">
    <span>这是一个对话框内容</span>

    <!-- 底部按钮 -->
    <template #footer>
      <el-button @click="dialogVisible = false">取消</el-button>
      <el-button type="primary">确认</el-button>
    </template>
  </el-dialog>
</template>
```

## 14. 分页（Pagination）

```html
<script setup>
import { ref } from 'vue'

const currentPage = ref(1)
</script>

<template>
  <el-pagination
    v-model:current-page="currentPage"
    :page-size="10"
    :total="100"
    layout="prev, pager, next"
  />
</template>
```

## 15. 表格（Table）

```html
<script setup>
const tableData = [
  { name: '小明', age: 18 },
  { name: '小红', age: 20 }
]
</script>

<template>
  <el-table :data="tableData" style="width: 100%">

    <!-- 列定义 -->
    <el-table-column prop="name" label="姓名" />
    <el-table-column prop="age" label="年龄" />

  </el-table>
</template>
```
# 实战项目（小试牛刀）

## 1. 创建项目（如果你还没有）

在终端执行：

```bash
# 创建一个 Vue + Vite 项目（项目名叫 element-demo）
npm create vite@latest element-demo

# 进入项目文件夹（必须执行，不然路径不对）
cd element-demo

# 安装项目基础依赖（Vue、Vite 等）
npm install


# ================= 安装 UI 组件库 =================

# 安装 Element Plus（UI组件库） + 图标库
npm install element-plus @element-plus/icons-vue


# ================= 启动项目 =================

# 启动开发服务器
npm run dev
```

## 2. 项目结构（你只用管这 2 个文件）

创建完后，你会看到：

```
element-demo/
├── index.html
├── package.json
├── src/
│   ├── main.js     👈 入口（要改）
│   ├── App.vue     👈 页面（要改）
│   └── assets/
```

## 3. 第一步：改 `main.js`

👉 打开：`src/main.js`
👉 **全部替换为：**

```javascript
import { createApp } from 'vue'
import App from './App.vue'

// ✅ 引入 Element Plus
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

const app = createApp(App)

// ✅ 使用组件库
app.use(ElementPlus)

app.mount('#app')
```

## 4. 第二步：改 `App.vue`（核心）

👉 打开：`src/App.vue`
👉 **全部删掉 → 粘贴下面完整代码**

```html
<template>
  <div style="padding: 20px">

    <h1>Element Plus 综合演示</h1>

    <!-- 按钮 -->
    <h2>按钮</h2>
    <el-button>默认</el-button>
    <el-button type="primary" @click="handleClick">主要按钮</el-button>
    <el-button type="success">成功</el-button>
    <el-button type="danger">危险</el-button>

    <!-- 图标 -->
    <h2>图标</h2>
    <el-button :icon="Edit">编辑</el-button>
    <el-button :icon="Delete" type="danger">删除</el-button>

    <!-- 提示 -->
    <h2>提示</h2>
    <el-button @click="showMessage">点击提示</el-button>

    <!-- 输入框 -->
    <h2>输入框</h2>
    <el-input v-model="input" placeholder="输入点什么" />

    <!-- 单选 -->
    <h2>单选</h2>
    <el-radio-group v-model="radio">
      <el-radio label="1">选项1</el-radio>
      <el-radio label="2">选项2</el-radio>
    </el-radio-group>

    <!-- 复选 -->
    <h2>复选</h2>
    <el-checkbox-group v-model="checkList">
      <el-checkbox label="A">A</el-checkbox>
      <el-checkbox label="B">B</el-checkbox>
    </el-checkbox-group>

    <!-- 下拉 -->
    <h2>下拉框</h2>
    <el-select v-model="select" placeholder="请选择">
      <el-option label="苹果" value="apple" />
      <el-option label="香蕉" value="banana" />
    </el-select>

    <!-- 日期 -->
    <h2>日期选择</h2>
    <el-date-picker v-model="date" type="date" />

    <!-- Tabs -->
    <h2>标签页</h2>
    <el-tabs v-model="tab">
      <el-tab-pane label="用户" name="1">用户内容</el-tab-pane>
      <el-tab-pane label="设置" name="2">设置内容</el-tab-pane>
    </el-tabs>

    <!-- 表单 -->
    <h2>表单</h2>
    <el-form :model="form" label-width="80px">
      <el-form-item label="用户名">
        <el-input v-model="form.username" />
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="form.password" type="password" />
      </el-form-item>
      <el-button type="primary" @click="submitForm">提交</el-button>
    </el-form>

    <!-- 对话框 -->
    <h2>对话框</h2>
    <el-button @click="dialog = true">打开</el-button>

    <el-dialog v-model="dialog" title="提示">
      这是一个弹窗
      <template #footer>
        <el-button @click="dialog = false">取消</el-button>
        <el-button type="primary" @click="dialog = false">确定</el-button>
      </template>
    </el-dialog>

    <!-- 表格 -->
    <h2>表格</h2>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="name" label="姓名" />
      <el-table-column prop="age" label="年龄" />
    </el-table>

    <!-- 分页 -->
    <h2>分页</h2>
    <el-pagination
      v-model:current-page="page"
      :page-size="5"
      :total="20"
    />

  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { ElMessage } from 'element-plus'
import { Edit, Delete } from '@element-plus/icons-vue'

// 按钮
const handleClick = () => alert('点了按钮')

// 提示
const showMessage = () => {
  ElMessage.success('成功！')
}

// 输入框
const input = ref('')

// 单选
const radio = ref('1')

// 复选
const checkList = ref(['A'])

// 下拉
const select = ref('')

// 日期
const date = ref('')

// Tabs
const tab = ref('1')

// 表单
const form = reactive({
  username: '',
  password: ''
})

const submitForm = () => {
  console.log(form)
}

// 对话框
const dialog = ref(false)

// 表格
const tableData = ref([
  { name: '小明', age: 18 },
  { name: '小红', age: 20 }
])

// 分页
const page = ref(1)
</script>
```

## 5. 你现在应该看到什么？

打开浏览器：

👉 [http://localhost:5173](http://localhost:5173)

你会看到一个页面，包含：

* 按钮（能点）
* 输入框（能输入）
* 弹窗（能弹）
* 表格（有数据）
* 分页（能点）

👉 这就说明：**你已经成功入门 Element Plus 了**


## 6. 如果失败，检查这 3 个点

1️⃣ 有没有安装：

```bash
npm install element-plus @element-plus/icons-vue
```

2️⃣ `main.js` 有没有：

```js
app.use(ElementPlus)
```

3️⃣ 控制台有没有报错（F12）


## 7. 最重要一句话

👉 你现在的操作路径是：

> 改 `main.js` + 覆盖 `App.vue` → 直接运行