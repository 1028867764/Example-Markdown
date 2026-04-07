# 前言

既然你已经有了 Flutter 基础，学习 **Jaspr** 会非常亲切。Jaspr 的核心理念就是：**用开发 Flutter 的逻辑，去写高性能、SEO 友好的原生 HTML/CSS 网页。**

简单来说，Flutter Web 是在 Canvas 上“画”像素，而 Jaspr 是把 Dart 代码“翻译”成真正的网页标签。


- **核心概念对比：从 Flutter 到 Jaspr**

在开始前，你需要先转换几个核心思维：

| 特性 | Flutter (Web) | Jaspr |
| :--- | :--- | :--- |
| **渲染方式** | Canvas/Skia (画像素) | **HTML + CSS (原生 DOM)** |
| **UI 组件** | `Widget` | **`Component`** |
| **基础单位** | `Container`, `Row`, `Column` | **`html.div`, `html.p`, `html.span`** |
| **布局逻辑** | 自身的布局引擎 | **CSS (Flexbox, Grid)** |
| **SEO** | 极差 (内容在 Canvas 里) | **优秀 (原生 HTML 文本)** |


# 1. 环境搭建与起步
* **CLI 安装**：`dart pub global activate jaspr_cli`
* **创建项目**：`jaspr create my_website`
* **渲染模式选择**：
    * **Static**：静态网站生成（SSG）。
    * **Server**：带服务端渲染（SSR）的动态网站。
    * **Client**：纯客户端渲染（类似 React/Vue SPA）。



# 2. 组件系统 (The Component Model)

Jaspr 的组件系统设计灵感来源于 **Flutter**，所以如果你熟悉 Flutter，会感觉非常亲切！  
在 Flutter 中，我们用 **Widget** 来构建界面；在 Jaspr 中，我们使用 **Component**（组件）来构建网页。

Jaspr 提供了三种基础组件类型，与 Flutter 高度相似，但有一些 Web 友好的调整：

## 2.1 StatelessComponent（无状态组件）
- **等同于** Flutter 中的 `StatelessWidget`。
- 特点：组件一旦创建，内部状态就不会改变（不可变）。
- 适合用于纯展示型界面，比如标题、按钮样式、静态列表等。
- 只需要重写 `build` 方法即可。

**简单示例（小白友好版）：**

```dart
import 'package:jaspr/jaspr.dart';

/// 一个简单的无状态组件，显示欢迎文字
class HelloWorld extends StatelessComponent {
  const HelloWorld({super.key});   // 构造函数，key 用于优化渲染（类似 Flutter）

  @override
  Iterable<Component> build(BuildContext context) sync* {
    // 使用 sync* + yield 返回多个子组件（这是 Jaspr 的特色！）
    yield h1([text('欢迎来到 Jaspr！')]);
    yield p([text('这是一个无状态组件的演示。')]);
  }
}
```

**说明**：
- `sync*` 表示这是一个**同步生成器函数**，允许你用 `yield` 一次返回多个组件。
- `h1()`、`p()`、`text()` 都是 Jaspr 内置的 HTML 组件（对应 `<h1>`、`<p>`、文本节点）。

## 2.2 StatefulComponent（有状态组件）
- **等同于** Flutter 中的 `StatefulWidget` + `State`。
- 特点：拥有可变的内部状态，可以通过 `setState()` 触发界面更新。
- 生命周期方法几乎完全一样：`initState()`、`didUpdateComponent()`、`dispose()` 等。
- 需要两个类：
  1. `StatefulComponent` 子类（负责创建 State）
  2. `State<T>` 子类（真正保存状态和写 `build` 方法）

**完整示例（计数器 - 经典小白入门案例）：**

```dart
import 'package:jaspr/jaspr.dart';

/// 1. 有状态组件本身（类似 StatefulWidget）
class Counter extends StatefulComponent {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

/// 2. 状态类（类似 State<Counter>）
class _CounterState extends State<Counter> {
  int count = 0;   // 可变状态

  // 初始化时调用（类似 Flutter 的 initState）
  @override
  void initState() {
    super.initState();
    print('计数器组件初始化了！');
  }

  // 清理时调用（类似 dispose）
  @override
  void dispose() {
    print('计数器组件被销毁了！');
    super.dispose();
  }

  // 点击按钮时调用，更新状态并重新渲染
  void increment() {
    setState(() {        // 关键！告诉 Jaspr 需要重新 build
      count++;
    });
  }

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div(classes: 'counter', [
      p([text('当前计数：$count')]),           // 显示状态
      button(
        onClick: (_) => increment(),          // 点击事件
        [text('点我 +1')],
      ),
    ]);
  }
}
```

**小贴士**：
- `setState(() { ... })` 是触发重绘的核心，和 Flutter 一模一样。
- 如果组件需要在客户端交互（点击、输入等），通常需要在类上方加上 `@client` 注解（Jaspr 的 SSR 特性）：
  ```dart
  @client
  class Counter extends StatefulComponent { ... }
  ```

## 2.3 Build 方法的变化（最重要区别！）

**Flutter 中**：
- `build` 方法**必须返回单个 `Widget`**。
- 如果想放多个子元素，需要用 `Column`、`Row`、`ListView` 等包装。

**Jaspr 中**：
- `build` 方法通常返回 **`Iterable<Component>`**（可迭代的组件集合）。
- **为什么这样设计？**  
  因为网页的 HTML 元素天生就可以有多个直接子节点（不像 Flutter 的 Widget 树必须有一个根 Widget）。这样更自然、更灵活，不需要每次都包一层 `Column`。

**推荐写法**（使用 `sync*` 和 `yield`）：

```dart
@override
Iterable<Component> build(BuildContext context) sync* {
  yield div(classes: 'header', [
    text('这是头部'),
  ]);

  yield p([text('这是段落内容')]);

  // 可以 yield 很多个，Jaspr 会自动把它们作为兄弟节点渲染
  for (var i = 0; i < 3; i++) {
    yield p([text('列表项 $i')]);
  }
}
```

**也可以直接返回单个组件**（如果只需要一个根元素）：

```dart
@override
Component build(BuildContext context) {
  return div([
    h1([text('单个根组件')]),
    p([text('内容')]),
  ]);
}
```

**小白常见问题解答**：
- **为什么不能直接 return 一个 List？**  
  因为 `Iterable` 更灵活，支持生成器（`sync*`），性能更好。
- **yield 和 return 有什么区别？**  
  `yield` 一次可以“吐出”一个组件，函数可以继续执行；`return` 直接结束函数。
- **从 Flutter 迁移时最需要注意什么？**  
  把 `Widget` 改成 `Component`，把 `return Column(children: [...])` 改成 `sync* + yield` 或直接用 `div([...])` 包裹。

**总结**：
Jaspr 的组件系统让 Flutter 开发者几乎零学习成本就能上手写网页。  
核心就是：**StatelessComponent ≈ StatelessWidget**，**StatefulComponent ≈ StatefulWidget**，但 `build` 返回 `Iterable<Component>` 更贴合 Web 的多子元素特性。

掌握了这两个组件，你就已经能构建大部分交互式网页了！  
下一节我们会继续学习如何使用内置的 HTML 组件（如 `div`、`button`、`input` 等）和样式系统。

---

**练习建议**（给小白）：
1. 先把上面的 `HelloWorld` 和 `Counter` 复制到你的 Jaspr 项目中运行。
2. 尝试把 Counter 改成显示两个数字的版本（用多个 `yield`）。
3. 把一个 Flutter 的简单页面尝试用 Jaspr 重写，感受 `build` 返回 Iterable 的便利。



# 3. HTML 标签与样式

Jaspr 的核心思想是：**你写的 Dart 代码最终会变成真实的 HTML 和 CSS**，而不是像 Flutter Web 那样用 Canvas 绘制。这意味着你的网站对 SEO 友好、加载快、可以直接被浏览器理解。

在 Jaspr 中，你不需要写 `<div>` 这样的 HTML 字符串，而是用 Dart 函数（如 `div()`）来创建组件。这些函数来自 `package:jaspr/html.dart`。

## 3.1 导入 HTML 组件

首先，在你的组件文件中导入 HTML 工具：

```dart
import 'package:jaspr/jaspr.dart';
import 'package:jaspr/html.dart';   // ← 关键导入，里面有 div、img、a 等所有常用标签
```

## 3.2 原生 HTML 标签组件（最常用的一批）

Jaspr 为几乎所有标准 HTML 标签都提供了对应的 Dart 函数。它们都接受以下常见参数：

- `children`：子组件列表（`List<Component>`）
- `id`：HTML id 属性（String?）
- `classes`：CSS 类名（String?，多个类用空格分隔，如 `'btn primary'`）
- `styles`：内联样式（`Styles` 类型，强烈推荐）
- `attributes`：其他 HTML 属性（如 `{'data-id': '123'}`）
- `events`：事件处理（如 `onClick`、`onMouseEnter` 等）
- 部分标签有专属参数（如 `img` 有 `src`、`alt`；`a` 有 `href`）

### 3.2.1 结构与容器类标签

```dart
// 最常用的容器：div（相当于 Flutter 的 Container）
div(classes: 'container', styles: Styles.box(...), [
  // 子元素
]);

// 语义化标签（推荐使用，提升 SEO 和无障碍访问）
section(id: 'about', [
  h1([text('关于我们')]),
  p([text('这是段落内容...')]),
]);

article([
  h2([text('文章标题')]),
  p([text('正文...')]),
]);

nav([
  ul([
    li([a(href: '/', [text('首页')])]),
    li([a(href: '/about', [text('关于')])]),
  ]),
]);

header([ /* 网站头部 */ ]);
footer([ /* 网站底部 */ ]);
```

### 3.2.2 文本与标题类

```dart
// 纯文本必须用 text() 包裹（Jaspr 会把它转成文本节点）
text('你好，Jaspr！'),

// 标题标签（h1 ~ h6）
h1([text('一级标题')]),
h2([text('二级标题')]),
h3([text('三级标题')]),

// 强调、加粗、斜体等
p([
  text('普通文字 '),
  strong([text('加粗文字')]),
  text(' '),
  em([text('斜体文字')]),
  text(' '),
  b([text('粗体')]),
  i([text('斜体')]),
  u([text('下划线')]),
]);

// 其他文本标签
span([text('行内元素')]),   // 常用于局部样式
small([text('小字备注')]),
mark([text('高亮')]),
```

### 3.2.3 图片与多媒体

```dart
// img 标签（强烈建议加上 alt 属性，提升可访问性）
img(
  src: 'https://example.com/photo.jpg',
  alt: '一张美丽的照片',
  width: 300,           // 可选，单位 px
  height: 200,
  styles: Styles.box(
    borderRadius: BorderRadius.all(Radius.circular(12.px)),
  ),
),

// 响应式图片（picture + source）
picture([
  source(srcset: 'large.jpg', media: '(min-width: 800px)'),
  img(src: 'small.jpg', alt: '响应式图片'),
]),

// 视频（video）
video(
  src: 'video.mp4',
  controls: true,       // 显示播放控件
  autoplay: false,
  loop: false,
  [
    // 后备文本
    text('您的浏览器不支持视频标签'),
  ],
),

// 音频（audio）
audio(src: 'music.mp3', controls: true),
```

### 3.2.4 链接与按钮

```dart
// 超链接 a
a(
  href: 'https://jaspr.site',
  target: Target.blank,     // 在新标签页打开
  rel: 'noopener',          // 安全属性
  [
    text('访问 Jaspr 官网'),
    // 可以放图标等子元素
    span([text(' →')]),
  ],
),

// 按钮（推荐用 button 而不是 div 模拟）
button(
  classes: 'btn btn-primary',
  onClick: () {
    print('按钮被点击了！');
    // 可以在这里处理状态或导航
  },
  [
    text('点击我'),
  ],
),

// 禁用按钮示例
button(
  disabled: true,
  [
    text('已禁用'),
  ],
),
```

### 3.2.5 列表

```dart
// 无序列表 ul + li
ul(classes: 'menu', [
  li([a(href: '#', [text('菜单项 1')])]),
  li([a(href: '#', [text('菜单项 2')])]),
]);

// 有序列表 ol
ol([
  li([text('第一步')]),
  li([text('第二步')]),
]);
```

### 3.2.6 表单相关（非常重要）

```dart
form(
  action: '/submit',
  method: 'post',
  [
    // 输入框
    label(forId: 'username', [text('用户名：')]),
    input(
      type: InputType.text,
      id: 'username',
      name: 'username',
      placeholder: '请输入用户名',
      required: true,
    ),

    // 密码框
    input(
      type: InputType.password,
      placeholder: '密码',
    ),

    // 复选框
    input(
      type: InputType.checkbox,
      id: 'agree',
      [
        text('我同意条款'),
      ],
    ),

    // 单选按钮
    input(
      type: InputType.radio,
      name: 'gender',
      value: 'male',
    ),
    text('男'),

    // 下拉选择
    select(
      name: 'city',
      [
        option(value: 'bj', [text('北京')]),
        option(value: 'sh', [text('上海')]),
        option(value: 'gz', [text('广州')], selected: true),
      ],
    ),

    // 文本域
    textarea(
      placeholder: '请输入留言...',
      rows: 5,
      [
        // 默认值可以放子文本
      ],
    ),

    // 提交按钮
    button(
      type: ButtonType.submit,
      [
        text('提交表单'),
      ],
    ),
  ],
),
```

### 3.2.7 表格（Table）

```dart
table([
  thead([
    tr([
      th([text('姓名')]),
      th([text('年龄')]),
      th([text('城市')]),
    ]),
  ]),
  tbody([
    tr([
      td([text('小明')]),
      td([text('25')]),
      td([text('北京')]),
    ]),
    tr([
      td([text('小红')]),
      td([text('23')]),
      td([text('上海')]),
    ]),
  ]),
]),
```

### 3.2.8 其他常用标签

- `br()` → 换行
- `hr()` → 水平分割线
- `blockquote()` → 引用块
- `pre()` + `code()` → 代码块
- `iframe()` → 嵌入其他页面/视频（如 YouTube）
- `canvas()` → 画布（配合 JS 或 Dart 绘图）

**低级方式创建任意标签**（当内置函数不够用时）：

```dart
DomComponent(
  tag: 'custom-tag',   // 或任何 HTML 标签
  id: 'my-element',
  classes: 'custom-class',
  styles: Styles(...),
  attributes: {'data-custom': 'value'},
  children: [text('内容')],
);
```

## 3.3 Styles 属性（类型安全的 CSS）

### 3.3.1 概述

Jaspr **不使用** Flutter 的 `TextStyle`、`BoxDecoration` 等，而是提供 `Styles` 类，直接对应 CSS 属性。

Jaspr 是一个用 Dart 写 Web 的框架，它的样式系统（`Styles`）设计得非常友好，像 Flutter 一样用代码写 CSS，但底层是原生 HTML + CSS。

`Styles` 类把常见的 CSS 属性按功能**分组**，这样写起来更清晰、容易链式调用（chain），不容易写乱。

推荐做法：**链式调用**不同的分组，然后用 `Styles.combine([...])` 把它们合并成一个 `Styles` 对象，传给组件的 `styles` 参数。

```dart
// 示例：一个红色正方形，里面有粗体文字
div(
  styles: Styles.combine([                  // combine 可以合并多个样式组
    Styles.box(                             // 盒模型相关
      width: 200.px,
      height: 200.px,
      backgroundColor: Colors.red,
      borderRadius: BorderRadius.all(Radius.circular(16.px)),
      padding: Padding.all(20.px),
      margin: Margin.all(10.px),
    ),
    Styles.text(                            // 文本相关
      fontSize: 18.px,
      fontWeight: FontWeight.bold,
      color: Colors.white,
      textAlign: TextAlign.center,
    ),
    Styles.flex(                            // Flex 布局
      direction: FlexDirection.column,
      alignItems: AlignItems.center,
      justifyContent: JustifyContent.center,
    ),
  ]),
  [
    text('你好，Jaspr！'),
    img(src: 'icon.png', alt: '图标'),
  ],
);
```

### 3.3.2 `Styles.box()` —— 盒子模型相关（最常用）

负责尺寸、外边距、内边距、边框、圆角、阴影等。

```dart
import 'package:jaspr/jaspr.dart';

final boxStyle = Styles.box(
  // 宽度和高度（支持 px, rem, %, 等 Unit）
  width: 300.px,           // 300 像素宽
  height: 200.px,          // 200 像素高
  margin: EdgeInsets.all(16.px),   // 外边距四周 16px
  padding: EdgeInsets.symmetric(horizontal: 24.px, vertical: 16.px), // 左右 24px，上下 16px
  border: Border.all(      // 边框
    color: Colors.grey,
    width: 2.px,
  ),
  borderRadius: BorderRadius.all(Radius.circular(12.px)), // 圆角
  boxShadow: BoxShadow(    // 阴影
    color: Colors.black.withOpacity(0.1),
    blurRadius: 10.px,
    offset: Offset(0.px, 4.px),
  ),
);
```

**使用示例**：

```dart
div(
  styles: boxStyle,
  [text('这是一个带样式的盒子')],
);
```

### 3.3.3 `Styles.text()` —— 文字样式

控制字体大小、粗细、颜色、对齐、行高等等。

```dart
final textStyle = Styles.text(
  fontSize: 18.px,                    // 字体大小
  fontWeight: FontWeight.w600,        // 半粗体（600）
  color: Colors.blue.shade700,        // 文字颜色
  textAlign: TextAlign.center,        // 水平居中
  lineHeight: 1.6,                    // 行高（倍数）
  letterSpacing: 0.5.px,              // 字间距
);
```

**使用示例**（可以和 box 组合）：

```dart
p(
  styles: Styles.combine([boxStyle, textStyle]),  // 合并使用
  [text('这是一段居中、蓝色、带行高的文字')],
);
```

### 3.3.4 `Styles.background()` —— 背景样式

背景颜色、背景图片、渐变等。

```dart
final bgStyle = Styles.background(
  color: Colors.white,                    // 背景色
  // image: BackgroundImage.url('https://example.com/bg.jpg'), // 背景图片（可选）
  // size: BackgroundSize.cover,           // 图片覆盖方式
);
```

**小提示**：背景颜色最常用，图片等高级用法可以再查文档。

### 3.3.5 `Styles.flex()` / `Styles.grid()` —— 布局系统（超级重要！）

- `Styles.flex()`：弹性布局（Flexbox）
- `Styles.grid()`：网格布局（Grid）

```dart
// Flex 布局示例（水平排列）
final flexStyle = Styles.flex(
  direction: FlexDirection.row,           // 主轴方向：横向
  wrap: FlexWrap.wrap,                    // 允许换行
  justifyContent: JustifyContent.spaceBetween, // 两端对齐
  alignItems: AlignItems.center,          // 交叉轴居中
  gap: Gap.all(16.px),                    // 子元素间距
);

// Grid 布局示例
final gridStyle = Styles.grid(
  columns: GridTemplate.repeat(3, GridTrackSize.fr(1)), // 3 列，等宽
  gap: Gap.all(12.px),
);
```

**使用示例**（做一个卡片列表）：

```dart
div(
  styles: Styles.combine([flexStyle, boxStyle]),  // 合并 flex + box
  [
    div(styles: boxStyle, [text('卡片1')]),
    div(styles: boxStyle, [text('卡片2')]),
    div(styles: boxStyle, [text('卡片3')]),
  ],
);
```

### 3.3.6 `Styles.position()` —— 定位

用于绝对定位、固定定位、层级等。

```dart
final positionStyle = Styles.position(
  position: Position.absolute,   // 绝对定位
  top: 20.px,
  left: 30.px,
  zIndex: 10,                    // 层级（数值越大越靠前）
);
```

### 3.3.7 `Styles.transition()` —— 过渡动画

让样式变化更平滑。

```dart
final transitionStyle = Styles.transition(
  property: 'all',                     // 所有属性都过渡
  duration: Duration(milliseconds: 300), // 过渡时间 300ms
  timingFunction: Curves.easeInOut,    // 缓动函数
);
```

**实用技巧**： hover 时改变样式 + transition 会很丝滑。

### 3.3.8 `Styles.raw()` —— 自定义或不常见属性

当内置分组没有你想要的属性时，用这个直接写 CSS 属性名。

```dart
final customStyle = Styles.raw({
  '--custom-color': '#ff6600',           // CSS 自定义变量
  'scrollbar-width': 'thin',             // 自定义滚动条（不常见属性）
});
```

### 3.3.9 最常用的合并方式：`Styles.combine([...])`

单独的分组用起来不方便，通常把多个分组**合并**成一个 `Styles`。

```dart
final cardStyle = Styles.combine([
  Styles.box(                          // 1. 盒子模型
    width: 320.px,
    padding: EdgeInsets.all(20.px),
    borderRadius: BorderRadius.all(Radius.circular(16.px)),
    boxShadow: BoxShadow(...),
  ),
  Styles.text(                         // 2. 文字样式
    fontSize: 16.px,
    color: Colors.black87,
  ),
  Styles.background(                   // 3. 背景
    color: Colors.white,
  ),
  Styles.transition(                   // 4. 动画
    property: 'all',
    duration: Duration(milliseconds: 200),
  ),
]);
```

**完整组件使用示例**：

```dart
div(
  styles: cardStyle,                   // 直接传合并后的样式
  [
    h3(styles: Styles.text(fontSize: 20.px, fontWeight: FontWeight.bold), [text('卡片标题')]),
    p([text('这是卡片内容...')]),
  ],
);
```

### 3.3.10 小白上手建议

1. **先掌握 `Styles.box()` + `Styles.text()`** —— 这两个能覆盖 80% 的日常需求。
2. **布局用 `Styles.flex()`** —— 大多数卡片、导航、列表都靠它。
3. **永远用 `Styles.combine()`** 合并，不要把所有属性挤到一个 `Styles.box()` 里（虽然可以，但分组更清晰）。
4. **颜色**：推荐用 `Colors` 类（比如 `Colors.blue`、`Colors.grey.shade500`），支持透明度 `withOpacity()`。
5. **单位**：常用 `.px`、`.rem`、`.percent`（百分比）。

想看更多实际案例，可以去 Jaspr 官方文档的 [**Styling**](https://docs.jaspr.site/concepts/styling) 部分，或者在 JasprPad 上直接实验这些代码。

## 3.4 外部 CSS 引入（推荐结合 Tailwind 等工具）

### 方式一：在 `web/index.html` 中引入

```html
<!-- web/index.html -->
<head>
  <link rel="stylesheet" href="/assets/styles.css">
  <!-- 或引入 CDN -->
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@latest/dist/tailwind.min.css" rel="stylesheet">
</head>
```

然后在 Jaspr 组件中使用 `classes`:

```dart
div(classes: 'bg-red-500 text-white p-8 rounded-xl', [
  text('使用 Tailwind 类'),
]);
```

### 方式二：使用官方/社区 Tailwind 集成（强烈推荐给小白）

1. 添加 dev 依赖：`dart pub add jaspr_tailwind --dev`
2. 在 `web` 目录创建 `styles.tw.css`：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

3. 在 `index.html` 引入生成的 CSS。
4. 组件中使用 Tailwind 类（如 `btn btn-primary px-4 py-2`）。

这样你就可以用熟悉的工具类快速开发，同时保留 Dart 的类型安全。

## 3.5 小贴士（小白必看）

- **语义化优先**：尽量用 `section`、`article`、`nav`、`header` 等代替一堆 `div`，对 SEO 和屏幕阅读器友好。
- **事件处理**：大多数组件支持 `onClick`、`onChange`、`onSubmit` 等，直接写 Dart 函数。
- **响应式**：结合 Tailwind 的 `md:``lg:` 前缀，或用 MediaQuery（Jaspr 支持）。
- **组件化**：把重复的 HTML 结构封装成自己的 `StatelessComponent` 或 `StatefulComponent`，就像 Flutter 的 Widget 一样。
- **调试**：在浏览器开发者工具里看生成的 HTML，非常干净。

掌握了这些 HTML 组件和 Styles，你就已经能用 Jaspr 搭建几乎任何静态或动态网页了！

接下来我们会学习如何创建自己的可复用 Component，以及状态管理。

**练习建议**：
1. 尝试用 `header` + `nav` + `main` + `footer` 搭建一个完整页面骨架。
2. 用 `flex` 或 `grid` 实现一个响应式卡片列表。
3. 结合 Tailwind 快速做出一个现代登录表单。



# 4. 交互与状态管理

Jaspr 虽然支持服务端渲染（SSR），但网页最终需要在浏览器中实现**交互**（点击、输入、动画等）。这一节我们来学习如何处理用户事件、管理组件状态，以及如何让组件在浏览器端“激活”交互。

Jaspr 的交互方式非常接近 Flutter，学习曲线很平缓。即使你是小白，也能快速上手。

## 4.1 事件处理（Event Handling）

在 Jaspr 中，给 HTML 元素绑定事件非常简单，主要有两种方式：

1. **便捷属性**（推荐新手使用）：如 `onClick`、`onInput`、`onChange` 等，直接传入一个函数。
2. **通用 `events` Map**（更灵活）：当需要处理自定义事件或多个事件时使用。

### 简单示例：点击按钮计数器

```dart
import 'package:jaspr/jaspr.dart';

@client  // ← 重要！后面会解释
class CounterButton extends StatefulComponent {
  const CounterButton({super.key});

  @override
  State<CounterButton> createState() => _CounterButtonState();
}

class _CounterButtonState extends State<CounterButton> {
  int count = 0;  // 组件内部状态

  @override
  Component build(BuildContext context) {
    return div([
      p([text('当前计数：$count')]),  // 显示状态
      button(
        // 方式一：使用便捷的 onClick 属性（最常用）
        onClick: () {
          setState(() => count++);  // 更新状态，触发 UI 重绘
          print('按钮被点击了！当前 count = $count');
        },
        styles: Styles.box(
          padding: EdgeInsets.all(12.px),
          backgroundColor: Colors.blue,
          color: Colors.white,
        ),
        [text('点击我 +1')],
      ),
    ]);
  }
}
```

**小白注意事项**：
- `setState(() { ... })` 是 Flutter 风格的经典写法，它会通知 Jaspr 重新调用 `build()` 方法，更新页面。
- `onClick` 接收一个 `() => void` 函数（无参数）。如果你需要事件对象，可以使用 `events` Map。
- 事件函数中可以直接访问 `this`（当前 State 对象）的变量和方法。

### 进阶示例：使用 `events` Map 处理 input 输入事件

```dart
input(
  [],
  // 方式二：使用 events Map（更通用）
  events: {
    'input': (event) {  // 事件名为字符串 'input'
      // event 是原生浏览器事件对象
      final inputElement = event.target as web.HTMLInputElement;  // 需要 import 'dart:html' as web; 或用 jaspr 的类型
      final newValue = inputElement.value;
      print('用户输入的内容：$newValue');
      // 这里可以结合 setState 更新状态
    },
  },
  attributes: {'placeholder': '在这里输入文字...'},
);
```

**提示**：大多数常见事件都有便捷属性（如 `onClick`、`onSubmit`、`onKeyDown` 等），优先使用它们，代码更清晰。

## 4.2 Riverpod 状态管理集成

Jaspr 官方通过 `jaspr_riverpod` 包深度支持 **Riverpod**，用法和 Flutter 的 `flutter_riverpod` **几乎完全一致**。这对 Flutter 开发者来说非常友好。

### 安装

在 `pubspec.yaml` 中添加：

```yaml
dependencies:
  jaspr: ^最新版本
  jaspr_riverpod: ^对应版本   # 通常与 jaspr 版本匹配
```

### 简单计数器示例（使用 NotifierProvider）

```dart
import 'package:jaspr/jaspr.dart';
import 'package:jaspr_riverpod/jaspr_riverpod.dart';

// 定义一个简单的 Todo 数据模型
class Todo {
  final String id;
  final String title;
  final bool completed;

  Todo({
    required this.id,
    required this.title,
    this.completed = false,
  });

  // 方便创建新的 Todo（切换完成状态或复制）
  Todo copyWith({String? id, String? title, bool? completed}) {
    return Todo(
      id: id ?? this.id,
      title: title ?? this.title,
      completed: completed ?? this.completed,
    );
  }
}

// 2. 定义 Notifier 类（这里封装了所有操作逻辑）
class TodosNotifier extends Notifier<List<Todo>> {
  // build() 方法返回初始状态（必须实现）
  @override
  List<Todo> build() {
    return []; // 初始时没有待办事项
  }

  // 添加新待办事项
  void addTodo(String title) {
    final newTodo = Todo(
      id: DateTime.now().millisecondsSinceEpoch.toString(), // 简单生成唯一 id
      title: title,
    );
    state = [...state, newTodo]; // 创建新列表，触发 UI 更新
  }

  // 切换完成状态
  void toggleTodo(String id) {
    state = [
      for (final todo in state)
        if (todo.id == id)
          todo.copyWith(completed: !todo.completed)
        else
          todo,
    ];
  }

  // 删除待办事项
  void removeTodo(String id) {
    state = state.where((todo) => todo.id != id).toList();
  }
}

// 3. 定义 NotifierProvider（全局可访问）—— 使用 constructor tear-off，更简洁！
  final todosProvider = NotifierProvider<TodosNotifier, List<Todo>>(
    TodosNotifier.new, 
  );

@client
class TodoList extends StatelessComponent {
  const TodoList({super.key});

  @override
  Component build(BuildContext context) {
    // 监听 todosProvider 的状态变化
    final todos = context.watch(todosProvider);

    // 获取 notifier 来调用方法（推荐方式）
    final notifier = context.read(todosProvider.notifier);

    // 用于输入新待办事项的本地状态（简单示例）
    String newTitle = '';

    return div([
      h2([text('我的待办事项 (${todos.length})')]),

      // 输入框 + 添加按钮
      div(styles: Styles.flex(direction: FlexDirection.row, gap: 8.px), [
        input(
          attributes: {'placeholder': '输入新的待办事项...'},
          onInput: (e) {
            // 这里为了简化直接用事件对象获取值（实际项目可结合 State）
            newTitle = (e.target as dynamic).value ?? '';
          },
        ),
        button(
          onClick: () {
            if (newTitle.trim().isNotEmpty) {
              notifier.addTodo(newTitle.trim());
              newTitle = ''; // 清空输入
            }
          },
          [text('添加')],
        ),
      ]),

      // 待办事项列表
      ul([
        for (final todo in todos)
          li(
            styles: Styles.flex(direction: FlexDirection.row, gap: 12.px, alignItems: AlignItems.center),
            [
              input(
                type: InputType.checkbox,
                checked: todo.completed,
                onChange: (_) => notifier.toggleTodo(todo.id),
              ),
              span(
                styles: todo.completed
                    ? Styles.text(decoration: TextDecoration.lineThrough)
                    : null,
                [text(todo.title)],
              ),
              button(
                onClick: () => notifier.removeTodo(todo.id),
                styles: Styles.box(color: Colors.red),
                [text('删除')],
              ),
            ],
          ),
      ]),

      if (todos.isEmpty) p([text('暂无待办事项，快添加一个吧！')]),
    ]);
  }
}
```

**为什么推荐 Riverpod？**
- 它比 `setState` 更适合复杂应用（全局状态、异步、依赖等）。
- `jaspr_riverpod` 支持**服务端到客户端的状态同步**（SSR 时服务器预加载的数据可以无缝传给浏览器）。
- 用法和 Flutter 几乎一样：`Provider`、`StateProvider`、`NotifierProvider`、`FutureProvider` 等全部支持。

**小白进阶提示**：
- 对于复杂逻辑，推荐使用 `NotifierProvider` 或 `AsyncNotifierProvider`。
- 如果需要在 SSR 时预加载数据，可以结合 `FutureProvider` + 同步功能，让客户端直接拿到服务器计算好的结果，避免二次请求。

## 4.3 @client 注解 —— 让组件在浏览器端“激活”

这是 Jaspr 在 SSR 模式下非常重要的概念。

### 什么是 @client？

- Jaspr 默认进行**服务端渲染**（SSR）：服务器先生成完整的 HTML，发给浏览器（SEO 友好、首屏快）。
- 但 HTML 本身是静态的，没有交互能力。
- **@client** 注解告诉 Jaspr：“这个组件需要在浏览器端进行 hydration（注水/激活）”，即：
  - 服务器仍会渲染它的初始 HTML。
  - 浏览器加载后，会为这个组件附加 JavaScript 事件监听和状态管理，使其变得可交互。

### 使用示例

```dart
import 'package:jaspr/jaspr.dart';

// 不加 @client 的组件：只在服务器渲染成静态 HTML，无交互
class StaticHeader extends StatelessComponent {
  const StaticHeader({super.key});

  @override
  Component build(BuildContext context) {
    return header([text('这是静态头部，不会响应点击')]);
  }
}

// 加了 @client 的组件：服务器渲染 + 浏览器激活交互
@client
class InteractiveCounter extends StatefulComponent {
  const InteractiveCounter({super.key});

  @override
  State<InteractiveCounter> createState() => _InteractiveCounterState();
}

class _InteractiveCounterState extends State<InteractiveCounter> {
  int count = 0;

  @override
  Component build(BuildContext context) {
    return div([
      text('交互计数：$count'),
      button(
        onClick: () => setState(() => count++),
        [text('点击增加')],
      ),
    ]);
  }
}
```

**小白常见疑问**：
- **什么时候需要加 @client？**  
  只要组件里有 `onClick`、`setState`、Riverpod 监听、动画等**浏览器端行为**，就必须加上。否则事件不会生效。
- **性能影响？**  
  只有标记为 `@client` 的组件才会编译成 JavaScript 并 hydrate。静态部分保持纯 HTML，性能更好。
- **整个页面都需要加吗？**  
  不需要！只给真正需要交互的部分加即可（比如头部导航、购物车、表单等）。这正是 Jaspr 比纯客户端框架（如 Flutter Web）更高效的地方。

### 完整小例子：把上面内容组合起来

```dart
// main.dart 或对应页面
class MyPage extends StatelessComponent {
  const MyPage({super.key});

  @override
  Component build(BuildContext context) {
    return div([
      StaticHeader(),           // 静态部分
      RiverpodCounter(),        // Riverpod 交互
      InteractiveCounter(),     // 普通 Stateful 交互
    ]);
  }
}
```

## 总结与建议（给小白）

1. **事件处理**：先用 `onClick` 等便捷属性，够用再用 `events` Map。
2. **状态管理**：简单场景用 `StatefulComponent + setState`；复杂场景强烈推荐 `jaspr_riverpod`。
3. **@client 注解**：记住“需要交互就加”，它是 Jaspr SSR 的核心魔法。
4. **调试技巧**：用 `jaspr` 的开发模式（`jaspr serve` 或对应命令）运行，能看到热重载和清晰的错误信息。
5. **进一步学习**：查看官方文档中的 [State Management](https://docs.jaspr.site/guides/state-management) 和 [Client-Side](https://docs.jaspr.site/dev/client) 部分，多在 Jaspr Playground 里实验代码。

掌握了这三点，你就能在 Jaspr 中轻松构建出既有优秀 SEO、又富有交互的现代网站了！




# 5. 路由系统 (Routing)

在 Jaspr 中，路由系统是实现**单页应用 (SPA)** 的核心。它让你可以通过**浏览器地址栏的 URL** 来切换不同的页面，而不需要刷新整个网页。

Jaspr 的路由系统非常接近 Flutter 的 `Navigator`，但它是基于**浏览器 URL 路径**来工作的。

## 5.1 主要概念

- **Router 组件**：整个应用的路由管理器，负责根据 URL 显示对应的页面（类似于 Flutter 的 `MaterialApp` + `Navigator`）。
- **Link 组件**：用于在页面之间进行**无刷新跳转**（代替传统的 `<a>` 标签，避免页面重载）。
- **Route**：定义 URL 路径与对应页面组件的映射关系。


## 5.2 基本用法（推荐写法）

在 `main.dart` 中使用 `Router` 组件包裹你的应用：

```dart
import 'package:jaspr/jaspr.dart';

void main() {
  runApp(App());
}

class App extends StatelessComponent {
  const App({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield Router(
      // routes: 定义所有路由规则
      routes: [
        // 首页路由
        Route(
          path: '/',                    // URL 路径
          builder: (context) => HomePage(),
        ),
        
        // 关于页面路由
        Route(
          path: '/about',               // 注意：路径必须以 / 开头
          builder: (context) => AboutPage(),
        ),
        
        // 动态路由（带参数）
        Route(
          path: '/user/:id',            // :id 是参数占位符
          builder: (context) => UserPage(),
        ),
      ],
      
      // 可选：当路由未匹配时显示的页面（404）
      notFound: NotFoundPage(),
    );
  }
}
```



## 5.3 创建页面组件

每个路由对应的页面都是普通的 Jaspr 组件：

```dart
// home.dart
class HomePage extends StatelessComponent {
  const HomePage({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div([
      h1([text('欢迎来到首页！')]),
      
      // 使用 Link 组件进行无刷新跳转
      Link(
        to: '/about',                    // 跳转目标路径
        child: button([text('去关于页面')]),
      ),
      
      br(),
      
      Link(
        to: '/user/123',
        child: button([text('查看用户 123')]),
      ),
    ]);
  }
}
```

```dart
// about.dart
class AboutPage extends StatelessComponent {
  const AboutPage({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div([
      h1([text('关于我们')]),
      
      p([text('这是一个使用 Jaspr 构建的网页应用。')]),
      
      // 返回首页
      Link(
        to: '/', 
        child: button([text('返回首页')]),
      ),
    ]);
  }
}
```



## 5.4 获取动态路由参数（重要！）

当使用 `/user/:id` 这种带参数的路由时，需要在页面中获取参数：

```dart
class UserPage extends StatelessComponent {
  const UserPage({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    // 通过 context 获取当前路由参数
    final routeParams = RouteParams.of(context);
    final userId = routeParams.get('id');        // 获取 :id 的值
    
    // 也可以用 getOrDefault 提供默认值
    // final userId = routeParams.getOrDefault('id', '未知');

    yield div([
      h1([text('用户详情页面')]),
      p([text('当前用户 ID: $userId')]),
      
      Link(
        to: '/',
        child: button([text('返回首页')]),
      ),
    ]);
  }
}
```



## 5.5 404 页面（Not Found）

```dart
class NotFoundPage extends StatelessComponent {
  const NotFoundPage({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div([
      h1([text('404 - 页面未找到')]),
      p([text('您访问的页面不存在。')]),
      
      Link(
        to: '/',
        child: button([text('返回首页')]),
      ),
    ]);
  }
}
```

**小白 Jaspr 路由使用小贴士**

1. **所有路径都必须以 `/` 开头**（除了根路径 `/` 本身）。
2. **推荐使用 `Link` 组件** 而不是 `<a>` 标签，这样页面不会刷新，体验更好。
3. 动态路由参数用 `:参数名` 表示，例如 `/post/:slug`、`/product/:id`。
4. 可以使用 `Router.of(context).push('/new-path')` 进行**编程式导航**（类似 Flutter 的 `Navigator.push`）。
5. 目前 Jaspr 的路由是**基于文件路径的简单路由**，功能已经足够大多数中小型项目使用。



# 6. 嵌套路由 (Nested Routes)

嵌套路由允许你创建一个**父路由 + 多个子路由**的结构。父路由通常负责**共享布局**（Shell/Layout），子路由则负责具体的内容页面。

## 6.1 基本嵌套路由写法

在 `App` 的 `Router` 中这样定义：

```dart
import 'package:jaspr/jaspr.dart';
import 'package:jaspr_router/jaspr_router.dart';   // 确保引入 jaspr_router

class App extends StatelessComponent {
  const App({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield Router(
      routes: [
        // 根路由（首页）
        Route(
          path: '/',
          builder: (context, state) => HomePage(),
        ),

        // 嵌套路由示例：仪表盘
        Route(
          path: '/dashboard',                    // 父路径
          builder: (context, state) => DashboardLayout(),  // 共享布局页面
          routes: [                              // 子路由列表
            Route(
              path: 'overview',                  // 注意：子路径不带前面的 /
              builder: (context, state) => DashboardOverview(),
            ),
            Route(
              path: 'settings',
              builder: (context, state) => DashboardSettings(),
            ),
            Route(
              path: 'users/:id',                 // 带参数的子路由
              builder: (context, state) => DashboardUserDetail(
                userId: state.pathParameters['id'] ?? '未知',
              ),
            ),
          ],
        ),
      ],
    );
  }
}
```

**重要说明**：
- 子路由的 `path` 是**相对路径**，不需要加 `/`。
- 实际访问的 URL 会自动拼接：`/dashboard/overview`、`/dashboard/settings`、`/dashboard/users/123`。


## 6.2 共享布局组件（DashboardLayout）

父路由的 `builder` 返回的组件必须包含 **`RouterOutlet()`**，它会负责渲染当前匹配的子路由内容。

```dart
// dashboard_layout.dart
class DashboardLayout extends StatelessComponent {
  const DashboardLayout({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div(classes: 'dashboard-container', [
      // 顶部导航（共享）
      nav([
        Link(to: '/dashboard/overview', child: text('概览')),
        Link(to: '/dashboard/settings', child: text('设置')),
        Link(to: '/dashboard/users/123', child: text('用户详情')),
      ]),

      // 侧边栏（共享）
      aside([
        h3([text('仪表盘菜单')]),
        // 可以放更多 Link
      ]),

      // 主要内容区域 —— 这里必须放 Outlet！
      main_([
        RouterOutlet(),        // ← 关键！子路由的内容会在这里渲染
      ]),

      // 底部栏（可选共享）
      footer([text('© 2026 我的 Jaspr 应用')]),
    ]);
  }
}
```

> **小白提示**：`RouterOutlet()` 就像一个“占位符”，Jaspr 会自动把当前子路由对应的组件渲染到这里。布局的其他部分（导航、侧边栏）在切换子页面时**不会重新渲染**，体验更好。



## 6.3 子页面示例（保持简单）

```dart
// dashboard_overview.dart
class DashboardOverview extends StatelessComponent {
  const DashboardOverview({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div([
      h1([text('仪表盘概览')]),
      p([text('欢迎来到概览页面，这里可以显示统计数据等。')]),
    ]);
  }
}

// dashboard_settings.dart
class DashboardSettings extends StatelessComponent {
  const DashboardSettings({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div([
      h1([text('设置中心')]),
      p([text('在这里修改你的偏好设置。')]),
    ]);
  }
}
```


## 6.4 编程式导航（Programming Navigation）

除了使用 `Link` 组件，你还可以通过代码主动跳转（类似 Flutter 的 `Navigator`）。

```dart
// 在任意组件中获取 Router 并导航
class SomePage extends StatelessComponent {
  const SomePage({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield button(
      events: {
        'onclick': (event) {
          final router = Router.of(context);

          // 方式1：推入新路由（类似 push，会保留历史）
          router.push('/dashboard/settings');

          // 方式2：替换当前路由（类似 replace，不会保留历史）
          // router.replace('/dashboard/overview');

          // 方式3：返回上一页
          // router.back();

          // 方式4：使用命名路由（后面会讲）或带参数
          // router.pushNamed('user-detail', pathParameters: {'id': '456'});
        }
      },
      child: text('进入设置页面'),
    );
  }
}
```

**常用方法总结**：
- `router.push('/path')` → 正常跳转（可返回）
- `router.replace('/path')` → 替换当前页面（不可返回）
- `router.back()` → 返回上一页
- `router.go('/path')` → 某些版本支持，直接跳转并重置部分历史（具体以 jaspr_router 文档为准）

**小白 Jaspr 嵌套路由使用建议**

1. **布局共享** 是嵌套路由的最大价值，尽量把重复的头部、侧边栏、底部放到父布局中。
2. `RouterOutlet()` **必须** 放在父布局里，否则子路由不会显示。
3. 子路由路径是**相对的**，不要加前面的 `/`。
4. 动态参数在子路由中使用 `state.pathParameters['参数名']` 获取。
5. 大型项目推荐结合 **ShellRoute**（如果 jaspr_router 提供）或自定义布局来实现更复杂的嵌套（例如带 Tab 的仪表盘）。




# 7. 编程式导航（Programming Navigation）进阶版

除了使用 `<Link>` 组件点击跳转，你还可以在代码中**主动控制路由**（类似 Flutter 的 `Navigator.push`）。

## 7.1 获取 Router 对象

```dart
final router = Router.of(context);
```

### 常用导航方法（带示例）

```dart
class NavigationExample extends StatelessComponent {
  const NavigationExample({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div([
      button(
        events: {
          'onclick': (event) {
            final router = Router.of(context);

            // 1. 正常跳转（压入新页面，可用浏览器后退返回）
            router.push('/dashboard/settings');

            // 2. 替换当前页面（不可后退，返回键会回到上一个历史页面）
            // router.replace('/dashboard/overview');

            // 3. 返回上一页（相当于浏览器后退）
            // router.back();

            // 4. 跳转到根路径并清除历史（谨慎使用）
            // router.go('/');

            // 5. 带查询参数跳转（URL 会变成 /search?q=flutter）
            router.push('/search', queryParameters: {'q': 'jaspr', 'page': '1'});
          }
        },
        child: text('进入设置页面'),
      ),
    ]);
  }
}
```

**小白贴士**：
- `push` 最常用（保留历史）
- `replace` 用于登录后跳转到首页（不想让用户能返回登录页）
- `back()` 常用于“取消”或“返回”按钮



## 7.2 命名路由（Named Routes）

当项目变大后，直接写路径字符串很容易出错。**命名路由**让你给每个路由起一个名字，后续跳转只用名字，路径改了也不用到处改代码。

#### 定义命名路由

```dart
class App extends StatelessComponent {
  const App({super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield Router(
      routes: [
        Route(
          name: 'home',           // 路由名称
          path: '/',
          builder: (context, state) => HomePage(),
        ),
        Route(
          name: 'dashboard',
          path: '/dashboard',
          builder: (context, state) => DashboardLayout(),
          routes: [
            Route(
              name: 'dashboard-overview',
              path: 'overview',
              builder: (context, state) => DashboardOverview(),
            ),
            Route(
              name: 'user-detail',
              path: 'users/:id',
              builder: (context, state) => UserDetailPage(
                userId: state.pathParameters['id'] ?? '',
              ),
            ),
          ],
        ),
      ],
    );
  }
}
```

### 使用命名路由跳转

```dart
// Link 组件使用 name
Link(
  toNamed: 'dashboard-overview',   // 推荐写法
  child: text('去概览页'),
),

// 编程式导航使用 pushNamed
button(
  events: {
    'onclick': (event) {
      Router.of(context).pushNamed(
        'user-detail',
        pathParameters: {'id': '123'},           // 替换 :id
        queryParameters: {'tab': 'profile'},     // ?tab=profile
      );
    }
  },
  child: text('查看用户 123'),
)
```

**优势**：路径改动时只需改定义处，跳转代码不用动。



## 7.3 ShellRoute —— 推荐的嵌套布局方式

Jaspr 官方更推荐使用 `ShellRoute` 来实现共享布局（比普通 `Route + RouterOutlet` 更清晰）。

#### ShellRoute 用法

```dart
Router(
  routes: [
    // ShellRoute 包裹需要共享布局的所有子路由
    ShellRoute(
      builder: (context, state, child) => DashboardShell(child: child),
      routes: [
        Route(
          path: '/dashboard',
          builder: (context, state) => DashboardOverview(), // 默认子页面
        ),
        Route(
          path: '/dashboard/settings',
          builder: (context, state) => DashboardSettings(),
        ),
        Route(
          path: '/dashboard/users/:id',
          builder: (context, state) => UserDetailPage(
            userId: state.pathParameters['id'] ?? '',
          ),
        ),
      ],
    ),
  ],
)
```

### Shell 布局组件（关键！）

```dart
class DashboardShell extends StatelessComponent {
  final Component child;   // 子路由渲染的内容

  const DashboardShell({required this.child, super.key});

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield div(classes: 'dashboard-shell', [
      // 顶部导航栏（切换子页面时保持不变）
      header([
        nav([
          Link(to: '/dashboard', child: text('概览')),
          Link(to: '/dashboard/settings', child: text('设置')),
          Link(to: '/dashboard/users/1', child: text('用户')),
        ]),
      ]),

      // 侧边栏
      aside([text('侧边菜单...')]),

      // 主要内容区 —— child 会在这里自动渲染
      main_([child]),

      // 底部
      footer([text('Jaspr ShellRoute 示例')]),
    ]);
  }
}
```

**小白记忆口诀**：`ShellRoute` = 外壳（布局） + `child`（内容）。布局不动，内容换。


## 7.4 懒加载路由（Lazy Loading）

当应用变大时，一次性加载所有页面代码会让首屏变慢。**懒加载**可以让用户访问某个页面时才加载它的代码。

### 使用 LazyRoute

```dart
Router(
  routes: [
    Route(
      path: '/',
      builder: (context, state) => HomePage(),
    ),
    // 懒加载仪表盘
    LazyRoute(
      path: '/dashboard',
      load: () async {
        // 这里可以做异步准备工作（模拟加载）
        await Future.delayed(const Duration(milliseconds: 300));
      },
      builder: (context, state) => DashboardShell(child: const DashboardOverview()),
    ),
  ],
)
```

**进阶**：也可以对 `ShellRoute` 使用懒加载（`ShellRoute.lazy`）。

**好处**：减小初始 bundle 大小，提升首屏加载速度。



## 7.5 重定向（Redirects）

有些场景需要用户自动跳转，比如：

- 未登录用户访问 `/dashboard` → 跳转到 `/login`
- 旧路径 `/old-page` → 永久重定向到新路径

### 在 Router 中添加 redirect

```dart
Router(
  // 全局重定向逻辑
  redirect: (context, state) {
    final isLoggedIn = false; // 这里换成你的登录状态判断

    if (!isLoggedIn && state.matchedLocation.startsWith('/dashboard')) {
      return '/login';                    // 重定向到登录页
    }

    // 可以根据路径做更多判断
    if (state.matchedLocation == '/old-home') {
      return '/';                         // 旧首页跳转到新首页
    }

    return null; // 不重定向，继续正常匹配路由
  },

  routes: [
    Route(path: '/login', builder: (context, state) => LoginPage()),
    // ... 其他路由
  ],
)
```

**小贴士**：`redirect` 返回 `null` 表示不重定向，返回字符串路径表示跳转。


## 小白 Jaspr 路由进阶总结（推荐做法）

1. **小项目**：直接用普通 `Route` + `Link` + `Router.of(context).push()` 就够。
2. **中大型项目**（推荐）：
   - 使用 **命名路由**（方便维护）
   - 使用 **ShellRoute**（共享布局）
   - 重要模块使用 **LazyRoute**（性能优化）
   - 登录/权限用 **redirect**
3. 始终优先用 `Link` 或 `toNamed`，少用裸 `<a>` 标签。
4. 动态参数统一通过 `state.pathParameters['xxx']` 获取。
5. 查询参数通过 `state.queryParameters['key']` 获取。