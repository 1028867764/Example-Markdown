## 前言

这是我去年写的👉👉  [Riverpod状态管理框架入门教程](https://blog.csdn.net/2403_88863963/article/details/148673041?fromshare=blogdetail&sharetype=blogdetail&sharerId=148673041&sharerefer=PC&sharesource=2403_88863963&sharefrom=from_link)（不推荐阅读）

众所周知，Flutter 自带的那一套原生状态管理方案是有局限性的。比如 “跨组件或跨页面共享数据” 实现起来会很困难。这时候，就不得不采用另外的状态管理方案，Riverpod 就是一个很好的选择（另外还有Bloc、GetX，但是我没学过）。如果说 Riverpod 的前身————Provider 将 UI 和状态、业务逻辑解耦的话，Riverpod 就是在这个基础上进一步拆分状态和业务逻辑，呈现出以 “ UI 、状态、业务逻辑” 三足鼎立且互不干扰的干净利索。

## Riverpod 简介

### 1. 什么是 Riverpod？

**Riverpod** 是 Flutter 的现代响应式状态管理库，由 [Remi Rousselet](https://github.com/rrousselGit) 开发，它是 Provider 的后续作品，但不依赖 `BuildContext`，拥有更高的灵活性、类型安全性与测试友好性。

主要特性包括：

* ✅ 完全脱离 `BuildContext`，不再依赖 Widget 树
* ✅ 更强的类型系统和编译期检查
* ✅ 易于测试，支持模块化设计
* ✅ 支持同步和异步状态管理

---

### 2. 与 Provider 的区别

| 对比项               | Provider     | Riverpod       |
| ----------------- | ------------ | -------------- |
| 是否依赖 BuildContext | ✅ 是          | ❌ 否            |
| 类型安全              | 一般           | ✅ 编译期强类型       |
| 支持热重载             | 部分情况下失效      | ✅ 稳定支持         |
| 测试友好性             | 需搭配上下文或 mock | ✅ 可单独测试业务逻辑    |
| 支持异步/组合           | 部分           | ✅ 全面内建支持       |
| 状态生命周期管理          | 较弱           | ✅ 可自动销毁、作用域更灵活 |

#### 关键差异解释：

* ✅ **无需 `BuildContext`**：你可以在服务层、ViewModel、异步回调中访问状态，无需通过 Widget 层传递。
* ✅ **模块化更清晰**：Provider 可以自由组合、传参（使用 `.family`）。
* ✅ **异步支持原生化**：直接使用 `FutureProvider`/`StreamProvider` 管理异步状态。

---

### 3. Riverpod 的优势与适用场景

#### ✅ 优势总结

* **彻底解耦 UI 与状态逻辑**：你可以在任意地方访问状态，独立测试逻辑。
* **支持组合、依赖注入**：Provider 可以依赖其他 Provider。
* **自动生命周期管理**：使用 `autoDispose` 后，状态可在不使用时自动释放。
* **完整异步支持**：内建 `FutureProvider` / `AsyncValue`。
* **良好测试支持**：通过 `ProviderContainer` 可在测试中模拟状态。

#### 🧭 典型使用场景

* 中大型 Flutter 项目
* 需要复杂状态交互或异步请求
* 关注架构清晰性和可维护性
* 团队开发、模块化协作
* 测试覆盖率要求高的项目


当然可以，以下是整理完善后的 Markdown 内容，包括各个 `Provider` 类型的用途说明以及 `ref` 的常见用法，并附上简单代码示例，方便你直接复制使用：

---

## 其它 `Provider` (略)

其它的？...我感觉用不着，真要用的时候再去问AI吧！

## `NotifierProvider` (重点知识)
- **NotifierProvider** 基本能覆盖 99% 的状态管理需求
- 关键点：
  - `build()` 方法返回初始状态。
  - 通过 `state = newValue` 来更新状态（必须重新赋值，不可变更新）。
  - 在 Notifier 内部可以用 `ref` 访问其他 Provider。



### 1. 为什么需要 NotifierProvider？

在 Flutter 开发中，状态管理是核心问题之一。简单的 `setState` 适合小组件，但当状态需要在多个页面间共享、涉及业务逻辑时，就需要更强大的方案。

**Riverpod** 是 Flutter 社区推荐的状态管理库，它比 Provider 更安全、更易测试，且支持编译时检查。**NotifierProvider** 是 Riverpod 中用于管理 **可变复杂状态** 的现代推荐方式（Riverpod 2.0+ 引入，取代了旧的 `StateNotifierProvider`）。

**NotifierProvider 的核心优势：**
- 状态（`state`）和业务逻辑（方法）封装在同一个 `Notifier` 类中，代码更清晰。
- 支持不可变状态（推荐使用 `copyWith`），减少 bug。
- 全局可用，一处定义，多处使用。
- 易于测试（可单独测试 Notifier 的方法）。
- 在 Riverpod 3.x 中仍是主流推荐（`StateNotifierProvider` 已标记为遗留，不建议新项目使用）。

“薛定谔的猫”项目就是一个绝佳示例：猫的状态（毛色 + 生死）需要在“看看猫”和“改变猫的状态”两个页面间共享。

### 2. 准备工作

#### 2.1 项目结构

```
lib/
├── main.dart
├── example_riverpod.dart     // CatState + CatNotifier + catProvider
├── to_see_page.dart
└── to_change_page.dart
```
#### 2.2 安装 Riverpod

在 `pubspec.yaml` 中添加依赖（最新版本请以 pub.dev 为准）：

```yaml
dependencies:
  flutter_riverpod: ^2.5.0  # 或更高版本
```

#### 2.3 包裹 ProviderScope

然后在 `main.dart` 中必须用 `ProviderScope` 包裹整个 App（你的代码已经正确做了）：

```dart
void main() {
  runApp(
    const ProviderScope(  // ← 必须包裹，否则无法使用 Riverpod
      child: MyApp(),
    ),
  );
}
```

`ProviderScope` 是 Riverpod 的“根容器”，所有 Provider 都在它下面生效。

### 3. 定义状态模型（CatState）

状态应该保持**不可变**（immutable），这样 Riverpod 能高效检测变化并重建 UI。

```dart
// example_riverpod.dart（或单独文件）
class CatState {
  final Color furColor;
  final bool isDead;

  CatState({
    this.furColor = Colors.orange,   // 默认橘猫
    this.isDead = false,             // 默认活着
  });

  // 关键：copyWith 方法实现不可变更新
  CatState copyWith({Color? furColor, bool? isDead}) {
    return CatState(
      furColor: furColor ?? this.furColor,
      isDead: isDead ?? this.isDead,
    );
  }
}
```

### 4. 创建 Notifier（业务逻辑核心）

`CatNotifier` 继承 `Notifier<CatState>`，这是 NotifierProvider 的核心。

```dart
class CatNotifier extends Notifier<CatState> {
  @override
  CatState build() {
    return CatState();  // 初始化默认状态
  }

  // 改变毛色
  void changeFurColor(Color newColor) {
    state = state.copyWith(furColor: newColor);  // 更新状态
  }

  // 救活猫
  void revive() {
    state = state.copyWith(isDead: false);
  }

  // 杀死猫
  void kill() {
    state = state.copyWith(isDead: true);
  }
}
```

**注意要点：**
- `build()` 方法只负责初始化状态，不要在构造函数中写逻辑。
- 使用 `state = ...` 更新状态（Riverpod 会自动通知监听者重建 UI）。
- 所有业务方法都写在这里，保持单一职责。

### 5. 创建 Provider（全局注册）

`final 你的Provider = NotifierProvider<你的Notifier, 你的State>(你的Notifier.new);`

```dart
final catProvider = NotifierProvider<CatNotifier, CatState>(CatNotifier.new);
```

这个 Provider 一旦创建，全局任何地方都可以通过 `ref.watch(catProvider)` 读取状态，或 `ref.read(catProvider.notifier)` 调用方法。

### 6. 在 UI 中使用（ConsumerStatefulWidget）

**监听状态（ToSeePage）：**

使用 `ref.watch(你的Provider);` 持续监听状态的变化（若有变化，相关组件立即局部刷新）

```dart
class ToSeePage extends ConsumerStatefulWidget { // ⚠️⚠️注意看！父类组件的名字
  const ToSeePage({super.key});

  @override
  ConsumerState<ToSeePage> createState() => _ToSeePageState();
}

class _ToSeePageState extends ConsumerState<ToSeePage> {
  @override
  Widget build(BuildContext context) {
    final cat = ref.watch(catProvider);  // 监听状态变化，自动重建

    return Scaffold(
      // ...
      body: Column(
        children: [
          Container(
            height: 100,
            width: 100,
            decoration: BoxDecoration(
              color: cat.furColor,
              borderRadius: BorderRadius.circular(12),
            ),
            child: const Center(child: Text('猫', style: TextStyle(color: Colors.white, fontSize: 20))),
          ),
          Text(cat.isDead ? '猫死了 😿' : '猫还活着 😺'),
        ],
      ),
    );
  }
}
```

**修改状态（ToChangePage）：**

使用 `ref.read(你的Provider.notifier).方法();` 调用业务逻辑中的方法

```dart
void _turnCatOrange() {
  ref.read(catProvider.notifier).changeFurColor(Colors.orange);
}

// 其他按钮类似...
```

**一次性读取状态：**

使用 `ref.read(你的Provider).属性;` 一次性读取状态，常用于单击、长摁等一次性操作中

```dart
  void _checkColor() {
    final cat = ref.read(
      catProvider,
    ); // ⚠️⚠️请注意这里不是watch，而是read。因为这是一次性操作，不需要也不适合持续监听
    if (cat.furColor == Colors.orange) {
      print('是橘猫');
    } else {
      print('不是橘猫');
    }
  }
```

### 7. 项目总代码
main.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'to_see_page.dart';
import 'to_change_page.dart';

void main() {
  runApp(
    const ProviderScope(
      // ← 这里必须包裹 ProviderScope
      child: MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '薛定谔的猫',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.white),
        useMaterial3: true,
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('薛定谔的猫'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              '欢迎来到薛定谔的猫实验！\n\n请选择要进入的页面：',
              textAlign: TextAlign.center,
              style: TextStyle(fontSize: 20),
            ),
            const SizedBox(height: 60),

            // 上面的按钮
            ElevatedButton(
              style: ElevatedButton.styleFrom(
                padding: const EdgeInsets.symmetric(
                  horizontal: 40,
                  vertical: 16,
                ),
                textStyle: const TextStyle(fontSize: 18),
              ),
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => const ToSeePage()),
                );
              },
              child: const Text('看看猫'),
            ),

            const SizedBox(height: 30),

            // 下面的按钮
            ElevatedButton(
              style: ElevatedButton.styleFrom(
                padding: const EdgeInsets.symmetric(
                  horizontal: 40,
                  vertical: 16,
                ),
                textStyle: const TextStyle(fontSize: 18),
              ),
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => const ToChangePage()),
                );
              },
              child: const Text('改变猫的状态'),
            ),
          ],
        ),
      ),
    );
  }
}
```
example_riverpod.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

// 猫的状态
class CatState {
  final Color furColor;
  final bool isDead;

  CatState({
    this.furColor = Colors.orange, // 默认橙色
    this.isDead = false, // 默认活着
  });

  // 不可变状态，推荐使用 copyWith 方法
  CatState copyWith({Color? furColor, bool? isDead}) {
    return CatState(
      furColor: furColor ?? this.furColor,
      isDead: isDead ?? this.isDead,
    );
  }
}

// 管理猫的状态(业务逻辑)
class CatNotifier extends Notifier<CatState> {
  @override
  CatState build() {
    return CatState(); // 重写build方法，初始化默认状态
  }

  // 方法1：改变猫的毛色
  void changeFurColor(Color newColor) {
    state = state.copyWith(furColor: newColor);
  }

  // 方法2：让猫活着
  void revive() {
    state = state.copyWith(isDead: false);
  }

  // 方法3：让猫死掉
  void kill() {
    state = state.copyWith(isDead: true);
  }
}

// 创建Provider，一次创建，全局使用
final catProvider = NotifierProvider<CatNotifier, CatState>(CatNotifier.new);
```
to_change.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'example_riverpod.dart';

class ToChangePage extends ConsumerStatefulWidget {
  const ToChangePage({super.key});

  @override
  ConsumerState<ToChangePage> createState() => _ToChangePageState();
}

class _ToChangePageState extends ConsumerState<ToChangePage> {
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    Future.delayed(const Duration(milliseconds: 500), () {
      setState(() {
        _isLoading = false;
      });
    });
  }

  void _turnCatOrange() {
    ref.read(catProvider.notifier).changeFurColor(Colors.orange);
  }

  void _turnCatBlack() {
    ref.read(catProvider.notifier).changeFurColor(Colors.black);
  }

  void _killCat() {
    ref.read(catProvider.notifier).kill();
  }

  void _reviveCat() {
    ref.read(catProvider.notifier).revive();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('改变猫的状态')),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                children: [
                  // 四个按钮区域
                  Expanded(
                    child: GridView.count(
                      crossAxisCount: 2, // 每行两个按钮
                      mainAxisSpacing: 16,
                      crossAxisSpacing: 16,
                      childAspectRatio: 1.6, // 按钮宽高比例，可自行调整
                      children: [
                        _buildActionButton(
                          '变橘猫',
                          doSomething: () => _turnCatOrange(),
                        ),
                        _buildActionButton(
                          '变黑猫',
                          doSomething: () => _turnCatBlack(),
                        ),
                        _buildActionButton(
                          '把它搞死',
                          doSomething: () => _killCat(),
                        ),
                        _buildActionButton(
                          '救活它',
                          doSomething: () => _reviveCat(),
                        ),
                      ],
                    ),
                  ),
                ],
              ),
            ),
    );
  }

  // 提取按钮构建方法，便于统一样式
  Widget _buildActionButton(String text, {required VoidCallback doSomething}) {
    return ElevatedButton(
      style: ElevatedButton.styleFrom(
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
        textStyle: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
        padding: const EdgeInsets.symmetric(vertical: 20),
        shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(16)),
        elevation: 6,
      ),
      onPressed: () {
        doSomething();
      },
      child: Text(text),
    );
  }
}
```
to_see.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'example_riverpod.dart';

class ToSeePage extends ConsumerStatefulWidget {
  const ToSeePage({super.key});

  @override
  ConsumerState<ToSeePage> createState() => _ToSeePageState();
}

class _ToSeePageState extends ConsumerState<ToSeePage> {
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    Future.delayed(const Duration(milliseconds: 500), () {
      setState(() {
        _isLoading = false;
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    final cat = ref.watch(catProvider);
    return Scaffold(
      appBar: AppBar(title: const Text('看看猫')),
      body: Center(
        child: _isLoading
            ? const CircularProgressIndicator()
            : Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Container(
                    height: 100,
                    width: 100,
                    decoration: BoxDecoration(
                      color: cat.furColor,
                      borderRadius: BorderRadius.circular(12),
                    ),
                    child: Center(
                      child: Text(
                        '猫',
                        textAlign: TextAlign.center,
                        style: TextStyle(
                          color: Colors.white,
                          fontSize: 20,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                  ),
                  const SizedBox(height: 30),
                  Text(
                    cat.isDead ? '猫死了' : '猫还活着',
                    style: TextStyle(
                      color: Colors.black,
                      fontSize: 16,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ],
              ),
      ),
    );
  }
}
```

### 8. 最佳实践与注意事项

1. **优先不可变状态**：始终用 `copyWith` 更新，不要直接修改对象内部属性。
2. **区分 read 和 watch**：
   - `ref.watch()`：监听变化，UI 自动更新。
   - `ref.read()`：只读一次或调用方法，不订阅。
3. **避免在 Notifier 构造函数中写逻辑**：全部放到 `build()` 中。
4. **性能优化**：复杂状态时使用 `ref.watch(catProvider.select((state) => state.furColor))` 只监听部分字段。
5. **测试**：可以单独测试 `CatNotifier`：
   ```dart
   test('changeFurColor updates state', () {
     final notifier = CatNotifier();
     notifier.changeFurColor(Colors.black);
     expect(notifier.state.furColor, Colors.black);
   });
   ```
6. **与 AsyncNotifierProvider 的区别**：
   - `NotifierProvider`：同步状态（如你的猫）。
   - `AsyncNotifierProvider`：异步初始化（如从网络加载数据），推荐用于 API 调用。
7. **Riverpod 版本注意**：Riverpod 2.x/3.x 中 `NotifierProvider` 是推荐方式，旧的 `StateNotifierProvider` 已不建议在新项目中使用。

### 9. 别高兴的太早

 >缺点：虽然现在好像已经入门Riverpod了，但是，新的问题接踵而来。那就是，如果 CatState 的其中一个属性变了也会导致监听另一个属性的 widget 重建，岂不是会造成无意义的性能损耗？

 #### 问题分析

```dart
final catProvider = NotifierProvider<CatNotifier, CatState>(CatNotifier.new);
```

当你调用 `changeFurColor()`、`revive()` 或 `kill()` 时，整个 `CatState` 都会被替换（因为 `state = ...`），导致所有 **监听 `catProvider`** 的 `Consumer` / `ref.watch(catProvider)` 的 Widget **全部重建**，即使它们只关心 `furColor` 或只关心 `isDead`。

比如：
- 一个只显示猫颜色的 Widget，也会因为 `isDead` 变化而重建。
- 一个只显示“猫是否活着”状态的 Widget，也会因为换毛色而重建。

这就是你说的 **“另一个属性变了也会导致这个 widget 重建”** —— **无意义的性能损耗**。


---

### 解决方案A（这样的情况不多时）：
**最佳推荐：使用 `select` 精确监听（最常用、最推荐）**

```dart
// 只监听毛色
final furColorProvider = catProvider.select((state) => state.furColor);

// 只监听是否死亡
final isDeadProvider = catProvider.select((state) => state.isDead);
```

使用方式：

```dart
Consumer(
  builder: (context, ref, child) {
    final color = ref.watch(furColorProvider);        // 只在颜色变时重建
    return Container(color: color);
  },
)
```

```dart
Consumer(
  builder: (context, ref, child) {
    final isDead = ref.watch(isDeadProvider);         // 只在死亡状态变时重建
    return Text(isDead ? "猫死了 😢" : "猫活着 🐱");
  },
)
```

**优点**：
- 性能最好
- 代码清晰
- Riverpod 官方强烈推荐

**缺点**：
- 状态拆分太多会污染UI

---

### 解决方案B（这样的情况很多时）：

请进一步咨询AI或者可以了解一下 [Oref](https://pub.dev/packages/oref) (群友安利的)

>Oref 是一款基于 alien_signals 构建的高性能 Flutter 状态管理工具，也是速度最快的 Flutter 信号和状态管理解决方案之一。

