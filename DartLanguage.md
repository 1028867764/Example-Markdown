
以下是对 Dart 官方文档核心内容的总结，涵盖基础语法、核心特性及进阶用法，适合快速掌握 Dart 语言的关键知识点：

---

# **1. 基础语法**
## **开始**
### **1.1.1 Hello World**
```dart
void main() {
  print('Hello, World!');
}
```
- `main()` 是Dart程序的唯一入口，无返回值时自动推断为 `void`。
- `print()` 用于输出内容到控制台。

### **1.1.2 注释**
在 Dart 中，注释的使用方法和大多数编程语言类似。Dart 支持单行注释、多行注释和文档注释，下面是具体的说明和使用示例：

#### 1.1.2.1 **单行注释（Single-line comments）**

单行注释用于对代码中的某一行或部分进行简单说明。你可以在代码中使用 `//` 来标记单行注释。

```dart
// 这是一个单行注释
int a = 10; // 这是对代码后面部分的注释
```

#### 1.1.2.2 **多行注释（Multi-line comments）**

多行注释可以用于注释掉多行代码或对复杂的代码块进行说明。使用 `/*` 和 `*/` 来标记多行注释。

```dart
/*
这是一个多行注释
可以注释掉多行代码
或者解释较长的内容
*/
int a = 10;
int b = 20;
```

#### 1.1.2.3 **文档注释（Documentation comments）**

文档注释是 Dart 中用于生成文档的注释，通常用于函数、类和库的说明。文档注释使用 `///`（单行）或 `/** */`（多行）来创建，并且通常会与 Dart 的文档生成工具（如 `dartdoc`）结合使用。

- ##### 单行文档注释（以 `///` 开头）

```dart
/// 计算两个整数的和
/// [a] 和 [b] 是要相加的两个整数
/// 返回它们的和
int add(int a, int b) {
  return a + b;
}
```

- ##### 多行文档注释（以 `/**` 和 `*/` 包围）

```dart
/**
 * 计算两个整数的和。
 * 
 * [a] 和 [b] 是要相加的两个整数。
 * 返回它们的和。
 */
int add(int a, int b) {
  return a + b;
}
```

#### 1.1.2.4 **注释的规范**

* **清晰明了**：注释应该简洁明了，不要解释显而易见的代码。只有在代码的意图或实现比较复杂时，才需要添加注释。
* **文档注释**：对于公共函数、类或库，使用文档注释（`///`）详细说明它们的功能和用法。这样有助于生成自动化文档，并让其他开发者容易理解代码的目的。
* **不要滥用注释**：尽量避免在代码中大量使用注释，而是通过命名清晰的变量和函数来提高代码的可读性。注释的目的是为了解释不直观的部分，而不是用来解释显而易见的内容。

#### 1.1.2.5 **生成文档**

使用 Dart 的 `dartdoc` 工具，你可以生成项目的文档。`dartdoc` 会提取文档注释并生成 HTML 格式的文档。

```bash
dart pub global activate dartdoc  # 激活 dartdoc
dart doc  # 生成文档
```

生成的文档通常包括你在代码中使用 `///` 和 `/** */` 注释的部分。

---

#### 1.1.2.6 总结

* **单行注释**：`//`，适用于简短说明。
* **多行注释**：`/* ... */`，适用于多行或块状注释。
* **文档注释**：`///` 或 `/** ... */`，用于自动生成文档，描述函数、类或库的用途。

注释是代码的重要组成部分，它帮助你和团队成员更好地理解和维护代码。

## **1.2 变量的声明与初始化**
### 1.2.1 `var`
在Dart中，使用`var`关键字来声明一个变量，Dart会自动推断出变量类型。例如：
```dart
var num = 123; // Dart 自动推断 num 为 int 类型
num = 789;     // ✅ 合法，同为 int 类型

var str = 'Hello'; // Dart 自动推断 str 为 String 类型
str = 666;         // ❌ 错误：str 已被推断为 String 类型，不能赋值为 int 类型
```
> 使用 `var` 声明变量后，**Dart 会根据第一次赋的值推断出类型**，该类型一旦确定，之后就**不能再赋予其他类型的值**。

### 1.2.2 显式声明
也可以在声明变量时明确指定其类型，这样能让代码更易于理解，并且在编译时检查到类型错误。示例如下：
```dart
String greeting = 'Hello Dart';
int age = 20;
```
### 1.2.3 `null` 和 `?`
在 Dart 中，`null` 是一个特殊的对象，表示“没有值”或“空值”。理解 Dart 中的 `null` 对于避免常见的运行时错误（比如 `NullPointerException`）至关重要。Dart 从 **2.12** 开始引入了 **空安全（null safety）**，使得代码更加健壮。

---

#### 1.2.3.1 基本概念

* `null` 是一个对象，属于 `Null` 类型。
* 所有对象的默认初始值是 `null`（在非空安全代码中）。
* 在空安全启用后，变量是否可以为 `null` 需要显式声明。

---

#### 1.2.3.2 空安全语法

##### ✅ 非空变量（默认不允许为 `null`）：

```dart
int a = 10;     // OK
a = null;       // ❌ 错误：a 不能为 null
```

##### ✅ 可空变量（加 `?`）：
如果变量可能含有空值，需要在类型后面加一个问号`?`。例如：
```dart
int? b = 5;     // OK
b = null;       // ✅ 合法

String? nullableString = null; // ✅ 合法
```

### 1.2.4 编译期 vs 运行期
| 概念    | 编译期（Compile-Time）                | 运行期（Run-Time）               |
| ----- | -------------------------------- | --------------------------- |
| 时机    | 程序**还没执行之前**，Dart 编译器在分析代码时，Flutter 将代码文件打包成相应平台的机器码时      | 程序已经开始执行时（即 `main()` 开始运行后） |

### 1.2.5 `final` 和 `const`
如果一个变量在初始化后值不再改变，可以使用 `final` 或 `const` 声明它。例如：
```dart
final pi = 3.14159;
const gravity = 9.8;
```
需要注意的是：
| 特性        | `final`              | `const`               |
| --------- | -------------------- | --------------------- |
| 是否只能赋值一次  | ✅ 是                           | ✅ 是                               |
| 是否可以延迟初始化 | ✅ 可以：声明时可以不赋值，允许在运行时首次使用前初始化。 | ❌ 不可以：必须在声明时立即初始化，且值必须在编译时已知。 |
| 是否为编译期常量  | ❌ 否，值在运行时确定               | ✅ 是，值在编译时就必须固定               |
```dart
final time = DateTime.now(); // ✅ 合法：运行时初始化
// const time = DateTime.now(); // ❌ 报错：const 不能用运行时值

const pi = 3.14; // ✅ 合法：编译时常量
```
关键点总结：

* `final`：可以稍后赋值一次，**值在运行时确定**。
* `const`：必须立即赋值，**值在编译时就必须固定**，用于定义**编译时常量**或创建**不可变对象**。

### 1.2.6 `late`
在 Dart 中，`late` 关键字用于 **推迟初始化变量**。它告诉编译器：“这个变量稍后一定会被初始化，但现在我还不打算立即给它赋值。”

---

#### 1.2.6.1 使用场景

`late` 常用于以下几种情况：

1. 类中的非空变量在构造函数中稍后初始化。
2. 需要懒加载（惰性加载）的变量。
3. 避免在声明时就必须赋值。
---

#### 1.2.6.2 基本语法

Dart 是空安全语言，默认要求非空变量在声明时初始化：

```dart
String name = 'Alice'; // 正常
String name; // ❌ 编译错误：未初始化
```

使用 `late` 可以推迟这个初始化过程：

```dart
late String name; // ✅ 合法

void main() {
  name = 'Alice'; // 只要在使用前赋值就行
  print(name);  // 输出: Alice
}
```

---

#### 1.2.6.3 注意事项

如果你 **声明了 `late` 变量但在使用前没有初始化**，将会抛出运行时错误：

```dart
late String name;

void main() {
  print(name); // ❌ RuntimeError: LateInitializationError
}
```

---

#### 1.2.6.4 推迟初始化 vs 惰性初始化

```dart
late final String aAA = bBB();

String bBB() {
  print('bBB...');
  return 'Result';
}
```

此变量 `aAA` 只有在第一次使用时才会调用 `bBB()`。所以 `late` 可以实现惰性加载。

---

#### 1.2.6.5 总结

| 特性               | 说明            |
| ---------------- | ------------- |
| `late`           | 推迟初始化（必须手动赋值） |
| `final`          | 只允许赋值一次     |
| `late final`     | 推迟并只允许赋值一次    |
| 不使用 `late` 且非空类型 | 声明时必须初始化      |
---

## **1.3 数据类型**
### **1.3.1 数值**
Dart中的数值类型分为`int`（整数）和`double`（浮点数），`num`是它们的父类。
- **`int`**  
  整数类型，支持二进制、八进制、十进制和十六进制表示：
```dart
  int a = 42;          // 十进制
  int b = 0b1010;      // 二进制 (10)
  int c = 0o12;        // 八进制 (10)
  int d = 0xA;         // 十六进制 (10)
```

- **`double`**  
  浮点数类型（64位双精度）：
```dart
  double e = 3.14159;
  double f = 2e3;      // 科学计数法 (2000.0)
```

### **1.3.2 `bool`**
- **`bool`**  
  Dart 使用 `bool` 表示布尔值，只有两个可能的值：`true` 和 `false`。  
```dart
  bool isRaining = true;
  bool isSunny = false;
```

#### 1.3.2.1 **布尔运算**  
  Dart 支持标准的逻辑运算：
```dart
  bool a = true;
  bool b = false;

  print(a && b);  // 逻辑与（AND）→ false
  print(a || b);  // 逻辑或（OR）→ true
  print(!a);      // 逻辑非（NOT）→ false
```

#### 1.3.2.2 **条件判断**  
  `bool` 类型常用于条件语句：
```dart
  if (isRaining) {
    print("记得带伞！");
  } else {
    print("天气晴朗！");
  }
```

#### 1.3.2.3 **类型安全**  
  Dart 是强类型语言，`if` 语句的条件必须是 `bool` 类型：
```dart
  int x = 10;
  // if (x) { ... }  // 错误！Dart 不允许隐式转换
  if (x > 0) {       // 正确，x > 0 返回 bool
    print("x 是正数");
  }
```

### **1.3.3 `String`**
在 Dart 中，`String` (字符串)是非常常用的数据类型。Dart 提供了许多强大的方法来操作字符串，涵盖了创建、访问、修改、搜索、格式化等多种常见操作。

#### 1.3.3.1 **字符串的创建**

```dart
String str1 = 'Hello, Dart!';  // 使用单引号
String str2 = "Hello, Dart!";  // 使用双引号
String str3 = '''
Dart is a great programming language.
It is easy to learn and powerful.
''';  // 使用三引号（多行字符串）
String str4 = """
Dart is a great programming language.
It is easy to learn and powerful.
""";  // 使用三引号（多行字符串）
```

#### 1.3.3.2 **字符串拼接**

```dart
String str1 = 'Hello';
String str2 = 'Dart';
String result = str1 + ', ' + str2 + '!';  // 使用 `+` 拼接
print(result);  // 输出: Hello, Dart!

// Dart 还支持使用插值操作（更推荐）
String result2 = '$str1, $str2!';
print(result2);  // 输出: Hello, Dart!
```

#### 1.3.3.3 **字符串长度**

```dart
String str = 'Hello, Dart!';
int length = str.length;
print(length);  // 输出: 13
```

#### 1.3.3.4 **访问字符串的单个字符**

```dart
String str = 'Hello, Dart!';
print(str[0]);  // 输出: H (访问第一个字符)
print(str[str.length - 1]);  // 输出: ! (访问最后一个字符)
```

#### 1.3.3.5 **查找子字符串**

```dart
String str = 'Hello, Dart!';
print(str.contains('Dart'));  // 输出: true（检查是否包含子字符串）
print(str.indexOf('Dart'));  // 输出: 7（返回子字符串的起始位置，-1表示找不到）
print(str.startsWith('Hello'));  // 输出: true（检查是否以某个子字符串开头）
print(str.endsWith('!'));  // 输出: true（检查是否以某个子字符串结尾）
```

#### 1.3.3.6 **提取子字符串**

```dart
String str = 'Hello, Dart!';
String substr = str.substring(7);  // 提取从索引7开始到结尾的子字符串
print(substr);  // 输出: Dart!

String substr2 = str.substring(0, 5);  // 提取从索引0到索引5的子字符串（不包括索引5）
print(substr2);  // 输出: Hello
```

#### 1.3.3.7 **替换字符串中的某些部分**

```dart
String str = 'Hello, Dart!';
String replaced = str.replaceAll('Dart', 'Flutter');
print(replaced);  // 输出: Hello, Flutter!
```

#### 1.3.3.8 **拆合字符串 `.split(',')` vs `.join(',')`**

```dart
//拆
String str = 'Hello, Dart, Flutter!';
List<String> parts = str.split(',');  // 按逗号分割字符串
print(parts);  // 输出: [Hello,  Dart,  Flutter!]

//合
List<String> list = ['a', 'b'];
String joined = list.join(','); // 转为字符串 "a,b"
```

#### 1.3.3.9 **去除空白字符**

```dart
String str = '   Hello, Dart!   ';
String trimmed = str.trim();  // 去除前后空白
print(trimmed);  // 输出: Hello, Dart!
```

#### 1.3.3.10 **字符串转换大小写**

```dart
String str = 'Hello, Dart!';
String upper = str.toUpperCase();  // 转为大写
String lower = str.toLowerCase();  // 转为小写
print(upper);  // 输出: HELLO, DART!
print(lower);  // 输出: hello, dart!
```

#### 1.3.3.11 **检查字符串是否为空**

```dart
String str = 'Hello, Dart!';
bool isEmpty = str.isEmpty;  // 检查是否为空字符串
print(isEmpty);  // 输出: false

String str2 = '';
bool isEmpty2 = str2.isEmpty;  // 检查是否为空字符串
print(isEmpty2);  // 输出: true

String? str = 'Hello';
if (str?.isNotEmpty == true) {
   print('字符串不为空');
 } else {
   print('字符串为空');
}
```
同理，还可以推广到集合，例如`List.isEmpty`、`List.isNotEmpty`、`Map.isEmpty`、`Map.isNotEmpty`等等


#### 1.3.3.12 **使用正则表达式进行字符串匹配**

```dart
String str = 'abc123def';
RegExp exp = RegExp(r'\d+');  // 匹配数字
Iterable<Match> matches = exp.allMatches(str);
for (var match in matches) {
  print(match.group(0));  // 输出: 123
}
```

#### 1.3.3.13 **字符串插值**

Dart 中的字符串插值非常直观和简单，允许直接在字符串中嵌入变量和表达式。你可以使用 `${}` 来插入表达式。

```dart
String name = 'Alice';
int age = 25;
String introduction = 'My name is $name and I am $age years old.';
print(introduction);  // 输出: My name is Alice and I am 25 years old.

String moreInfo = 'In 5 years, I will be ${age + 5} years old.';  // 使用表达式
print(moreInfo);  // 输出: In 5 years, I will be 30 years old.
```

#### 1.3.3.14 **常用的字符串方法**

| 方法              | 描述           |
| --------------- | ------------ |
| `toUpperCase()` | 转换为大写字母      |
| `toLowerCase()` | 转换为小写字母      |
| `trim()`        | 去除字符串两端的空白字符 |
| `isEmpty`       | 判断字符串是否为空    |
| `isNotEmpty`    | 判断字符串是否不为空    |
| `replaceAll()`  | 替换所有匹配的子字符串  |
| `split()`       | 按分隔符拆分字符串    |
| `contains()`    | 检查是否包含子字符串   |
| `substring()`   | 提取子字符串       |
| `indexOf()`     | 查找子字符串的索引位置  |
| `startsWith()`  | 判断是否以某个字符串开头 |
| `endsWith()`    | 判断是否以某个字符串结尾 |

---

#### 1.3.3.15 **示例：综合字符串操作**

```dart
void main() {
  String str = '  Dart is awesome!  ';

  // 去除两端空格并转换为大写
  String modified = str.trim().toUpperCase();
  print(modified);  // 输出: DART IS AWESOME!

  // 替换字符串中的某些字符
  String replacedStr = modified.replaceAll('DART', 'FLUTTER');
  print(replacedStr);  // 输出: FLUTTER IS AWESOME!

  // 分割字符串
  List<String> words = str.split(' ');
  print(words);  // 输出: [Dart, is, awesome!]

  // 使用插值
  String name = 'Alice';
  String greeting = 'Hello, $name! Welcome to Dart.';
  print(greeting);  // 输出: Hello, Alice! Welcome to Dart.
}
```
---

### **1.3.4 `List`**
`List` （列表）是有序可变的集合，支持泛型：  

在 Dart 中，`List` 是一个非常常用的数据结构，用于存储有序的元素集合。Dart 提供了丰富的内置方法来操作 `List`，包括添加、删除、查找、排序等。下面是一些常见的 `List` 方法和操作的示例：

#### 1.3.4.1 **创建 List**

```dart
List<int> numbers = [1, 2, 3, 4, 5];  // 使用字面量创建int 类型的 List
List<String> names = ['Alice', 'Bob', 'Charlie'];  // 使用字面量创建 String 类型的 List


var dynamicList = []; // 创建未指定类型的动态空List（可存储任意类型，不推荐）
List<String> nothing = []; // 使用字面量创建 String 类型的空 List


// 使用构造函数创建空 List
List<int> emptyList = List<int>.empty();
List<int> filledList = List<int>.filled(5, 0);  // 创建一个长度为 5，且每个元素初始化为 0 的 List
```

#### 1.3.4.2 **访问 List 元素**

```dart
void main() {
  List<String> fruits = [
    'apple',
    'banana',
    'cherry',
    'date',
    'fig',
    'grape',
    'honeydew'
  ];

  // 访问单个元素
  print(fruits[0]);  // 输出: apple
  print(fruits[1]);  // 输出: banana
  print(fruits[2]);  // 输出: cherry
  print(fruits[3]);  // 输出: date
  print(fruits[4]);  // 输出: fig
  print(fruits[5]);  // 输出: grape
  print(fruits[6]);  // 输出: honeydew

  // 打印列表长度
  print(fruits.length);  // 输出: 7

  // 访问多个元素：用 sublist(start, end)
  // '顾头不顾腚'，包括'start'不包括'end'
  print(fruits.sublist(2, 5));  // 输出: [cherry, date, fig]

  // 从某个位置一直到最后一个
  print(fruits.sublist(3));  // 输出: [date, fig, grape, honeydew]

}
```

#### 1.3.4.3 **添加元素**

* `add()`：添加一个元素到 List 的末尾。
* `addAll()`：将另一个 List 的所有元素添加到当前 List。
* `insert()`：在指定位置插入一个元素。
* `insertAll()`：在指定位置插入多个元素。

```dart
List<int> numbers = [1, 2, 3];

// 添加单个元素到末尾
numbers.add(4);
print(numbers);  // 输出: [1, 2, 3, 4]

// 添加多个元素
numbers.addAll([5, 6]);
print(numbers);  // 输出: [1, 2, 3, 4, 5, 6]

// 在指定位置插入一个元素
numbers.insert(2, 10);  // 在索引 2 位置插入 10
print(numbers);  // 输出: [1, 2, 10, 3, 4, 5, 6]

// 在指定位置插入多个元素
numbers.insertAll(4, [7, 8]);
print(numbers);  // 输出: [1, 2, 10, 3, 7, 8, 4, 5, 6]
```

#### 1.3.4.4 **删除元素**

* `remove()`：删除第一个匹配的元素。
* `removeAt()`：删除指定位置的元素。
* `removeLast()`：删除 List 中的最后一个元素。
* `removeWhere()`：删除符合条件的所有元素。
* `clear()`：清空 List 中的所有元素。

```dart
List<int> numbers = [1, 2, 3, 4, 5];

// 删除指定的元素
numbers.remove(3);  // 删除第一个匹配的 3
print(numbers);  // 输出: [1, 2, 4, 5]

// 删除指定位置的元素
numbers.removeAt(2);  // 删除索引为 2 的元素（元素 4）
print(numbers);  // 输出: [1, 2, 5]

// 删除最后一个元素
numbers.removeLast();  // 删除最后一个元素 5
print(numbers);  // 输出: [1, 2]

// 删除符合条件的元素
numbers.removeWhere((element) => element % 2 == 0);  // 删除所有偶数
print(numbers);  // 输出: [1]
```

#### 1.3.4.5 **查找元素**

* `indexOf()`：查找某个元素首次出现的索引，如果未找到则返回 -1。
* `lastIndexOf()`：查找某个元素最后一次出现的索引。
* `contains()`：判断 List 中是否包含某个元素。

```dart
List<String> names = ['Alice', 'Bob', 'Charlie', 'Alice', 'Bob', 'Charlie'];

// 查找元素的索引
print(names.indexOf('Bob'));  // 输出: 1

// 未找到则返回 -1
print(names.indexOf('Banana'));  // 输出: -1

// 查找元素的最后索引
print(names.lastIndexOf('Alice'));  // 输出: 3

// 判断元素是否存在
print(names.contains('Charlie'));  // 输出: true
```

#### 1.3.4.6 **排序和反转**

* `sort()`：对 List 进行排序。
* `reversed`：反转 List 的顺序。

```dart
List<int> numbers = [5, 3, 8, 1, 2];

// 排序
numbers.sort();
print(numbers);  // 输出: [1, 2, 3, 5, 8]

// 反转
var reversed = numbers.reversed;
print(reversed);  // 输出: (8, 5, 3, 2, 1)
```

#### 1.3.4.7 **转换和映射**

* `map()`：通过给 List 中的每个元素应用一个函数来转换元素。
* `where()`：过滤 List 中符合条件的元素。
* `toSet()`：将 List 转换为 Set，去除重复的元素。

```dart
List<int> numbers = [1, 2, 3, 4, 5];

// 使用 map() 转换元素
var doubled = numbers.map((e) => e * 2).toList();
print(doubled);  // 输出: [2, 4, 6, 8, 10]

// 使用 where() 过滤元素
var evenNumbers = numbers.where((e) => e % 2 == 0).toList();
print(evenNumbers);  // 输出: [2, 4]

// 转换为 Set（去重）
List<int> numbersWithDuplicates = [1, 2, 2, 3, 3, 4];
var uniqueNumbers = numbersWithDuplicates.toSet().toList();
print(uniqueNumbers);  // 输出: [1, 2, 3, 4]
```
* 你也许已经注意到了，上面所提供的3个方法后面都有 `.toList()` ，这是为什么呢？后面的 `.toList()` 可以省略吗？
* **为什么需要 `.toList()？`**
```markdown
# 在 Dart 中，集合操作通常返回的是 Iterable 类型而非 List 类型，这是因为：

# 1. **惰性计算**：map() 和 where() 返回的是 Iterable，它们采用惰性计算（lazy evaluation），只有在真正需要时才会执行计算，这能提高性能。

# 2. **类型差异**：
   - map() → 返回 Iterable<T>
   - where() → 返回 Iterable<T> 
   - toSet() → 返回 Set<T> 
   而我们需要的是 List<T>

   注意：Set 是无序且不重复的集合，会合并同类项，也就是“去重”
    

# 3. **操作需求**：List 提供了更多实用方法（如索引访问、添加元素等）和更好的可读性
```
* **何时可以省略 `.toList()`？**
```dart
List<int> numbers = [1, 2, 3, 4, 5];

// 示例1 使用 forEach 遍历结果，无需转换为 List
numbers.map((e) => e + 7).forEach(print); // 依次输出: 8, 9, 10, 11, 12 

// 示例2 使用 for-in 循环遍历 map() 的结果，无需转换为 List
for (var number in numbers.map((e) => e * 2)) {
  print(number);  // 依次输出: 2, 4, 6, 8, 10 
} // 而不是 [2, 4, 6, 8, 10]

// 示例3 使用 for-in 循环遍历 where() 的结果，无需转换为 List
for (var evenNumber in numbers.where((e) => e % 2 == 0)) {
  print(evenNumber);  // 依次输出: 2, 4
} // 而不是 [2, 4]

// 示例4 链式调用，无需转换为 List
var filtered = numbers
    .map((e) => e*2)
    .where((e) => e > 5); 

for (var filt in filtered) {
  print(filt); 
} // 依次输出: 6, 8, 10, 而不是 [6, 8, 10]
```

#### 1.3.4.8 **其他常用方法**

* `forEach()`：遍历 List 中的每个元素。
* `join()`：将 List 中的元素连接为一个字符串。
* `split()`：将字符串按照指定的分隔符切割成一个字符串列表。
* `first`：获取 List 中的第一个元素。
* `last`：获取 List 中的最后一个元素。
* `isEmpty`：判断 List 是否为空。

```dart
List<String> names = ['Alice', 'Bob', 'Charlie'];
String sentence = "apple,banana,orange";

// 遍历每个元素
names.forEach((name) => print(name));
// 输出: 
// Alice
// Bob
// Charlie

// 在上面代码中，forEach 传入一个参数并在{}代码块中打印该参数
// 像这样形式的代码可以简化成：
names.forEach(print);
// 输出: 
// Alice
// Bob
// Charlie

// 将 List 中的元素连接为字符串
String result = names.join(', ');
print(result);  // 输出: Alice, Bob, Charlie

// 将字符串切割成List
List<String> fruits = sentence.split(',');
print(fruits);  // 输出: [apple, banana, orange]

// 获取第一个和最后一个元素
print(names.first);  // 输出: Alice
print(names.last);   // 输出: Charlie

// 判断 List 是否为空
print(names.isEmpty);  // 输出: false
```

### **1.3.5 `Map`**
`Map` 是 `key-value` (键值对)集合，键唯一：  

在 Dart 中，`Map` 是一种非常重要的数据结构，用于存储键值对。`Map` 提供了丰富的内置方法来操作和访问数据。下面是一些常见的 `Map` 方法和操作的示例。

#### 1.3.5.1 **创建 Map**

```dart
// 使用字面量创建 Map
Map<String, int> scores = {
  'Alice': 90,
  'Bob': 85,
  'Charlie': 88,
};

// 创建未指定类型的动态空Map（可存储任意类型，不推荐）
var dynamicMap = {}; 

// 使用字面量创建 <String, int> 类型的空 Map
Map<String, int> nothing = {}; 

// 使用构造函数创建空 Map
Map<String, int> emptyMap = Map<String, int>();

// 使用 Map.from() 来创建一个新的 Map，复制另一个 Map 的内容
Map<String, int> copyMap = Map.from(scores);
```

#### 1.3.5.2 **访问 Map 元素**

```dart
Map<String, int> scores = {
  'Alice': 90,
  'Bob': 85,
  'Charlie': 88,
};

// 通过键访问值
print(scores['Alice']);  // 输出: 90
print(scores['Bob']);  // 输出: 85
print(scores['Charlie']);  // 输出: 88

// 获取 Map 的长度
print(scores.length);  // 输出: 3

// 检查键是否存在
print(scores.containsKey('Bob'));  // 输出: true
print(scores.containsKey('David'));  // 输出: false

// 检查值是否存在
print(scores.containsValue(88));  // 输出: true
```

#### 1.3.5.3 **添加元素**

* `addAll()`：将另一个 `Map` 的键值对添加到当前 `Map`。
* `putIfAbsent()`：只有在指定键不存在时才添加键值对。
* `[]=`：通过键添加或更新值。

```dart
Map<String, double> prices = {
  'apple': 7.5,
  'banana': 3.0,
};

// 添加一个新键值对
prices['orange'] = 3.9;
print(prices);  // 输出: {apple: 7.5, banana: 3.0, orange: 3.9}

// 使用 addAll() 添加多个键值对
prices.addAll({'candy': 6.0, 'biscuit': 5.7});
print(prices);  // 输出: {apple: 7.5, banana: 3.0, orange: 3.9, candy: 6.0, biscuit: 5.7}

// 使用 putIfAbsent() 只有键不存在时才添加
prices.putIfAbsent('hamburger', () => 9.9);
print(prices);  // 输出: {apple: 7.5, banana: 3.0, orange: 3.9, candy: 6.0, biscuit: 5.7, hamburger: 9.9}
```

#### 1.3.5.4 **删除元素**

* `remove()`：根据键删除一个键值对。
* `removeWhere()`：根据条件删除符合条件的所有键值对。
* `clear()`：删除 `Map` 中的所有键值对。

```dart
Map<String, double> prices = {
  'apple': 7.5,
  'banana': 3.0,
  'orange': 3.9,
};

// 使用 remove() 删除指定键值对
prices.remove('banana');
print(prices);  // 输出: {apple: 7.5, orange: 3.9}

// 使用 removeWhere() 删除所有价格低于 5 的商品
prices.removeWhere((key, value) => value < 5.0);
print(prices);  // 输出: {apple: 7.5}

// 使用 clear() 清空整个 Map
prices.clear();
print(prices);  // 输出: {}

```

#### 1.3.5.5 **查找元素**

* `keys`：返回 `Map` 中所有的键。
* `values`：返回 `Map` 中所有的值。
* `forEach()`：遍历 `Map` 中的每个键值对。

```dart
Map<String, double> prices = {
  'apple': 7.5,
  'banana': 3.0,
  'orange': 3.9,
};

// 获取所有的键
print(prices.keys);  // 输出: (apple, banana, orange)

// 获取所有的值
print(prices.values);  // 输出: (7.5, 3.0, 3.9)

// 遍历 Map
prices.forEach((key, value) {
  print('$key 的价格是 $value 元');
});
// 输出:
// apple 的价格是 7.5 元
// banana 的价格是 3.0 元
// orange 的价格是 3.9 元
```

#### 1.3.5.6 **修改元素**

* `update()`：更新指定键的值。
* `updateAll()`：更新所有键值对，根据给定的函数更新值。

```dart
Map<String, double> prices = {
  'apple': 7.5,
  'banana': 3.0,
  'orange': 3.9,
};

// 更新指定键的值
prices.update('apple', (value) => value + 1.0);
print(prices);  // 输出: {apple: 8.5, banana: 3.0, orange: 3.9}

// 如果指定键不存在，则可以提供一个默认值
prices.update('grape', (value) => value + 1.0, ifAbsent: () => 4.5);
print(prices);  // 输出: {apple: 8.5, banana: 3.0, orange: 3.9, grape: 4.5}

// 使用 updateAll() 更新所有价格，加 0.5
prices.updateAll((key, value) => value + 0.5);
print(prices);  // 输出: {apple: 9.0, banana: 3.5, orange: 4.4, grape: 5.0}
```

#### 1.3.5.7 **其他常用方法**

* `isEmpty`：判断 `Map` 是否为空。
* `isNotEmpty`：判断 `Map` 是否不为空。
* `addEntries()`：添加多个键值对。
* `putIfAbsent()`：当指定的键不存在时，添加一个键值对。

```dart
Map<String, double> prices = {
  'apple': 7.5,
  'banana': 3.0,
  'orange': 3.9,
};

// 判断 Map 是否为空
print(prices.isEmpty);  // 输出: false

// 判断 Map 是否不为空
print(prices.isNotEmpty);  // 输出: true

// 使用 addEntries() 添加多个键值对
prices.addEntries([
  MapEntry('cookie', 6.0),
  MapEntry('cake', 8.8),
]);
print(prices);  // 输出: {apple: 7.5, banana: 3.0, orange: 3.9, cookie: 6.0, cake: 8.8}
```

#### 1.3.5.8 **Map 复制与转换**

* `Map.from()`：从已有的 `Map` 创建一个新的 `Map`。
* `Map.of()`：从已有的键值对集合创建 `Map`。

```dart
Map<String, double> prices = {
  'apple': 7.5,
  'banana': 3.0,
  'orange': 3.9,
};

// 使用 Map.from() 创建新的 Map
Map<String, double> copiedPrices = Map.from(prices);
print(copiedPrices);  // 输出: {apple: 7.5, banana: 3.0, orange: 3.9}

// 使用 Map.of() 创建新的 Map
Map<String, double> newPrices = Map.of({'milk': 6.6, 'tea': 5.5});
print(newPrices);  // 输出: {milk: 6.6, tea: 5.5}
```

#### 1.3.5.9 总结

Dart 的 `Map` 提供了丰富的操作方法来处理键值对。常见的操作包括：

* **添加元素**：使用 `[]=`、`addAll()` 和 `putIfAbsent()`。
* **删除元素**：使用 `remove()`、`removeWhere()` 和 `clear()`。
* **查找元素**：使用 `containsKey()`、`containsValue()`、`keys` 和 `values`。
* **修改元素**：使用 `update()` 和 `updateAll()`。
* **遍历 Map**：使用 `forEach()`。
* **复制与转换**：使用 `Map.from()` 和 `Map.of()`。


### **1.3.6 `Set`**
`Set` 是无序且不重复的集合：
```dart
Set<String> uniqueItems = {'a', 'b', 'c'};
uniqueItems.add('a'); // 无效（重复值），自带去重效果
print(uniqueItems);   // {a, b, c}
```

---

### **1.3.7 特殊数据类型**
#### **1.3.7.1 `dynamic`**
`dynamic` 类型：动态类型，可以表示任何类型的值，在编译时绕过类型检查（慎用）：
```dart
dynamic value = 42;
value = 'Hello';      // 合法但可能引发运行时错误
```

#### **1.3.7.2 `Object?`**
`Object?` 类型：是所有对象的父类，支持可空性（需启用空安全）：
```dart
Object? obj = null;   // 允许 null
if (obj is String) {
  print(obj.toUpperCase()); // 安全类型转换
}
```

---

### **1.3.8 类型安全与空安全**
- **类型推断**：Dart 会自动推断变量类型（如 `var x = 10` 推断为 `int`）。
- **非空断言**：在变量后加 `!` 告诉 Dart 该值不为 `null`（需确保安全）：
```dart
  String? nullableStr;
  print(nullableStr!.length); // 若 nullableStr 为 null 会抛出异常
```
- **空合并运算符**：`??` 提供默认值：
```dart
  String? name;
  print(name ?? 'Anonymous'); // 输出 'Anonymous'
```

---

### **1.3.9 类型转换**
- **数值与字符串**：
```dart
  int zhengShu = int.parse('123');    // 字符串转 int
  double fuDianShu = double.parse('3.14'); // 字符串转 double
  String str1 = 100.toString();     // int 转字符串
  String str2 = 9.9.toString();  // double 转字符串
  print(3.141592653.toStringAsFixed(1)); // 保留1位小数，输出3.1
  print(3.141592653.toStringAsFixed(3)); // 保留3位小数，输出3.142
  print(3.141592653.toStringAsFixed(4)); // 保留4位小数，输出3.1416
```
- **集合转换**：
```dart
  List<String> list = ['a', 'b'];
  String joined = list.join(','); // 转为字符串 "a,b"
  List<String> split = 'a,b'.split(','); // 字符串转List
```

---
### **1.3.10 类型判断**
在 Dart 中，数据类型通常由编译器推断，或者你可以显式地声明类型。以下是如何确定数据类型的一些方法：

#### 1.3.10.1 **类型推断**

Dart 是一种强类型语言，通常会根据赋值推断变量的类型。如果你没有显式声明类型，Dart 会根据初始值自动推断类型。

```dart
void main() {
var x = 10;      // Dart 推断 x 的类型是 int
var y = 3.14;    // Dart 推断 y 的类型是 double
var isMoney = true；// Dart 推断 isMoney 的类型是 bool
var name = "Alice"; // Dart 推断 name 的类型是 String
var words = ['你', '干', '嘛']; // Dart 推断 words 的类型是 List<String>
var prices = {'apple': 6.9, 'banana': 3.0}; // Dart 推断 prices 的类型是 Map<String, double>
}
```
在这个例子中，`var` 用来声明变量，Dart 根据赋值推断类型。
#### 1.3.10.2 **同名赋值的常见类型错误 ❌**
```dart
void main() {
  var x = 10;
  x = true; // ❌ 错误：不能将类型为 'bool' 的值赋给类型为 'int' 的变量。

  var y = 3.14;
  y = '三点一四'; // ❌ 错误：不能将类型为 'String' 的值赋给类型为 'double' 的变量。

  var isMoney = true；
 isMoney = 3.14; // ❌ 错误：不能将类型为 'double' 的值赋给类型为 'bool' 的变量。

  var name = "Alice";
  name = 6; // ❌ 错误：不能将类型为 'int' 的值赋给类型为 'String' 的变量。

  var name = "Alice";
  name = [true, false, true]; // ❌ 错误：不能将类型为 'List<bool>' 的值赋给类型为 'String' 的变量。

  var words = ['你', '干', '嘛'];
  words = [90, 93, 72]; // ❌ 错误：不能将类型为 'List<int>' 的值赋给类型为 'List<String>' 的变量。

  var words = ['你', '干', '嘛'];
  words = {'word1': '你'}; // ❌ 错误：不能将类型为 'Map<String, String>' 的值赋给类型为 'List<String>' 的变量。

  var prices = {'apple': 6.9, 'banana': 3.0};
  prices = {'apple': true, 'banana': false}; // ❌ 错误：不能将类型为 'Map<String, bool>' 的值赋给类型为 'Map<String, double>' 的变量。

  var prices = {'apple': 6.9, 'banana': 3.0};
  prices = [6.9, 3.0]; // ❌ 错误：不能将类型为 'List<double>' 的值赋给类型为 'Map<String, double>' 的变量。
}
```
#### 1.3.10.3 **显式类型声明（推荐）✅**

你也可以显式地声明变量的类型：

```dart
void main() {
int x = 10;
double y = 3.14;
bool isMoney = true；
String name = "Alice";
List<String> words = ['你', '干', '嘛'];
Map<String, double> prices = {'apple': 6.9, 'banana': 3.0}; 
}
```

#### 1.3.10.4 **使用 `runtimeType` 属性**

Dart 中每个对象都有一个 `runtimeType` 属性，它表示对象在运行时的类型。你可以通过 `runtimeType` 获取对象的类型名称（以字符串形式）。

```dart
void main() {
  var x = 10;
  print(x.runtimeType);  // 输出: int

  var y = 3.14;
  print(y.runtimeType);  // 输出: double

  var isMoney = true;
  print(isMoney.runtimeType);  // 输出: bool

  var name = "Alice";
  print(name.runtimeType);  // 输出: String

  var words = ['你', '干', '嘛'];
  print(words.runtimeType);  // 输出: List<String>

  var prices = {'apple': 6.9, 'banana': 3.0};
  print(prices.runtimeType);  // 输出: Map<String, double>
}
```

#### 1.3.10.5 **`is` 判断符**

如果你想检查某个对象是否是某个特定类型，你可以使用 `is` 判断符，其构成的语句返回一个 `bool` 值。

```dart
void main() {
  var x = 10;
  var prices = {'apple': 6.9, 'banana': 3.0};

  if (x is int) {
    print("x 是 int 类型");
  } else {
    print("x 不是 int 类型");
  }
  // 输出: x 是 int 类型

  if (prices is Map<String, bool>) {
    print("prices 是 Map<String, bool> 类型");
  } else {
    print("prices 不是 Map<String, bool> 类型");
  }
  // 输出: prices 不是 Map<String, bool> 类型
} 
```

#### 1.3.10.6 **`is!` 判断符**

`is!` 用来测试一个对象是否不是某个类型，其构成的语句返回一个 `bool` 值。

```dart
void main() {
  var x = 9.9;

  if (x is! String) {
    print("x 不是 String 类型");
  }
 // 输出: x 不是 String 类型
}

```

#### 1.3.10.7 **`as` 运算符**

你还可以使用 `as` 来进行类型转换，将对象转换为指定类型。注意，转换时必须确保对象实际是该类型，否则会抛出异常。

```dart
void main() {
  var x = "123";
  var y = x as String; // 强制转换
  print(y);  // 输出: 123
}
```

#### 1.3.10.8 总结

* **类型推断**：通过赋值自动推断类型。
* **显式声明**：你可以手动声明变量的类型。
* **`runtimeType`**：获取对象的运行时类型。
* **`is` 运算符**：检查对象是否为某个类型。
* **`is!` 运算符**：检查对象是否不为某个类型。
* **`as` 运算符**：强制转换对象类型。

### **1.3.11 注意事项**
- **不可变性**：`String`、`List` 等基础类型默认不可变，修改需重新赋值。
- **性能优化**：频繁修改 `List` 时，可预分配容量：`List<int> list = List.filled(100, 0);`。

通过掌握这些数据类型和特性，可以高效编写安全且易维护的 Dart 代码！

---

## **1.4 运算符（Operators）**
---

### 1.4.1 算术运算符（Arithmetic Operators）

| 运算符  | 描述            | 示例（`a = 10, b = 3`） | 结果         |
| ---- | ------------- | ------------------- | ---------- |
| `+`  | 加法            | `a + b`             | `13`       |
| `-`  | 减法            | `a - b`             | `7`        |
| `*`  | 乘法            | `a * b`             | `30`       |
| `/`  | 除法（返回 double） | `a / b`             | `3.333...` |
| `~/` | 整除（结果取整数部分）            | `a ~/ b`            | `3`        |
| `%`  | 取余（模）         | `a % b`             | `1`        |

---
⚠️**注意**⚠️**余数始终为非负值**

即使除数是负的，Dart 仍然保证余数始终为非负值。

```dart
print(-10 % 3); // 输出 2
print(10 % -3); // 输出 1
print(-10 % -3); // 输出 2
```

解释：
- `-10 % 3` 相当于 “-10 除以 3 的余数”，但 Dart 返回正的余数 `2`（因为 `-10 = -4×3 + 2`）
- `10 % -3` 相当于 “10 除以 -3 的余数”，但 Dart 返回正的余数 `1`（因为 `10 = -3×-3 + 1`）
- `-10 % -3` 相当于 “-10 除以 -3 的余数”，但 Dart 返回正的余数 `2`（因为 `-10 = -4×3 + 2`）

### 1.4.2 关系运算符（Relational / Comparison Operators）

| 运算符  | 描述   | 示例（`a = 10, b = 3`） | 结果      |
| ---- | ---- | ------------------- | ------- |
| `==` | 等于   | `a == b`            | `false` |
| `!=` | 不等于  | `a != b`            | `true`  |
| `>`  | 大于   | `a > b`             | `true`  |
| `<`  | 小于   | `a < b`             | `false` |
| `>=` | 大于等于 | `a >= b`            | `true`  |
| `<=` | 小于等于 | `a <= b`            | `false` |

---

### 1.4.3 逻辑运算符（Logical Operators）

| 运算符  | 描述       | 示例（`a = true, b = false`） | 结果    |
|--------|------------|------------------------------|---------|
| `&&`   | 与（AND）   | `a && b`                     | `false` |
| `\|\|`   | 或（OR）    | `a \|\| b`                     | `true`  |
| `!`    | 非（NOT）   | `!a`                         | `false` |

---

### 1.4.4 赋值运算符（Assignment Operators）
#### 1.4.4.1 常用赋值

| 运算符   | 描述    | 示例（`a = 10`） | 结果    |
| ----- | ----- | ------------ | ----- |
| `=`   | 赋值    | `a = 10`     | `10`  |
| `+=`  | 加后赋值  | `a += 2`     | `12`  |
| `-=`  | 减后赋值  | `a -= 2`     | `8`   |
| `*=`  | 乘后赋值  | `a *= 2`     | `20`  |
| `/=`  | 除后赋值  | `a /= 2`     | `5.0` |
| `~/=` | 整除后赋值 | `a ~/= 3`    | `3`   |
| `%=`  | 取余后赋值 | `a %= 3`     | `1`   |

---
-示例代码
```dart
int x = 10;
x += 2; // 等价于 x = x + 2
print(x); // 输出: 12

int y = 6;
y *= 7; // 等价于 y = y * 7
print(y); // 输出: 42
```

#### 1.4.4.2 自增/自减
##### 后置自增/自减（先使用，后运算）
- 语法： `i++`  或  `i--` 
- 执行逻辑：先返回变量的当前值，再将变量值加1或减1。
```dart
int a = 5;
int b = a++; // 等价于：b = a; a = a + 1;
// 执行后：b = 5，a = 6

int c = 10;
int d = c--; // 等价于：d = c; c = c - 1;
// 执行后：d = 10，c = 9

```
##### 前置自增/自减（先运算，后使用）
- 语法： `++i`  或  `--i` 
- 执行逻辑：先将变量值加1或减1，再返回新值。
```dart
int a = 5;
int b = ++a; // 等价于：a = a + 1; b = a;
// 执行后：b = 6，a = 6

int c = 10;
int d = --c; // 等价于：c = c - 1; d = c;
// 执行后：d = 9，c = 9
```

### 1.4.5 条件运算符（Conditional Operators）

#### 1.4.5.1 三元运算符
- 基本语法
```dart
condition ? AAA : BBB;
// 如果 condition 为 true，返回 AAA
// 如果 condition 为 false，返回 BBB
```
- 示例代码
```dart
int size = 20;
String length = size >= 18 ? 'Big' : 'Small';
print(length); // 输出: Big
```

#### 1.4.5.2 空值合并运算符（`??`）
- 基本语法
```dart
a ?? b
// 如果 a 不为 null，则返回 a 的值；
//如果 a 为 null，则返回 b 的值。
```
- 示例代码
```dart
String? name;
var finalName = name ?? '默认名'; //如果 name 是空值，则返回 '默认名' ，等价于 var finalName = '默认名' ;
```

#### 1.4.5.3 空值赋值运算符（`??=`）

```dart
String? name;
name ??= '默认名'; //如果 name 是空值，则将 '默认名' 赋值给 name ，等价于 String? name = '默认名' ; 
```

---

### 1.4.6 类型运算符（Type Operators）

| 运算符   | 描述       | 示例               | 结果      |
| ----- | -------- | ---------------- | ------- |
| `is`  | 是否某类型    | `a is String`    | `true`  |
| `is!` | 不是某类型    | `a is! int`      | `true`  |
| `as`  | 类型转换（强制） | `obj as MyClass` | 转换为指定类型 |

---

### 1.4.7 其他运算符

| 运算符  | 描述             | 示例                        |
| ---- | -------------- | ------------------------- |
| `..` | 级联操作符（链式调用）    | `a..method1()..method2()` |
| `?`  | 可空类型标记         | `String? name;`           |
| `!`  | 非空断言（确保非 null） | `name!`                   |

---
### 1.4.8 级联操作符
Dart 中的级联操作符（`..`）允许你在同一个对象上连续调用多个方法或设置多个属性，而不需要重复对象名。
#### 基本语法
```dart
 /*
 原为：
  object..method1()..method2()..property = value;

  经过VScode的'Format Document'之后，变成如下'列表式'排布
*/

object
  ..method1()
  ..method2()
  ..property = value;
```
#### 等效于：
```dart
object.method1();
object.method2();
object.property = value;
```
#### 示例代码
```dart
class Person {
  String? name;
  int? age;

  void setName(String name) => this.name = name;
  void setAge(int age) => this.age = age;
}

void main() {
  var p = Person()
    ..setName("小李")
    ..setAge(28); 
/* 
    相当于 
    var p = Person(); 
    p.setName("小李"); 
    p.setAge(28); 
*/    
}
```
#### 注意事项

* 只能用于**返回对象自身的函数**（即返回 `this` 或无返回值的函数）；
* 不能用于 `null` 对象（除非使用 **空级联操作符 `?..`**，Dart 2.6+ 支持）。

```dart
MyClass? obj;
obj
  ?..doSomething()
  ?..value = 42;
```

### **1.4.9 可空类型标记 `?`，非空断言 `!`**

Dart 从 2.12 版本开始支持**健全的空安全（Sound Null Safety）**，通过 `?` 标记可空类型，明确区分**非空类型**和**可为 null 的类型**。

---

#### **1. 基本语法**
```dart
类型? 变量名;  // 声明可空变量
```
- **非空类型**（默认）：`String name`（必须初始化且不能为 `null`）
- **可空类型**：`String? name`（允许赋值为 `null`）

---

#### **2. 示例代码**
```dart
class User {
  String name;      // 非空类型（必须初始化）
  int? age;         // 可空类型（允许为 null）

  User(this.name);  // name 必须通过构造函数初始化
}

void main() {
  // 可空变量示例
  String? nullableString = null;  // ✅ 允许
  String nonNullableString = 'abc';  // ✅ 必须非空
  // String nonNullableString = null;     // ❌ 编译错误

  // 类中的可空属性
  final user = User('Alice')..age = null;  // ✅ 可空属性赋值为 null

  // 安全访问可空变量
  print(nullableString?.length);  // 输出：null（不会抛出异常）
}
```

---

#### **3. 关键操作**
| 操作 | 语法 | 说明 |
|------|------|------|
| **安全调用** | `object?.method()` | 若 `object` 为 `null` 则跳过方法调用，返回 `null` |
| **空值合并** | `object ?? defaultValue` | 若 `object` 为 `null` 则返回默认值 |
| **强制解包** | `object!` | 断言 `object` 非空（若为 `null` 抛出异常） |


```dart
void main() {
  String? maybeName;

  // 安全调用
  print(maybeName?.toUpperCase());  // 输出：null

  // 空值合并
  print(maybeName ?? '未知');       // 输出：未知

  // 强制解包（谨慎使用！）
  // print(maybeName!);             // 运行时异常（Null check error）
}
```

---

#### **4. 级联操作符与可空类型结合**
```dart
class Config {
  String? title;
  void log() => print('Title: $title');
}

void main() {
  // 安全级联（避免 NPE）
  Config()
    ?..title = '设置页'  // 若 Config() 为 null 则跳过后续操作
    ..log();           // 输出：Title: 设置页
}
```

---

#### **5. 为什么需要可空类型？**
1. **编译时检查**：防止 `NullPointerException` 运行时错误。
2. **代码清晰性**：明确标识哪些变量可能为 `null`。
3. **与 Flutter 结合**：Widget 的某些属性天然可为空（如 `Text.style`）。

---
# **2. 控制流**

## **2.1 条件语句**
### **2.1.1 `if-else`**
- **基本语法**：
```dart
void main() {
  if (条件1) {
    // 当 条件1 为 true 时执行
    执行语句1;
  } else if (条件2) {
    // 当 条件1 为 false，但 条件2 为 true 时执行
    执行语句2;
  } else {
    // 当所有条件均为 false 时执行
    执行语句3;
  }
  // 'else if () {}' 和 'else {}' 代码块不是必需的
}
```
- **示例代码**：
```dart
void main() {
  int score = 85;
  if (score >= 90) {
    print('优秀');
  } else if (score >= 60) {
    print('及格');
  } else {
    print('不及格');
  }
  // 输出 及格
}
```
- **特点**：
  - 条件表达式必须是布尔值（Dart 无隐式类型转换，`if (score)` 会报错）。
  - 支持嵌套 `if-else`。

### **2.1.2 `switch-case`**
- **基本语法**：
```dart
void main() {
  switch (变量或表达式) {
    // 从上到下依次匹配 case ，如果匹配某个 case，则执行其代码块，否则继续向下匹配 case 
    case 值1:
      执行语句1;
      break; // 如果省略 break，将直接穿透到下一个 case
    case 值2:
      执行语句2;
      break;
    // 多条件（'值3'和'值4'）合并写法，多个 case 共享同一段代码
    case 值3:    // 如果匹配 值3
    case 值4:    // 或匹配 值4
      执行语句3;  // 执行同一段代码
      break;
    default:
      执行语句5;  // 如果没有匹配的 case，则执行 default 块（如果存在），default 块不是必需的
  }
}
```
- **示例代码**：
```dart
void main() {
  String day = 'Friday';
  switch (day) {
    case 'Monday':
      print('星期一');
      break;
    case 'Tuesday':
      print('星期二');
      break;
    case 'Wednesday':
      print('星期三');
      break;
    case 'Thursday':
      print('星期四');
      break;
    case 'Friday':
      print('星期五');
      break;   
  /* 当 day 的值为 'Saturday' 或 'Sunday' 时
     这是 switch 语句的多条件合并写法     */
    case 'Saturday':
    case 'Sunday': // 多条件合并
      print('周末');
      break;
    default:
      print('其他');
  }
  // 输出 星期五
}
```
- **关键点**：
  - **必须用 `break`**：否则会继续执行下一个 `case`（无穿透行为）。
  - **`default`** 表示当所有 `case` 都不匹配时执行的代码，类似于 `if-else` 中的 `else`，不是必须的，但建议总是包含以处理意外情况
  - 支持任意类型（如 `int`、`String`、对象等）。
  - Dart 3.0+ 支持更复杂的模式匹配（需启用实验性功能）。

---

### **2.1.3 `continue` vs `break`**

| 语句 | 作用 |
|------|------|
| `continue` | 跳过当前循环的剩余部分，进入下一次迭代 |
| `break` | 直接终止整个循环 |

**示例对比**
```dart
void main() {
 for (int i = 0; i < 5; i++) {
   if (i == 2) {
     continue; // 跳过 i=2，继续循环
   }
   if (i == 4) {
     break; // 终止整个循环
   }
   print(i); // 输出: 0, 1, 3
 }
}
```
**说明**：
- `continue` 跳过 `i=2`，但循环继续执行。
- `break` 直接终止循环，不再执行后续迭代。

---

## **2.2 `for` 循环**
### **2.2.1 传统 `for`**
- 结构示意
```dart
 void main() {
  for (初始条件; 循环条件; 迭代器) {
   // 循环体
  }
}
/*
 初始条件：在总循环开始时执行仅一次
 循环条件：每次循环前检查，若为true则继续执行循环体
 迭代器：每次执行循环体后执行

 当循环条件为true时，执行循环体
 当循环条件为false时，退出for循环
*/

```
- 示例代码
```dart
  void main() {
   for (int i = 0; i < 5; i++) {
    print(i); // 输出 0, 1, 2, 3, 4
   }
  }
 ```

### **2.2.2 `for-in` 遍历集合**
`for-in` 遍历完所有元素后，循环会自动结束。
```dart
  void main() {
  // for-in 遍历列表
  List<String> fruits = ['Apple', 'Banana', 'Orange'];

  for (var fruit in fruits) {
    print(fruit); // 依次输出: Apple, Banana, Orange
  }

// 定义一个 Map，表示学生分数
Map<String, int> scores = {'Alice': 90, 'Bob': 85, 'Charlie': 95};

// 使用 for-in 搭配 .entries 遍历每一个键值对
// entry 只是变量名，也可以换成其他名字（如 e、pair）
// entry.key 表示键（学生姓名）
// entry.value 表示值（分数）

for (var entry in scores.entries) {
  print('${entry.key}: ${entry.value}');
  // 输出：
  // Alice: 90
  // Bob: 85
  // Charlie: 95
}

}
```

### **2.2.3 `forEach` 遍历集合**
`forEach` 遍历完所有元素后，循环会自动结束。
```dart
void main() {
  // forEach 遍历列表
  List<String> fruits = ['Apple', 'Banana', 'Orange'];

  fruits.forEach((fruit) {
    print(fruit); // 依次输出: Apple, Banana, Orange
  });

  // forEach 打印的简化写法（不适用于 Map ）
  fruits.forEach(print); // 依次输出: Apple, Banana, Orange

  // forEach 遍历键值对
    Map<String, int> scores = {'Alice': 90, 'Bob': 85, 'Charlie': 95};

  scores.forEach((key, value) {
    print('$key: $value');
    // 输出:
    // Alice: 90
    // Bob: 85
    // Charlie: 95
  });
}
```

### **2.2.4 `for-in` vs `forEach`**
- **优先用 `forEach`**：  
  当只需要简单遍历且无需中断时，`forEach` 更简洁：
```dart
  List<int> numbers = [1, 2, 3];
  numbers.forEach(print); // 直接传函数
```

- **用 `for-in` 处理复杂逻辑**：  
  需要中断循环或处理键值对时：
```dart
  for (var entry in scores.entries) {
    if (entry.value < 90) continue; // 跳过低分
    print('${entry.key} passed!');
  }
```

### **2.2.5 三维 List 遍历（蔬菜分类）**
```dart
void main() {
  // 定义一个三维 List，表示不同类别的蔬菜，每个类别下还有子类别
  List<List<List<String>>> vegetables = [
    // 叶菜类
    [
      ["小白菜", "大白菜"], // 小白菜和大白菜是叶菜类的子类别
      ["生菜", "油麦菜"]
    ],
    // 根茎类
    [
      ["胡萝卜", "白萝卜"],
      ["土豆", "红薯"] // 红薯也是根茎类蔬菜
    ],
    // 果菜类
    [
      ["西红柿", "黄瓜"],
      ["茄子", "辣椒"] // 辣椒属于果菜类
    ]
  ];

  print("=== 蔬菜分类遍历 ===");

  int count = 0; 

  // 外层循环遍历每个大类别
  for (var bigCategory in vegetables) {
    // 中层循环遍历每个大类别下的子类别
    for (var subCategory in bigCategory) {
      // 内层循环遍历当前子类别的每种蔬菜
      for (var veg in subCategory) {
        print(veg); // 输出蔬菜名称
        count++;
      }
    }
  }

print("总共有 $count 种蔬菜");

  // 输出结果：
  // === 蔬菜分类遍历 ===
  // 小白菜
  // 大白菜
  // 生菜
  // 油麦菜
  // 胡萝卜
  // 白萝卜
  // 土豆
  // 红薯
  // 西红柿
  // 黄瓜
  // 茄子
  // 辣椒
  // 总共有 12 种蔬菜
}
```
   - 结构：`List<List<List<String>>>` 表示分类数据
   - 应用：适合处理表格型数据（如超市货架分类）
### **2.2.6 三维 Map 遍历（蔬菜分类）**
```dart
void main() {
  // 定义一个三维 Map，表示不同类别的蔬菜，每个类别下还有子类别
  Map<String, Map<String, List<String>>> vegetables = {
    // 叶菜类
    "叶菜类": {
      "小白菜类": ["小白菜", "大白菜"],
      "生菜类": ["生菜", "油麦菜"],
    },
    // 根茎类
    "根茎类": {
      "萝卜类": ["胡萝卜", "白萝卜"],
      "薯类": ["土豆", "红薯"],
    },
    // 果菜类
    "果菜类": {
      "茄果类": ["西红柿", "黄瓜"],
      "椒类": ["胡椒", "辣椒"],
    },
  };

  print("=== 蔬菜分类遍历（三维Map - for-in）===");

  int count = 0;

  // 外层遍历大类别（for-in）
  for (var bigCategory in vegetables.entries) {
    print("\n【${bigCategory.key}】");

    // 中层遍历子类别（for-in）
    for (var subCategory in bigCategory.value.entries) {
      print("${subCategory.key}：");

      // 内层遍历蔬菜（for-in）
      for (var veg in subCategory.value) {
        print("  - $veg"); // 输出蔬菜名称
        count++;
      }
    }
  }

  print("\n总共有 $count 种蔬菜");
}
/* 输出结果：
=== 蔬菜分类遍历（三维Map - for-in）===

【叶菜类】
小白菜类：
  - 小白菜
  - 大白菜
生菜类：
  - 生菜
  - 油麦菜

【根茎类】
萝卜类：
  - 胡萝卜
  - 白萝卜
薯类：
  - 土豆
  - 红薯

【果菜类】
茄果类：
  - 西红柿
  - 黄瓜
椒类：
  - 胡椒
  - 辣椒

总共有 12 种蔬菜
*/
```
   - 结构：`Map<String, Map<String, List<String>>>` 表示分类数据
   - 应用：适合处理具有明确命名层级和语义的分类数据（如电商平台的商品分类）
### **2.2.7 排列组合（水果与饮料搭配）**
```dart
void main() {
  // 定义水果列表
  List<String> fruits = ["苹果", "香蕉", "橙子"];
  // 定义饮料列表
  List<String> drinks = ["牛奶", "果汁", "茶"];

  print("\n=== 水果与饮料搭配推荐 ===");
  // 外层循环遍历每种水果
  for (var fruit in fruits) {
    // 内层循环遍历每种饮料
    for (var drink in drinks) {
      print("$fruit + $drink"); // 输出搭配组合
    }
  }

  // 输出结果：
  // === 水果与饮料搭配推荐 ===
  // 苹果 + 牛奶
  // 苹果 + 果汁
  // 苹果 + 茶
  // 香蕉 + 牛奶
  // 香蕉 + 果汁
  // 香蕉 + 茶
  // 橙子 + 牛奶
  // 橙子 + 果汁
  // 橙子 + 茶
}
```
   - 结构：`for-in` 嵌套 `for-in`
   - 应用：生成所有可能的组合（如菜单搭配）
### **2.2.8 树形结构的先序遍历**
#### **2.2.8.1 语法示意**
Dart 中实现树形结构的先序遍历（pre-order traversal），通常通过递归函数来完成。以下是 **语法示意**：
```dart
class TreeNode {
  String name;
  List<TreeNode> children;

  TreeNode({required this.name, this.children = const []}); // 可选参数的默认值必须是编译期常量
}

// 统计终端节点数量
int countLeaves(TreeNode node) {
  if (node.children.isEmpty) {
    // 终止条件
    print('${node.name}: 触底啦!返回一个1');
    return 1;
  }
  print('下来到${node.name}啦!count = 0');
  int count = 0;
  for (var child in node.children) {
    print('这里是节点${child.name}:即将往下试探');
    count += countLeaves(child); // 递归调用
  }
  print('上来到${node.name}了:此时的count是${count}');
  return count;
}

void main() {
  /*

 构建一个树形结构：
         A
      /  |   \
     B   C    D
    /   / \     \
   F   E   G     H
      /       / | \
     I       J  K  L

*/
  var root = TreeNode(
    name: 'A',
    children: [
      TreeNode(name: 'B', children: [TreeNode(name: 'F')]),
      TreeNode(
        name: 'C',
        children: [
          TreeNode(name: 'E', children: [TreeNode(name: 'I')]),
          TreeNode(name: 'G'),
        ],
      ),
      TreeNode(
        name: 'D',
        children: [
          TreeNode(
            name: 'H',
            children: [
              TreeNode(name: 'J'),
              TreeNode(name: 'K'),
              TreeNode(name: 'L'),
            ],
          ),
        ],
      ),
    ],
  );

  print('终端节点数量：${countLeaves(root)}');
}

/*输出结果：
下来到A啦!count = 0
这里是节点B:即将往下试探
下来到B啦!count = 0
这里是节点F:即将往下试探
F: 触底啦!返回一个1
上来到B了:此时的count是1
这里是节点C:即将往下试探
下来到C啦!count = 0
这里是节点E:即将往下试探
下来到E啦!count = 0
这里是节点I:即将往下试探
I: 触底啦!返回一个1
上来到E了:此时的count是1
这里是节点G:即将往下试探
G: 触底啦!返回一个1
上来到C了:此时的count是2
这里是节点D:即将往下试探
下来到D啦!count = 0
这里是节点H:即将往下试探
下来到H啦!count = 0
这里是节点J:即将往下试探
J: 触底啦!返回一个1
这里是节点K:即将往下试探
K: 触底啦!返回一个1
这里是节点L:即将往下试探
L: 触底啦!返回一个1
上来到H了:此时的count是3
上来到D了:此时的count是3
上来到A了:此时的count是6
终端节点数量：6
*/
```

#### **2.2.8.2 说明**
* **先序遍历**的顺序是：先访问当前节点，然后依次递归访问每个子节点。
* Dart 中使用 `List<TreeNode>` 表示多子节点结构，适合构建通用树（非二叉树）。
* `const []` 是 Dart 中初始化不可变空列表的写法，用作默认参数。
* 结构：自定义类 + 递归
* 应用：层级数据（如分类目录）
---

### **2.2.9 遍历对比**

| 示例 | 数据结构 | 遍历方式 | 输出特点 |
|------|----------|----------|----------|
| 1    | 三维集合 | 顺序遍历 | 按类别分组输出 |
| 2    | 两个列表 | 笛卡尔积 | 生成所有组合 |
| 3    | 树形结构 | 递归先序 | 深度优先遍历 |


---

## **2.3 `while` 和 `do-while`**
- **`while`**（先判断条件）：
```dart
  int count = 0;
  while (count < 3) {
    print(count); // 输出 0, 1, 2
    count++;
  }
```
- **`do-while`**（至少执行一次）：
```dart
  int num = 0;
  do {
    print(num); // 输出 0
    num++;
  } while (num < 0); // 条件不满足，仅执行一次
```

---

## **2.4 `assert`（仅用于Debugging）**

### 2.4.1 基本语法
在 Dart 里，`assert` 是一个内置的调试工具，其基本语法如下：
```dart
assert(condition, [message]);
```
- `condition`：这是一个布尔表达式，必须返回 `true` 或者 `false`。若结果为 `false`，则会触发断言失败，抛出 `AssertionError` 异常。
- `message`（可选）：当断言失败时显示的字符串信息，用于描述错误原因。

以下是一个简单示例：
```dart
int age = -5;
assert(age >= 0, '年龄不能为负数'); // 抛出 AssertionError
```
在上述示例中，`age >= 0` 这个条件为 `false`，所以会抛出 `AssertionError`，并且会显示错误信息 `'年龄不能为负数'`。

### 2.4.2 特点
1. **仅在调试模式下生效（发布版本会被移除）**：
    - Dart 程序有调试模式（Debug）和生产模式（Release）。在调试模式下，`assert` 语句会正常执行，用于帮助开发者发现代码中的逻辑错误。而在生产模式下（例如通过 `--release` 标志编译的代码），`assert` 语句会被完全移除，不会影响程序的性能。
    - 例如在 Flutter 开发中，Debug 模式对应 Dart 的 JIT 模式，会打开所有的断言（`assert`）；Release 模式对应 Dart 的 AOT 模式，会关闭所有的断言。
2. **第二个参数为可选错误信息**：
    - 当断言条件为 `false` 时，若提供了第二个参数，那么这个参数的值会作为错误信息显示出来，帮助开发者更清晰地了解断言失败的原因。如果不提供第二个参数，则会使用默认的错误信息。
    - 示例：
```dart
void setAge(int age) {
  assert(age >= 0 && age <= 120, 'Age must be between 0 and 120');
  print('Age set to: $age');
}

void main() {
  setAge(-5); // 触发断言错误，会显示 'Age must be between 0 and 120'
}
```
3. **常用于验证输入或中间状态**：
    - 在函数或方法中，可以使用 `assert` 来验证输入参数是否符合预期。在类的关键逻辑中，也能使用 `assert` 来验证对象的状态是否合法。在开发阶段，还可以使用 `assert` 来验证代码中的假设是否成立。
    - 示例：
```dart
class BankAccount {
  double balance = 0;

  void deposit(double amount) {
    assert(amount > 0, 'Deposit amount must be positive');
    balance += amount;
  }
}

void main() {
  var account = BankAccount();
  account.deposit(100); // 正常执行
  account.deposit(-50); // 触发断言错误，会显示 'Deposit amount must be positive'
}
```

## **2.5 控制流进阶技巧**
### **2.5.1 级联操作（Cascade）**
Dart 中的级联操作符（`..`）允许你在同一个对象上连续调用多个方法或设置多个属性，而不需要重复对象名。
```dart
List<int> numbers = [1, 2, 3];
numbers..add(4) // 级联调用
       ..forEach(print); // 输出 1, 2, 3, 4
```

### **2.5.2 三元表达式（Ternary Operator）**
替代简单 `if-else`：
```dart
int a = 10, b = 20;
String result = (a > b) ? 'a大' : 'b大'; // 输出 'b大'
```

### **2.5.3 空安全与控制流**
Dart 3.0+ 支持更安全的空值处理：
```dart
String? name;
print(name?.toUpperCase() ?? '匿名'); // 输出 '匿名'（若 name 为 null）
```

---


## **2.6 注意事项**
### 2.6.1 **类型严格性**  
   条件表达式必须返回布尔值，`if (score)` 会报错（需显式写 `if (score > 0)`）。
### 2.6.2 **`switch` 的局限性**  
   Dart 不支持 `fall-through`（除非显式省略 `break`），但可通过 `case` 合并实现多条件。
### 2.6.3 **性能优化**  
   - 避免在循环内重复计算（如 `for (int i = 0; i < list.length; i++)` 可缓存 `list.length`）。
   - 大集合遍历优先用 `for-in` 或 `forEach`（代码更简洁）。



---

# **3. 函数**

## **3.1 函数基础语法**
### **3.1.1 标准函数定义**
#### **3.1.1.1 函数结构示意图**
```dart
返回值的类型 函数名(参数a, 参数b...) {
      // 函数体
    return 返回值; // 非void类型需返回值
}
```
#### **3.1.1.2 `yield`**
 `return` 会终止所处代码块的执行。但有的时候，我们希望函数在返回一个值的同时，不会终止当前函数的执行，这时候就要用到 `yield`

- 使用 `return`（普通函数，会提前终止）：
```dart
Iterable<int> numbersWithReturn() {
  for (int i = 0; i < 5; i++) {
    return i;        // 执行到这里就直接返回，后面代码不会执行
  }
  print("这里永远不会打印");
}

void main() {
  print(numbersWithReturn());  // 只输出 0
}
```

- 使用 `yield`（生成器函数，不会终止）：
```dart
// 同步生成器
Iterable<int> numbers() sync* {
  for (int i = 0; i < 5; i++) {
    yield i;         // 产生一个值，暂停，下次继续
  }
  print("生成器执行完毕！");
}

void main() {
  for (var n in numbers()) {
    print(n);
  }
  // 输出：
  // 0
  // 1
  // 2
  // 3
  // 4
  // 生成器执行完毕！
}
```

#### **3.1.1.3 传统写法与箭头函数**
```dart
// 传统写法（多行）
int add(int a, int b) {
  return a + b;
}

// 箭头函数（单行，自动返回）
int multiply(int a, int b) => a * b;
```
#### **3.1.1.4 `print` 函数返回 `void`**
```dart
void printMessage() => print('Hello, Dart!');
```

### **3.1.2 函数类型**
- 显式声明函数类型变量：
```dart
  int Function(int, int) mathOperation = (a, b) => a + b;
  print(mathOperation(2, 3)); // 输出 5
```

---

## **3.2 参数类型**

1. 必需参数（位置参数）
2. 命名参数（含 `required`、默认值）
3. 可选位置参数

---

### **3.2.1 必需参数 `()`**
- 按顺序传，全部必填
```dart
void createAccount(String username, String password, int age, String country) {
  print('Username: $username, Age: $age, Country: $country');
}

void main() {
  createAccount('user123', 'pass456', 30, 'China'); // ✅ 正确
  // createAccount('user123'); // ❌ 错误：缺少参数
}
```

---

### **3.2.2 命名参数 `{}`**
- 以类似于键值对的方式明确传参，可省略参数，可加默认值（可选参数的默认值必须是编译期常量，不能是变量），也可以 `required` 强制传参
```dart
void createProfile({
  required String username,
  required String email,
  int age = 18, // 加默认值 (必须是编译期常量，不能是变量)
  String country = 'Unknown', // 加默认值 (必须是编译期常量，不能是变量)
  bool isPremium = false, // 加默认值 (必须是编译期常量，不能是变量)
}) {
  print(
    'User: $username, Email: $email, Age: $age, Country: $country, Premium: $isPremium',
  );
}

void main() {
  createProfile(
    username: 'alice',
    email: 'alice@example.com',
    age: 25,
    isPremium: true,
  );
  /* 输出：
User: alice, Email: alice@example.com, Age: 25, Country: Unknown, Premium: true
*/

  // 也可以省略部分参数（使用默认值）
  createProfile(username: 'bob', email: 'bob@example.com');
  /* 输出：
User: bob, Email: bob@example.com, Age: 18, Country: Unknown, Premium: false
*/
}
```

---

### **3.2.3 可选位置参数 `[]`**
- 按顺序传，可省略后面的
```dart
void sendMessage(
  String content, [
  String? sender,
  String? receiver,
  String? time,
]) {
  print('Message: $content');
  print('From: $sender, To: $receiver, At: $time');
}

void main() {
  sendMessage('Hello!'); // 只传 message
  sendMessage('Hello!', 'Alice'); // 传 sender
  sendMessage('Hello!', 'Alice', 'Bob', '12:00'); // 全部传
}
```

---

### **3.2.4 混合写法 (必需参数{命名参数})**
**注意：命名参数要在必需参数后面**
```dart
void placeOrder(
  String productId,
  int quantity, {
  String address = 'Warehouse',
  bool express = false,
  String? note,
}) {
  print('Product: $productId x$quantity');
  print('Ship to: $address, Express: $express, Note: $note');
}

void main() {
  // 调用示例 1：
  placeOrder('A123', 5);
  /* 输出：
Product: A123 x5
Ship to: Warehouse, Express: false, Note: null
*/

  // 调用示例 2：
  placeOrder('A123', 2, express: true, note: 'Gift wrap');
  /* 输出：
Product: A123 x2
Ship to: Warehouse, Express: true, Note: Gift wrap
*/
}
```

---

### 3.2.5 总结
* 一般来说，必需参数（位置参数）和命名参数用得最多
* 用位置参数，顺序不能错；
* 用命名参数，名字得写清；
* 参数多时，推荐命名参数，**默认值 + `required` 更安全**；
* 可选参数可以传也可以不传，但**默认值只能用于可选参数**。

---

## **3.3 函数作为参数**
- 将函数作为参数传递：
```dart
  void execute(Function operation) {
    operation(5, 3);
  }
  
  execute((a, b) => print(a - b)); // 输出 2
```

## **3.4 匿名函数**
- 直接定义未命名的函数：
```dart
  var list = [1, 2, 3];
  list.forEach((item) => print(item)); // 输出 1, 2, 3
```

## **3.5 递归函数**

* **定义**：递归函数是指在函数内部调用自身的函数。
* **常用于**：树形结构遍历、数学计算（如阶乘、斐波那契）、搜索算法等。
* **关键组成**：

  * **终止条件**：避免无限递归
  * **递归调用**：函数调用自身
  * **状态推进**：每次递归都向终止条件逼近

---

### 3.5.1 示例 1：计算阶乘

```dart
int factorial(int n) {
  if (n == 0 || n == 1) return 1; // 终止条件
  return n * factorial(n - 1);    // 递归调用
}

void main() {
  print(factorial(5)); // 输出 120
}
```

---

### 3.5.2 示例 2：树的先序遍历

```dart
class TreeNode {
  String name;
  List<TreeNode> children;

  TreeNode({required this.name, this.children = const []}); // 可选参数的默认值必须是编译期常量
}

// 统计终端节点数量
int countLeaves(TreeNode node) {
  if (node.children.isEmpty) {
    // 终止条件
    print('${node.name}: 触底啦!返回一个1');
    return 1;
  }
  print('下来到${node.name}啦!count = 0');
  int count = 0;
  for (var child in node.children) {
    print('这里是节点${child.name}:即将往下试探');
    count += countLeaves(child); // 递归调用
  }
  print('上来到${node.name}了:此时的count是${count}');
  return count;
}

void main() {
  /*

 构建一个树形结构：
         A
      /  |   \
     B   C    D
    /   / \     \
   F   E   G     H
      /       / | \
     I       J  K  L

*/
  var root = TreeNode(
    name: 'A',
    children: [
      TreeNode(name: 'B', children: [TreeNode(name: 'F')]),
      TreeNode(
        name: 'C',
        children: [
          TreeNode(name: 'E', children: [TreeNode(name: 'I')]),
          TreeNode(name: 'G'),
        ],
      ),
      TreeNode(
        name: 'D',
        children: [
          TreeNode(
            name: 'H',
            children: [
              TreeNode(name: 'J'),
              TreeNode(name: 'K'),
              TreeNode(name: 'L'),
            ],
          ),
        ],
      ),
    ],
  );

  print('终端节点数量：${countLeaves(root)}');
}

/*输出结果：
下来到A啦!count = 0
这里是节点B:即将往下试探
下来到B啦!count = 0
这里是节点F:即将往下试探
F: 触底啦!返回一个1
上来到B了:此时的count是1
这里是节点C:即将往下试探
下来到C啦!count = 0
这里是节点E:即将往下试探
下来到E啦!count = 0
这里是节点I:即将往下试探
I: 触底啦!返回一个1
上来到E了:此时的count是1
这里是节点G:即将往下试探
G: 触底啦!返回一个1
上来到C了:此时的count是2
这里是节点D:即将往下试探
下来到D啦!count = 0
这里是节点H:即将往下试探
下来到H啦!count = 0
这里是节点J:即将往下试探
J: 触底啦!返回一个1
这里是节点K:即将往下试探
K: 触底啦!返回一个1
这里是节点L:即将往下试探
L: 触底啦!返回一个1
上来到H了:此时的count是3
上来到D了:此时的count是3
上来到A了:此时的count是6
终端节点数量：6
*/
```

---

### 3.5.3 注意事项

* 每次递归都必须**推进状态**，朝终止条件靠近
* 没有终止条件会造成栈溢出（Stack Overflow）
* 递归可改为迭代，但有时递归更简洁直观

## 3.6 变量作用域
在Dart中，变量作用域指变量在代码里可访问的范围，其作用域规则遵循块级作用域，即由代码块 `{}` 的位置决定。主要有以下四种作用域：
### 3.6.1 **全局作用域**
程序中任何地方都能访问，通常在文件顶部声明的变量处于全局作用域，全局变量在整个程序生命周期内都有效。不过要慎用全局变量，过度使用会导致命名冲突和内存浪费。
```dart
int globalVar = 100; // 全局变量
void main() {
  print(globalVar); // 访问全局变量
}
```
### 3.6.2 **函数作用域**
在函数内部声明的变量，仅在该函数内部可访问，这类变量具有局部作用域，在函数执行时创建，执行结束后销毁。
```dart
void myFunction() {
  int localVar = 200; // 局部变量
  print(localVar); // 只能在函数内部访问
}
void main() {
  myFunction();
  // print(localVar); // 错误：无法访问函数外部的 localVar
}
```
### 3.6.3 **块级作用域**
Dart中被大括号 `{}` 包围的代码块会创建新的作用域，像控制语句 `if`、`for` 和 `while` 等都会创建新作用域。
```dart
void main() {
  if (true) {
    var blockVar = 300; // 块级变量
    print(blockVar); // 仅在该块内部访问
  }
  // print(blockVar); // 错误：无法在块外部访问 blockVar
}
```
```dart
for (var i = 0; i < 3; i++) {
  print(i); // 输出0,1,2
}
// print(i); // 报错，Dart的for循环计数器变量作用域仅限于for循环的代码块内部
```
### 3.6.4 **闭包作用域**
闭包是Dart的特殊功能，一个函数能“记住”并访问它创建时的外部作用域中的变量，即便外部函数已执行完毕，闭包仍可访问外部函数的局部变量。但要注意，被捕获的变量会一直存在内存中，可能导致内存泄漏。
```dart
Function createClosure() {
  int outerVar = 400; // 外部变量
  return () {
    print(outerVar); // 闭包可以访问外部变量
  };
}
void main() {
  var closure = createClosure();
  closure(); // 输出外部变量的值
}
```
### 3.6.5 **作用域优先级**
当变量名冲突时，遵循就近原则，优先访问最近作用域的变量。若局部变量和全局变量同名，局部变量会覆盖全局变量，局部变量的作用域优先于全局变量，直到局部作用域结束。
```dart
int x = 500; // 全局变量
void main() {
  int x = 600; // 局部变量，覆盖了全局变量
  print(x); // 输出局部变量的值
}
```
## 3.7 函数访问范围

在Dart语言里，公共函数和私有函数是控制函数访问范围的重要概念，以下为你详细介绍：

### 3.7.1 公共函数
在Dart中，默认情况下，所有未加下划线的标识符都是公共的（Public），公共函数的作用范围是可以从任何地方访问，等价于其他语言中的`public`。定义公共函数时，只需使用普通的命名方式，无需前缀。以下是一个示例：
```dart
class MyClass {
  // 公共函数
  void publicMethod() {
    print('This is a public method');
  }
}

void main() {
  var obj = MyClass();
  obj.publicMethod(); // 可以正常调用公共函数
}
```
上述代码中，`publicMethod`就是一个公共函数，在`main`函数里可以正常调用它。

### 3.7.2 私有函数
Dart使用下划线`_`前缀来表示私有成员，私有函数只能在定义它们的库（library）内访问，不会对其他库公开，等价于其他语言中的`private`。以下是私有函数的示例：
```dart
class MyClass {
  // 私有函数
  void _privateMethod() {
    print('This is a private method');
  }

  // 公共函数调用私有函数
  void callPrivateMethod() {
    _privateMethod();
  }
}

void main() {
  var obj = MyClass();
  obj._privateMethod(); // 错误，无法直接访问私有函数
  obj.callPrivateMethod(); // 可以通过公共函数间接调用私有函数
}
```
在这个例子中，`_privateMethod`是私有函数，不能直接在`main`函数中访问，但可以通过类中的公共函数`callPrivateMethod`间接调用它。

### 3.7.3 总结
- **公共函数**：默认情况下，未加下划线的函数就是公共函数，作用范围是全局可访问。
- **私有函数**：以`_`开头的函数为私有函数，只能在定义它的库内访问。

---

## **3.8 函数返回值**
在Dart中，函数的返回值可以是多种类型，包括基本数据类型（如`int`、`double`等）、对象类型（如`String`、`Widget`等），当函数没有显式使用`return`语句返回值时，默认返回`null`。以下是一些不同返回值类型的函数示例：

### 3.8.1 返回`int`类型
```dart
// 计算两个整数的和
int add(int a, int b) {
  return a + b;
}

void main() {
  int result = add(3, 5);
  print(result); // 输出 8
}
```
在这个例子中，`add`函数的返回类型是`int`，它接收两个整数参数`a`和`b`，并返回它们的和。

### 3.8.2 返回`String`类型
```dart
// 拼接两个字符串
String joinStrings(String str1, String str2) {
  return str1 + str2;
}

void main() {
  String combined = joinStrings('Hello', ' World');
  print(combined); // 输出 Hello World
}
```
这里`joinStrings`函数的返回类型为`String`，它将传入的两个字符串拼接起来并返回结果。

### 3.8.3 返回`Widget`类型（以Flutter为例）
```dart
import 'package:flutter/material.dart';

// 创建一个简单的文本组件
Widget createTextWidget(String text) {
  return Text(text);
}

void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Center(
        child: createTextWidget('Hello, Flutter!'),
      ),
    ),
  ));
}
```
在Flutter开发中，`Widget`是构建用户界面的基本单元。`createTextWidget`函数接收一个字符串参数，返回一个`Text`类型的`Widget`，用于在界面上显示文本。

### 3.8.4 隐式返回`null`的情况
```dart
// 根据条件返回不同的字符串，若不满足条件则隐式返回 null
String? findGreeting(String name) {
  if (name == 'Bob') {
    return 'Hello, Bob!';
  }
  // 没有 return 语句，隐式返回 null
}

void main() {
  String? greeting = findGreeting('Alice');
  print(greeting); // 输出 null
}
```
此例中，`findGreeting`函数只有在传入的`name`为`'Bob'`时才会返回一个字符串，其他情况下没有`return`语句，会隐式返回`null`。

### 3.8.5 返回`double`类型
```dart
// 计算两个数的平均值
double calculateAverage(double num1, double num2) {
  return (num1 + num2) / 2;
}

void main() {
  double average = calculateAverage(10.5, 20.5);
  print(average); // 输出 15.5
}
```
`calculateAverage`函数的返回类型是`double`，它接收两个双精度浮点数参数，计算并返回它们的平均值。

### 3.8.6 返回`List`类型
```dart
List<int> filterEven(List<int> numbers) {
  return numbers.where((n) => n % 2 == 0).toList();
}
```
### 3.8.7 返回`Map`类型
```dart
Map<String, double> lengthMap() {
  return {'friend': 18.0, 'guy': 15.9, 'alien': 20.1};
}

void main() {
  final size = lengthMap();
  print(size); //输出： {'friend': 18.0, 'guy': 15.9, 'alien': 20.1};
}
```
## **3.9 注意事项**
1. **参数顺序**：  
   命名参数 `{}` 必须要在必需参数（位置参数）之后：
 ```dart
   // 正确 ✅
   void func(int a, {int b}) {}
   
   // 错误 ❌
   void func({int b}, int a) {} // 编译错误
 ```

2. **可选参数限制**：  
   命名参数不能有默认值的冲突（如两个同名参数）。
 ```dart
   // 正确 ✅
  String func({String a = '编', String b = '程'}) {
  return a + b;
}

   // 错误 ❌
  String func({String a = '编', String a = '程'}) {
  return a + a;
} // 报错：两个同名参数

 ```
3. **箭头函数限制**：  
   箭头函数只能用于单行表达式，多行逻辑需用传统写法：
 ```dart
   // 错误示例 ❌
   int add(int a, int b) => {
     print('Adding...');
     a + b // 缺少分号或括号会报错
   };
   
   // 正确写法 ✅
   int add(int a, int b) {
     print('Adding...');
     return a + b;
   }
 ```

---

## **3.10 回调函数**
- 如何理解回调函数呢？
想象一下这里有3个东西，外函数、内函数、参数C。内函数作为外函数的一个参数传入外函数。当外函数被调用执行时，外函数会逐行执行内部代码，一旦到了内函数的那一行，就会（如果有参数C的话）往内函数传入参数C，并执行内函数（不管有没有参数C传入都会执行）。这个内函数也被称作"**回调函数**"，这个过程也被称作"**使用'内函数'作为回调**"。

### **3.10.1 语法结构示意图**
```dart
  外函数返回值的类型 外函数(callBack) {
         参数C = 一顿操作;
         callBack(参数C);
}

  内函数a返回值的类型 内函数a(匹配参数C的类型) {
          一顿操作;
}

  内函数b返回值的类型 内函数b(匹配参数C的类型) {
          一顿操作;
}

   void main() {  
   外函数(内函数a); // 使用内函数a作为回调
   外函数(内函数b); // 使用内函数b作为回调
}
```
### **3.10.2 回调函数作为参数时的书写格式1**
```dart
// 回调函数作为参数传入时，书写格式为 "返回值类型 Function(回调函数参数值的类型) 回调函数的名字" 

// 1. 定义一个具名函数（加法）
int add(int x, int y) {
  return x + y;
}

// 2. 定义另一个具名函数（乘法）
int multiply(int x, int y) {
  return x * y;
}

// 3. 定义一个函数，接收两个数字和一个“算法回调”
void calculate(int a, int b, int Function(int, int) compute) {
  print("开始计算...");
  int result = compute(a, b); 
  print("计算结果是: $result");
}

void main() {
  // 4. 直接传入函数名（注意：不要带括号，带括号就变成直接执行了）
  calculate(10, 5, add); 
  
  calculate(10, 5, multiply);
}

/**
 * 控制台输出结果：
 * * 开始计算...
 * 计算结果是: 15
 * 开始计算...
 * 计算结果是: 50
 */
```
### **3.10.3 回调函数作为参数时的书写格式2**
```dart
// 若回调参数既不接受参数，也不返回任何值时候，"void Function() 回调函数的名字" 还可以写作 "VoidCallback 回调函数的名字" 

void runTask(VoidCallback action) {
  print("准备开始执行...");
  action(); // 调用传入的回调函数
  print("任务执行完毕。");
}

void main() {
  // 2. 传入一个匿名函数作为参数
  runTask(() {
    print("我是被传进来的回调逻辑！");
  });
}

/*  控制台输出结果：
准备开始执行...
我是被传进来的回调逻辑！
任务执行完毕。
*/
```

### **3.10.4 🧮 示例1：数学处理** 
```dart
// 定义一个函数，接收一个数字和一个回调函数，对数字进行处理
void processNumber(int number, int Function(int) callback) {
  int result = callback(number);
  print("处理结果是：$result");
}

// 回调函数：将数字加倍
int doubleIt(int x) {
  return x * 2;
}

// 回调函数：计算平方
int squareIt(int x) {
  return x * x;
}

void main() {
  processNumber(5, doubleIt);   // 输出：处理结果是：10
  processNumber(7, squareIt);   // 输出：处理结果是：49

  // 也可以使用匿名函数作为回调
  processNumber(3, (x) => x + 100);  // 输出：处理结果是：103
}
```

### **3.10.5 🐔 示例2：鸡和香蕉的故事**  
```dart
void cookFood(Function(String) callback) {
  print("👨‍🍳 厨师开始做饭..."); // 输出: 👨‍🍳 厨师开始做饭...
  
  Future.delayed(Duration(seconds: 1), () {
    String food = DateTime.now().second.isEven ? "🐔 炸鸡" : "🍌 香蕉派";
    print("🍽️ 做好了: $food"); // 输出: 🍽️ 做好了: 🐔 炸鸡 或 🍌 香蕉派
    callback(food); // 回调告诉顾客吃啥
  });
}

void main() {
  cookFood((String food) {
    print("😋 顾客吃到了: $food"); // 输出: 😋 顾客吃到了: 🐔 炸鸡 或 🍌 香蕉派
  });
  
  print("🕒 等待上菜中..."); // 输出: 🕒 等待上菜中... (立即执行)
}
```
**可能的输出（随机）**：
```
👨‍🍳 厨师开始做饭...
🕒 等待上菜中...
🍽️ 做好了: 🐔 炸鸡
😋 顾客吃到了: 🐔 炸鸡
```
或
```
👨‍🍳 厨师开始做饭...
🕒 等待上菜中...
🍽️ 做好了: 🍌 香蕉派
😋 顾客吃到了: 🍌 香蕉派
```

---

### **3.10.6 🍌 示例3：Future + 错误回调（香蕉快递）**  
```dart
Future<String> deliverBanana(bool isBroken) async {
  await Future.delayed(Duration(seconds: 1));
  if (isBroken) {
    throw Exception("💥 香蕉被压烂了！");
  } else {
    return "🍌 新鲜香蕉送达！";
  }
}

void main() async {
  print("📦 下单香蕉...");

  try {
    String banana = await deliverBanana(false); // 👈 await 等待结果
    print("✅ 收到: $banana");
  } catch (error) {
    print("❌ 错误: $error");
  } finally {
    print("🔚 无论成功或失败我都执行（包装纸丢掉）");
  }

  print("🚚 等待快递...（虽然写在后面，但会等 async 结束）");
}
```
**两种可能的输出**：
1. **成功**：
  ```dart
📦 下单香蕉...
✅ 收到: 🍌 新鲜香蕉送达！
🔚 无论成功或失败我都执行（包装纸丢掉）
🚚 等待快递...（虽然写在后面，但会等 async 结束）
  ```
2. **失败**（如果 `deliverBanana(true)`）：
  ```dart
📦 下单香蕉...
❌ 错误: Exception: 💥 香蕉被压烂了！
🔚 无论成功或失败我都执行（包装纸丢掉）
🚚 等待快递...（虽然写在后面，但会等 async 结束）

  ```

---

## **3.11 高阶函数（返回函数）**
```dart
Function createMultiplier(int factor) {
  return (int x) => x * factor;
}

var doubleIt = createMultiplier(2);
print(doubleIt(5)); // 输出 10
```

---

# **4. 面向对象**
## **4.1 类与对象（Class & Object）**
在 Dart 中，**类（class）** 就像是一个“模板”或“设计图”，用来描述一类事物具有什么样的属性（数据）和能做什么样的事情（方法）。

**对象（object）** 就是根据这个模板创建出来的“实际的东西”。你可以把类想象成是“设计图”，而对象就是“根据设计图造出来的产品”。

通过类和对象，我们可以把代码写得更清晰、结构更好，而且可以反复利用这份“设计图”创建多个类似的对象。
### 4.1.0 创建对象语法示意
- 带注释版
```dart
class 类名 {
  // 1.类的属性
  这里创建属性

  // 2.主构造函数，也叫默认构造函数（Default Constructor）:用于初始化基本属性
   类名(this.属性1, this.属性2) // 构造函数初始化列表
      : 属性a = A值,   
        属性b = B值,   
        属性c = C值 {函数体};  // 主构造函数的函数体往往省略


/* 'this.' 自动将传入的参数赋值给对应的属性。

 构造函数参数列表后面可以接一个初始化列表，也可以不接，
 不接的时候直接写 "类名(构造函数参数列表);" 就可以了。

 如果没有写主构造函数，Dart 会默认提供一个不传入任何参数的默认构造函数
*/


  // 3.命名构造函数（Named Constructor）[选学]
   类名.命名构造函数(构造函数参数列表)
      : 属性aa = AA值,  // ————|初始化|
        属性bb = BB值,  //————|列表|
        属性cc = CC值;   

/* 在 Dart 中，
“命名构造函数（Named Constructor）”之所以这么叫，
是因为它给构造函数取了一个名字，区别于主构造函数（默认构造函数）。
你可以在同一个类中定义多个命名构造函数，
以支持不同的初始化方法。
当然，在一般情况下，主构造函数就够用了。
*/


  // 4.类的方法
   void 方法名() {
      // 方法内容
   }
}

void main() {

  // 使用主构造函数创建对象
    类名(参数1, 参数2...); 
    类名();

/* ⚠️⚠️⚠️ 重要提醒 ⚠️⚠️⚠️
 * 对！你没有看错！
 * 每次写 '类名(参数)' 都会创建一个全新的对象实例！🆕
 * 如果你不用变量接住它，这个对象就只能临时用一下，然后就被回收了。
 *
 * 如果你想重复使用这个对象（难道你要复制粘贴参数一万次？😱），
 * 建议用变量保存对象，像这样：
 *   var 变量 = 类名(参数); // ✅ 推荐写法
 *
 * 否则会导致：
 *   - 🗑️ 对象用完即丢，无法复用
 *   - 🔁 重复代码，容易出错
 *   - 🧠 内存浪费（创建一万个看起来一样但其实不一样的对象？🚫）
 */



  // 使用不带参数的主构造函数创建对象，并声明一个变量'对象1'接收
  var 对象1 = 类名();

  // 使用带参数的主构造函数创建对象，并声明一个变量'对象2'接收
  var 对象2 = 类名(参数1, 参数2...);

  // 使用命名构造函数创建对象，并声明一个变量'对象3'接收
  var 对象3 = 类名.命名构造函数();

  // 调用属性
  对象2.属性

  // 调用方法
  对象2.方法名();

}
```
- 不带注释版
```dart
class 类名 {
  这里创建属性

   类名(this.属性1, this.属性2);  

   类名.命名构造函数(构造函数参数列表)
      : 属性aa = AA值,  
        属性bb = BB值,  
        属性cc = CC值;   

   void 方法名() {
      一顿操作;
   }
}

void main() {

    类名(参数1, 参数2...); 
    类名();

  var 对象1 = 类名();

  var 对象2 = 类名(参数1, 参数2...);

  var 对象3 = 类名.命名构造函数();

  对象2.属性

  对象2.方法名();

}
```

### 4.1.1 创建对象示例1 水果
```dart
// 定义一个名为 Fruit 的类，类就像是一个模板，用来创建具有相同属性和行为的水果对象
class Fruit {
  // 属性（字段），用来描述水果的特征
  // name 表示水果的名字，类型是 String，也就是字符串
  String name;
  // color 表示水果的颜色，类型是 String
  String color;
  // price 表示水果的价格，类型是 double
  double price;

  // 主构造函数的作用是创建对象时进行初始化操作
  // 这里使用语法糖，让代码更简洁，它会自动将传入的参数赋值给对应的属性
  Fruit(this.name, this.color, this.price);

  // 方法，方法就是对象能做的事情，这里 describe 方法用来让水果做自我介绍
  void describe() {
    // 使用字符串插值，将 name、color 和 price 的值插入到字符串中
    print('I am a $color $name, and my price is $price dollars per kilogram.');
  }

  // 命名构造函数，命名构造函数可以让我们以不同的方式创建对象
  // 这里从 JSON 数据中创建 Fruit 对象
  // 初始化列表以冒号 ':' 开头，用于在对象创建时对属性进行初始化
  Fruit.fromJson(Map<String, dynamic> json) 
      : name = json['name'] ?? 'Unknown',// 如果 JSON 数据中有 'name' 键，则使用该值，否则使用 'Unknown'
        color = json['color'] ?? 'Unknown',// 如果 JSON 数据中有 'color' 键，则使用该值，否则使用 'Unknown'
        price = json['price'] ?? 0; // 如果 JSON 数据中有 'price' 键，则使用该值，否则使用 0
} 

void main() {
  // 实例化对象，就像是用模板创造出一个具体的水果
  // 调用 Fruit 类的主构造函数，传入名字 'Apple'、颜色 'Red' 和价格 5.0
  var fruit1 = Fruit('Apple', 'Red', 5.0);
  // 调用水果的 describe 方法，让这个水果做自我介绍
  fruit1.describe(); // 输出: I am a Red Apple, and my price is 5.0 dollars per kilogram.

  // 使用命名构造函数，从 JSON 数据中创建 Fruit 对象
  // 定义一个 Map 类型的 JSON 数据，模拟从网络或文件中获取的数据
  var fruit2 = Fruit.fromJson({'name': 'Banana', 'color': 'Yellow'});
  // 打印这个水果的名字
  print(fruit2.name); // 输出: Banana
}
```
### 4.1.2 创建对象示例2 家具
```dart
// 定义一个名为 Furniture 的类，类就像是一个模板，用来创建具有相同属性和功能的家具对象
class Furniture {
  // 属性（字段），用来描述家具的特征
  // typeName 表示家具的种类，比如椅子、桌子、镜子
  String? typeName;
  // material 表示家具使用的材料，比如木头、金属、玻璃
  String? material;
  // usage 表示家具的功能，比如用于坐、放东西等
  String? usage;

  // 主构造函数，创建对象时初始化属性
  Furniture(this.typeName, this.material, this.usage);

  // 方法，让家具进行自我介绍
  void describe() {
    print('This is a $material $typeName, mainly used for $usage.');
  }
  // 方法，如何制造家具
  void made() {
    print('To make a $typeName, you primarily need $material.');
  }

  // 命名构造函数：从 JSON 数据创建家具对象
  Furniture.fromJson(Map<String, dynamic> json)
      : typeName = json['typeName'] ?? 'Unknown',
        material = json['material'] ?? 'Unknown',
        usage = json['usage'] ?? 'No specific use';
}

void main() {
  // 使用主构造函数创建一个家具对象chair
  var chair = Furniture('chair', 'wood', 'sitting');
  chair.describe(); // 输出: This is a wood chair, mainly used for sitting.

// 使用主构造函数创建一个家具对象mirror
  var mirror = Furniture('mirror', 'glass');
  mirror.made(); // 输出: To make a mirror, you primarily need ​glass.

  // 使用命名构造函数从 JSON 创建对象
  var table = Furniture.fromJson({'typeName': 'table', 'material': 'metal'});
  print(table.typeName); // 输出: table
  print(table.usage); // 输出: No specific use
}
```
### 4.1.3 创建对象示例3 商品
```dart
class Product {
  // 1. 类的属性
  String name;
  double price;
  String category;
  bool inStock;

  // 2. 主构造函数
  Product(this.name, this.price, this.category, this.inStock);

  // 3. 命名构造函数 ：用于创建一个默认的商品
  Product.defaultProduct()
      : name = '未知商品',
        price = 0.0,
        category = '未分类',
        inStock = false;

  // 4. 类的方法
  void displayInfo() {
    print('商品信息:');
    print('名称: $name');
    print('价格: ¥$price');
    print('分类: $category');
    print('是否有库存: ${inStock ? '是' : '否'}');
  }
}

void main() {
  // 使用主构造函数创建对象
  var product1 = Product('苹果手机', 6999.0, '电子产品', true);

  // 使用命名构造函数创建对象
  var product2 = Product.defaultProduct();

  // 调用属性
  print('商品1名称: ${product1.name}'); // 输出: 商品1名称: 苹果手机
  print('商品2价格: ${product2.price}'); // 输出: 商品2价格: 0.0

  // 调用方法
  product1.displayInfo();
  /*
  输出:
  商品信息:
  名称: 苹果手机
  价格: ¥6999.0
  分类: 电子产品
  是否有库存: 是
  */

  product2.displayInfo();
    /*
  输出:
  商品信息:
  名称: 未知商品
  价格: ¥0.0
  分类: 未分类
  是否有库存: 否
  */
}
```
### 4.1.4 创建对象示例4 书本
```dart
// 在前面的3个示例中，主构造函数里面并没有使用灵活的'命名参数'形式传参，所以我将在示例4里弥补这个遗憾
class Book {
  final String bookName; // 书名（必需）
  final String isbn;     // ISBN（必需）
  final double? price;   // 价格（可选）
  final String? introduction; // 简介（可选）
  final int? page;       // 页数（可选）

  // 主构造函数（必需参数 + 命名参数）
  Book({
    required this.bookName,
    required this.isbn,
    this.price,
    this.introduction,
    this.page,
  }) {
    // 构造函数内可以添加逻辑
    _validateBook(); // 创建时验证书籍
  }

  // 命名构造函数：创建一个"免费书"（price = 0）[选学]
  Book.free({
    required this.bookName,
    required this.isbn,
    this.introduction,
    this.page,
  }) : price = 0;

  // 命名构造函数：创建一个"神秘书"（随机生成属性）[选学]
  Book.mystery()
      : this(
          bookName: _generateRandomTitle(),
          isbn: _generateRandomISBN(),
          price: Random().nextDouble() * 100, // 随机价格（0-100）
          page: Random().nextInt(500) + 100,  // 随机页数（100-600）
        );

  // 方法：验证书籍信息
  void _validateBook() {
    if (page != null && page! <= 0) {
      throw ArgumentError('页数必须大于0！');
    }
    if (price != null && price! < 0) {
      throw ArgumentError('价格不能为负数！');
    }
  }

  // 方法：获取带折扣的价格 [选学]
  double? getDiscountedPrice(double discount) {
    if (price == null) return null;
    if (discount <= 0 || discount >= 1) {
      throw ArgumentError('折扣必须在0和1之间！');
    }
    return price! * discount;
  }

  // 方法：打印书籍信息
  void printInfo() {
    print('书名: $bookName');
    print('ISBN: $isbn');
    print('价格: ${price?.toStringAsFixed(2) ?? "未知"}');
    print('页数: ${page ?? "未知"}');
    if (introduction != null) {
      print('简介: ${introduction!.substring(0, introduction!.length.clamp(0, 20))}...');
    }
  }

  // 静态方法：生成随机书名 [选学]
  static String _generateRandomTitle() {
    final adjectives = ['魔法', '黑暗', '终极', '遗失', '疯狂'];
    final nouns = ['指南', '秘典', '物语', '之旅', '传说'];
    return '${adjectives[Random().nextInt(adjectives.length)]}${nouns[Random().nextInt(nouns.length)]}';
  }

  // 静态方法：生成随机ISBN [选学]
  static String _generateRandomISBN() {
    return 'ISBN-${Random().nextInt(9000) + 1000}';
  }
}

void main() {
  // 对象1：普通书籍
  final book1 = Book(
    bookName: 'Dart编程指南',
    isbn: 'ISBN-1234',
    price: 59.99,
    page: 300,
    introduction: '这是一本关于Dart语言的全面指南，适合初学者和高级开发者。',
  );
  book1.printInfo();
  /* 输出：
     书名: Dart编程指南
     ISBN: ISBN-1234
     价格: 59.99
     页数: 300
     简介: 这是一本关于Dart语言的全面指南...
  */
  print('折扣价: ${book1.getDiscountedPrice(0.8)?.toStringAsFixed(2)}'); // [选学]
  // 输出：折扣价: 47.99

  // 对象2：仅传入必需参数（bookName 和 isbn）
  final book2 = Book(
    bookName: 'Flutter实战',
    isbn: 'ISBN-5678',
  );
  book2.printInfo();
  /* 输出：
     书名: Flutter实战
     ISBN: ISBN-5678
     价格: 未知
     页数: 未知
              （未传入 introduction，故不显示简介行）
  */

  // 对象3：免费书籍 [选学]
  final freeBook = Book.free(
    bookName: '免费Flutter教程',
    isbn: 'ISBN-0000',
    introduction: '学习Flutter的入门教程',
  );
  freeBook.printInfo();
  /* 输出：
     书名: 免费Flutter教程
     ISBN: ISBN-0000
     价格: 0.00
     页数: 未知
     简介: 学习Flutter的入门教程...
  */

  // 对象4：随机神秘书籍 [选学]
  final mysteryBook = Book.mystery();
  print('\n神秘书籍:');
  mysteryBook.printInfo();
  /* 输出（随机示例）：
     神秘书籍:
     书名: 终极传说
     ISBN: ISBN-7890
     价格: 42.15
     页数: 450
     简介: null（如果随机生成的简介为空）
  */
}
```
## **4.2 Getter/Setter**
在 Dart 中，**直接字段**和 **getter/setter** 的主要区别体现在**封装性**、**灵活性**和**使用场景**上。以下是具体对比：

---

### **4.2.1 核心区别**
| **特性**         | **直接字段**                          | **Getter/Setter**                          |
|------------------|--------------------------------------|--------------------------------------------|
| **定义方式**     | 直接声明为公共变量（无下划线前缀）       | 通过 `get`/`set` 关键字定义，通常操作私有字段（`_field`） |
| **访问控制**     | 外部代码可自由读写                     | 可限制为只读（仅 getter）或添加逻辑验证（setter）       |
| **逻辑扩展**     | 无额外逻辑                             | 可在读取/写入时添加验证、计算或副作用（如日志记录）       |
| **封装性**       | 低（暴露实现细节）                     | 高（隐藏内部实现，通过方法控制访问）                |
| **典型场景**     | 简单数据存储，无需保护或处理逻辑         | 需要验证输入、动态计算或保护私有数据时               |

---

### **4.2.2 代码示例对比**
#### **(1) 直接字段**
```dart
class Person {
  String name; // 直接字段（外部可直接修改）
  int age;
}

void main() {
  var person = Person();
  person.name = 'Alice'; // 直接赋值
  person.age = -10;      // 非法值也能赋值（无保护），年龄为负数，逻辑上无意义
}
```

#### **(2) Getter/Setter 封装私有字段**
```dart
class Person {
  String _name = '';
  int _age = 0;

  // Getter（只读访问；不提供 setter，因此无法通过公开接口修改 _name）
  String get name => _name;

  // 如果想要（带有前提条件）赋值私有字段，用 Setter
  // Setter（带验证）
  set age(int value) {
    if (value >= 0) _age = value; // 只允许非负数
    else throw ArgumentError('Age cannot be negative');
  }

  // 动态计算属性
  String get info => 'Name: $_name, Age: $_age';
}

void main() {
  var person = Person();
  person._name = 'Alice'; // 直接访问私有字段（在同一个文件中是允许的，但不推荐）
  person.name = 'banana'; // ❌ 错误：`name` 只有 getter 没有 setter，不能赋值
  print(person._name);     // 访问私有字段，输出：Alice（同样只在本文件中可见）
  print(person.name);     // 通过 getter 访问 `_name`，输出：Alice
  person.age = -10;       // 抛出异常：Age cannot be negative
  print(person.info);     // 如果前面 age 抛出异常，这行不会执行；否则输出：Name: Alice, Age: 0
}
```

---

### **4.2.3 关键优势分析**
- **Getter/Setter 的用途**  
  - **数据保护**：防止非法值破坏对象状态（如负数年龄）。  
  - **动态计算**：如 `area` 属性通过半径实时计算，无需存储。  
  - **副作用管理**：属性变更时触发通知或日志。  
  - **兼容性**：未来可修改内部逻辑而不影响外部调用。

- **直接字段的适用场景**  
  - **简单DTO**：仅需传递数据时（如 `Point(x, y)`）。  
  - **性能敏感**：避免方法调用开销（极少数情况）。

---

### **4.2.4 设计建议**
- **优先使用 Getter/Setter**：  
  除非属性是纯粹的数据容器且无需任何逻辑控制，否则应通过 getter/setter 封装字段。
- **私有化字段**：  
  即使使用 getter/setter，也应将字段声明为私有（`_field`），强制通过方法访问。
  通过合理选择，可兼顾代码的**安全性**和**可维护性**。
---

## **4.3 单继承（Inheritance）**

### **4.3.0 语法结构示意**
```dart
class 父类 {
  父类的属性；
  父类的方法();
  同名方法();
}

class 子类 extends 父类 { 
  
  // 覆盖父类方法
  @override
  void 同名方法() {
    // 新内容
  }
  super.父类的方法(); // 调用父类方法
  super.父类的属性; // 调用父类属性
  子类的属性；
  子类的方法();
}

void main() {
  var 对象 = 子类();
  对象.父类的属性、方法();  
  对象.子类的属性、方法();  
}
```
### **4.3.1 示例1 动物**
```dart
class Animal {
  void breathe() => print('Breathing...');
}

class Dog extends Animal {
  void bark() => print('Woof!');
  
  // 覆盖父类方法
  @override
  void breathe() {
    super.breathe(); // 调用父类方法
    print('Dog is panting');
  }
}

void main() {
  var dog = Dog();
  dog.breathe(); // 输出: Breathing... \n Dog is panting
  dog.bark();    // 输出: Woof!
}
```
### **4.3.2 示例2 光源**
```dart
class Light {
  void shine() => print('Shining...');
}

class DeskLamp extends Light {
  void turnOn() => print('The power is on!');
  void turnOff() => print('The power is off!');
  
  // 覆盖父类方法
  @override
  void shine() {
    turnOn();
    super.shine(); // 调用父类方法
  }
}

void main() {
  var deskLamp = DeskLamp();
  deskLamp.shine(); // 输出: The power is on! \n Shining... 
  deskLamp.turnOff();    // 输出: The power is off!
}
```

## **4.4 混入（Mixin）**
### 4.4.0 语法结构示意
```dart
class 基类 { // 在Dart 中只能继承一个基类（单继承）
  基类的属性；
  基类的方法();
}

mixin  混入1 { // 在Dart 中可以混入多个 mixin（多继承）
  混入1的属性；
  混入1的方法();
}

mixin 混入2 {
  混入2的属性；
  混入2的方法();
}

class 子类 extends 基类 with 混入1, 混入2 {  //当不需要基类时，'extends 基类 '可省略
  子类的属性；
  子类的方法();
}

void main() {
  var 对象 = 子类();
  对象.基类的属性、方法();  
  对象.混入1的属性、方法();  
  对象.混入2的属性、方法();  

  // Mixin 不能定义构造函数
  var 对象 = 混入1(); // ❌ 编译错误

}
```
### 4.4.1 示例1
```dart
mixin Flyable {
  void fly() => print('Flying...');
}

mixin Swimmable {
  void swim() => print('Swimming...');
}

class Duck with Flyable, Swimmable {
  // 同时拥有飞行和游泳能力
}

void main() {
  var duck = Duck();
  duck.fly();  // 输出: Flying...
  duck.swim(); // 输出: Swimming...


// Mixin 不能定义构造函数
  var fly = Flyable(); // ❌ 编译错误
}
```
### 4.4.2 示例2
```dart
// Animal 基类
class Animal {
  void breathe() => print('Breathing...');
}

mixin Flyable {
  void fly() => print('Flying...');
}

mixin Swimmable {
  void swim() => print('Swimming...');
}

class Duck extends Animal with Flyable, Swimmable {

}

void main() {
  var duck = Duck();
  duck.fly();  // 输出: Flying...
  duck.swim(); // 输出: Swimming...
  duck.breathe(); // 输出: Breathing...
}
```

---
## **4.5 初始化列表**
初始化列表以冒号 `:` 开头，用于在对象创建时对属性进行初始化。初始化列表位于主构造函数的参数列表之后，函数体之前。

```
 主构造函数(参数列表) : 初始化列表之 A = a, 初始化列表之 B = b, 初始化列表之 C = c {
    函数体 // 常常省略函数体
 };
```

### 4.5.1 使用初始化列表设置初始值（含逻辑）
```dart
class Student {
  String name;
  int score;
  String grade;

  // 初始化列表中根据 score 设置 grade
  Student(this.name, this.score) : grade = score >= 90 ? 'A' : 'B';

  void showInfo() {
    print('$name got score $score, grade $grade');
  }
}

void main() {
  var s = Student('Tom', 92);
  s.showInfo(); // 输出：Tom got score 92, grade A
}
```
### 4.5.2 初始化 `final` 字段和参数校验
```dart
class Point {
  final int x;
  final int y;
  
  // 初始化列表
  Point(this.x, this.y)
      : assert(x >= 0 && y >= 0, 'Coordinates must be positive');
}

void main() {
  var p = Point(1, 2); // 正常
  // var p = Point(-1, 2); // 抛出异常
}
```
### 4.5.3 初始化父类构造函数
当子类继承父类时，子类的构造函数可以通过初始化列表显式调用父类的构造函数。
```dart
class Animal {
  final String name;

  // 父类构造函数
  Animal(this.name);
}

class Dog extends Animal {
  final String breed;

  // 子类构造函数，初始化父类构造函数，并添加自己的初始化逻辑
  Dog(String name, this.breed) : super(name);

  void bark() {
    print('Woof! My name is $name and I am a $breed.');
  }
}

void main() {
  var dog = Dog('Buddy', 'Golden Retriever');
  dog.bark();  // 输出: Woof! My name is Buddy and I am a Golden Retriever.
}
```
## **4.6 再提几点**
### **4.6.1 命名规范**

在编程中，良好的命名规范能够大大提高代码的可读性和可维护性。Dart 语言遵循一套统一的命名风格规范，下面逐一说明：

#### 4.6.1.1 变量名/方法名：`camelCase`

* **不能以阿拉伯数字开头**，例如 `1stValue` 是非法的，应该使用 `firstValue`。
* 首个单词小写，后续单词首字母大写。不使用下划线。
* 适用于函数、方法、普通变量、参数、局部变量等。

**示例：**

```dart
int calculateTotal(int quantity, int price) {
  int totalPrice = quantity * price;
  return totalPrice;
}
```

#### 4.6.1.2 类名：`PascalCase`

* 同样，**类名也不能以阿拉伯数字开头**，如 `3DModel` 应改为 `ThreeDModel`。
* 首个单词首字母大写，后续单词首字母大写。不使用下划线。
* 通常用于类（`class`）、枚举（`enum`）、扩展（`extension`）等类型名称。

**示例：**

```dart
class UserAccount {}
mixin SweetBaby {}
enum OrderStatus { Pending, Completed, Cancelled }
extension StringUtils on String {}
```

#### 4.6.1.3 补充建议

* **命名不得以阿拉伯数字开头**：无论是变量名、方法名、类名还是文件名，均不能以阿拉伯数字作为首字符。
* **私有字段**：使用下划线 `_` 开头表示私有，例如 `_counter`、`_sweetBaby`。
* **文件名/库名**：使用小写字母和下划线分隔，例如：`user_account.dart`。
* **避免缩写**：尽量使用清晰的全称，例如 `calculateTotal` 优于 `calcTot`。



### **4.6.2 空安全处理**
 ```dart
   String? nullableString;
   print(nullableString?.length ?? 0); // 安全访问
 ```

### **4.6.3 不可变设计**
 ```dart
   class ImmutablePoint {
     final double x;
     const ImmutablePoint(this.x);
   }
 ```

### **4.6.4 方法参数设计**
   - 必需参数放在前面
   - 可选参数用 `{}` 或 `[]` 包裹
   - 避免过多可选参数（超过3个考虑使用对象封装）

## **4.7 主构造函数** [选学]
```dart
class ImmutablePoint {
  final int x;
  final int y;
  
  // 主构造函数
  const ImmutablePoint(this.x, this.y);
  
  // 静态常量实例
  static const zero = ImmutablePoint(0, 0);
}

void main() {
  var p1 = const ImmutablePoint(1, 2); // 编译时常量
  var p2 = ImmutablePoint.zero;        // 使用静态常量
}
```

---

## **4.8 抽象类与接口** [选学]
### **4.8.1 抽象类**
```dart
abstract class Shape {
  // 抽象方法（无实现）
  double getArea();
  
  // 具体方法
  void describe() => print('This is a shape');
}

class Rectangle implements Shape {
  final double width;
  final double height;
  
  Rectangle(this.width, this.height);
  
  @override
  double getArea() => width * height;
}

void main() {
  var rect = Rectangle(3, 4);
  print(rect.getArea()); // 输出: 12
}
```

### **4.8.2 接口实现**
```dart
class Printer {
  void printDocument(String content) => 
      print('Printing: $content');
}

class MultiFunctionDevice implements Printer {
  @override
  void printDocument(String content) {
    // 实现接口方法
    print('Advanced printing: $content');
  }
  
  // 可添加额外功能
  void scan() => print('Scanning...');
}
```

---

## **4.9 方法与运算符重载** [选学]
### **4.9.1 方法重载（Dart风格）**
```dart
class Calculator {
  // 方法1：两数相加
  int add(int a, int b) => a + b;
  
  // 方法2：三数相加（通过可选参数实现）
  int add(int a, int b, [int c = 0]) => a + b + c;
}

void main() {
  var calc = Calculator();
  print(calc.add(1, 2));    // 输出: 3
  print(calc.add(1, 2, 3)); // 输出: 6
}
```

### **4.9.2 运算符重载**
```dart
class Vector {
  final double x;
  final double y;
  
  Vector(this.x, this.y);
  
  // 重载加法运算符
  Vector operator +(Vector other) => 
      Vector(x + other.x, y + other.y);
      
  // 重载相等运算符
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is Vector && 
           x == other.x && 
           y == other.y;
  }
  
  @override
  int get hashCode => Object.hash(x, y);
}

void main() {
  var v1 = Vector(1, 2);
  var v2 = Vector(3, 4);
  print(v1 + v2); // 输出: Vector(4, 6)
}
```

---

## **4.10 高级特性** 
### **4.10.1 工厂构造函数** [选学]
```dart
class Logger {
  final String name;
  
  // 私有构造函数
  Logger._internal(this.name);
  
  // 工厂构造函数
  factory Logger(String name) {
    if (name.isEmpty) {
      return Logger._internal('Default');
    }
    return Logger._internal(name);
  }
}

void main() {
  var logger = Logger('');
  print(logger.name); // 输出: Default
}
```

### **4.10.2 枚举类型**
```dart
enum Color { red, green, blue } // 创建枚举成员

void main() {
  var color = Color.red;
  
  // 获取所有枚举成员的值
  print(Color.values); 
  
  // 获取枚举成员的名称和索引
  print(color.name);   // 输出: red
  print(color.index);  // 输出: 0
  
  // 模式匹配
  switch (color) {
    case Color.red:
      print('Stop');
      break;
    case Color.green:
      print('Go');
      break;
  }
}
```

### **4.10.3 `static`**


在 Flutter（基于 Dart）开发中，使用 `static` 关键字可以优化性能、减少内存占用，并实现全局共享的配置、常量、工具方法等

>为什么在 Flutter 中需要使用 `static`？

- Flutter 的 UI 是由大量 **Widget** 组成的树，每次 rebuild（重建）时都会创建新实例。当你想多次直接调用某个类的属性或方法，却又不想多次将其实例化的时候（创建的对象越多，占用的内存越多，性能也会被随之拉低），使用 `static` 添加静态成员，会是一个很棒的选择。
- 我只想告诉你，调用类中的属性或方法，并不只有 “先将类实例化为一个对象，然后再链式操作调用” 这一条路径。你还可以直接把类当作对象来访问、调用其中的静态成员（前提是使用 `static` ）。


#### 4.10.3.1 静态常量（最常用：颜色、尺寸、文本样式）

```dart
// app_constants.dart
import 'package:flutter/material.dart';

class AppConstants {
  // 推荐使用 static const
  // 声明静态成员
  static const String appName = '我的 Flutter App';
  static const double defaultPadding = 16.0;
  static const Color primaryColor = Colors.blue;
  static const Color backgroundColor = Color(0xFFF5F5F5);

  // 文本样式（常与 Theme 结合使用）
  static const TextStyle headlineStyle = TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.black87,
  );
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppConstants.backgroundColor,  // 直接调用
    // ❌ 而不是 AppConstants().backgroundColor
      appBar: AppBar(       
        title: const Text(AppConstants.appName),  // 直接调用
    // ❌ 而不是 AppConstants().appName
      ),
      body: Center(
        child: Text(
          '欢迎使用',
          style: AppConstants.headlineStyle,  // 直接调用
    // ❌ 而不是 AppConstants().headlineStyle
        ),
      ),
    );
  }
}

// 相比 “先创建对象，再访问属性” ，少创建了3个对象，性能嘎嘎上升

```

**优势**：整个 App 任何地方都可以直接访问，避免多次创建对象，结合 `const` Widget 性能更好。

#### 4.10.3.2 静态方法（工具类）

在 Flutter 开发中，很多常用的功能不需要创建对象就能使用，比如计算年龄、判断是否是周末、显示提示消息等。这些功能最适合写成 `static` 方法，放在工具类里，随时随地直接调用。

```dart
// app_utils.dart

class AppUtils {

  // 静态方法：计算年龄（生活常用）
  static int calculateAge(DateTime birthDate) {
    return DateTime.now().year - birthDate.year;
  }

  // 静态方法：判断今天是不是周末
  static bool isWeekend() {
    final weekday = DateTime.now().weekday;
    return weekday == 6 || weekday == 7;  // 6=周六，7=周日
  }

  // 静态方法：显示一个简单的问候语
  static String getGreeting() {
    final hour = DateTime.now().hour;
    if (hour < 6) return "这么早，晚安～";
    if (hour < 12) return "早上好！";
    if (hour < 14) return "中午好！";
    if (hour < 18) return "下午好！";
    return "晚上好！";
  }

  // 静态方法：判断输入的手机号是否看起来合理（简单版）
  static bool isValidPhone(String phone) {
    return phone.length == 11 && phone.startsWith("1");
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    final birthDate = DateTime(1995, 6, 15);

    return Scaffold(
      appBar: AppBar(
        title: const Text('生活小工具'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('你的年龄是：${AppUtils.calculateAge(birthDate)} 岁'), // 直接调用
    // ❌ 而不是 AppUtils().calculateAge(birthDate)

            const SizedBox(height: 16),

            Text('今天是周末吗？ ${AppUtils.isWeekend() ? "是" : "不是"}'), // 直接调用
    // ❌ 而不是 AppUtils().isWeekend()

            const SizedBox(height: 16),

            Text(AppUtils.getGreeting(), style: const TextStyle(fontSize: 20)), // 直接调用
    // ❌ 而不是 AppUtils().getGreeting()

            const SizedBox(height: 16),

            Text('手机号 13812345678 是否合理？ ${AppUtils.isValidPhone("13812345678") ? "合理" : "不合理"}'),  // 直接调用
    // ❌ 而不是 AppUtils().isValidPhone("13812345678")
          ],
        ),
      ),
    );
  }
}

// 相比 “先创建对象，再调取方法” ，少创建了4个对象，性能嘎嘎上升

```

**优势**：
- 不用写 `AppUtils()` 就能直接调用，非常方便。
- 代码简洁干净，适合在 App 的任何页面使用。
- 把常用的生活小功能集中放在一个类里，方便以后查找和修改。

**小贴士**：  
把所有这些生活小工具都放在 `AppUtils` 类中，以后想加新功能（比如计算 BMI、转换温度等）也只需要在这个文件里添加就行了。

---

## **4.11 实际应用示例** [选学]
### **4.11.1 数据模型类**
```dart
class User {
  final String id;
  final String name;
  final DateTime? createdAt;
  
  User({
    required this.id,
    required this.name,
    this.createdAt,
  });
  
  // 工厂构造函数（从JSON创建）
  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      createdAt: json['createdAt'] != null 
          ? DateTime.parse(json['createdAt']) 
          : null,
    );
  }
  
  // 转换为JSON
  Map<String, dynamic> toJson() => {
    'id': id,
    'name': name,
    'createdAt': createdAt?.toIso8601String(),
  };
}
```

### **4.11.2 响应式属性**
```dart
class Counter {
  int _value = 0;
  
  int get value => _value;
  
  set value(int newValue) {
    if (_value != newValue) {
      _value = newValue;
      _notifyListeners();
    }
  }
  
  final List<void Function()> _listeners = [];
  
  void addListener(void Function() listener) {
    _listeners.add(listener);
  }
  
  void _notifyListeners() {
    for (var listener in _listeners) {
      listener();
    }
  }
}
```





---

# **5. 异步编程**

## **5.1 为什么要异步编程？**

Dart采用单线程模型，这意味着所有代码都在一个主线程上顺序执行。在这样的模型下，异步编程显得尤为重要，主要体现在以下几个方面：

### 5.1.1 避免程序阻塞
在单线程环境中，如果执行耗时操作（如网络请求、文件读写等）时不采用异步编程，程序会被阻塞，无法响应其他事件。例如，在Flutter应用里，若从服务器获取数据时使用同步方式，界面会被冻结，用户无法进行其他操作，严重影响用户体验。而异步编程允许程序在等待耗时操作完成的同时继续执行其他任务，保证了程序的流畅性和响应性。就像厨师在烹饪一道菜时，利用异步编程可以让他在等待食材煮熟的时间里去准备下一道菜的食材，不会因为一道菜的烹饪而耽误其他工作。

### 5.1.2 提高程序性能
单线程模型下，异步编程能充分利用系统资源，提高程序的整体性能。通过事件循环机制，Dart可以在等待异步操作完成时执行其他任务，避免了资源的浪费。比如在进行网络请求时，使用异步编程可以让程序在等待服务器响应的同时处理其他事务，从而提高了程序的吞吐量和响应速度。以服务器处理多个请求为例，异步编程可以让服务器同时处理多个请求，而不是一个一个地等待每个请求完成后再处理下一个，大大提高了服务器的并发处理能力。

### 5.1.3 简化并发编程
Dart的单线程模型避免了多线程环境下的竞态条件和同步问题，开发者无需担心复杂的锁机制和线程安全问题。异步编程通过事件循环和队列机制，以一种简单而有序的方式处理并发任务。开发者只需要关注任务的执行顺序和依赖关系，而不需要手动管理线程的创建、销毁和同步，降低了开发难度和出错的可能性。例如，在Java等多线程语言中，开发者需要使用锁来保证线程安全，而在Dart中，由于是单线程模型，这些问题都得到了简化。

### 5.1.4 保证UI响应性
在UI框架如Flutter中，单线程模型有助于保持UI更新的简洁性和一致性。所有的UI更新操作都在主线程上执行，异步编程可以确保在执行耗时操作时不会阻塞UI线程，从而保证界面的流畅响应。例如，当用户在Flutter应用中进行滑动、点击等操作时，异步编程可以让程序在后台处理数据，而不会影响界面的交互体验。如果使用同步编程，界面可能会因为耗时操作而卡顿，给用户带来不好的体验。

综上所述，尽管Dart是单线程模型，但异步编程在其中起着至关重要的作用，它可以让程序在单线程环境下实现高效的并发处理，提高程序的性能和响应能力，同时简化开发过程。

---


## **5.2 Future 基础**
### 5.2.1 创建 Future
####  **1. `Future.value`**
用于创建已完成的 Future，就像提前知道结果一样。
```dart
Future<String> fetchUserName() {
  // 模拟从服务器获取用户名（但其实是立即完成）
  return Future.value("👤 张小猫");
}

Future<void> main() async {
  print("🔍 正在获取用户信息...");

  try {
    String userName = await fetchUserName();
    print("✅ 用户信息获取成功: $userName");
  } catch (e) {
    print("❌ 获取失败: $e");
  } finally {
    print("📦 操作结束");
  }
}
/* 输出结果：
🔍 正在获取用户信息...
✅ 用户信息获取成功: 👤 张小猫
📦 操作结束
*/
```
####  **2. `Future.delayed`**
模拟延迟操作，比如网络请求的延迟响应。
```dart
Future<void> cookNoodles() async {
  print("👨‍🍳 开始煮泡面...");

  await Future.delayed(Duration(seconds: 1));
  print("🥢 第 1 分钟：加入调料包...");

  await Future.delayed(Duration(seconds: 1));
  print("🥢 第 2 分钟：面条开始变软...");

  await Future.delayed(Duration(seconds: 1));
  print("🥢 第 3 分钟：香气扑鼻～");

  print("✅ 泡面完成！开吃啦！🍜");
}

void main() {
  cookNoodles();
  print("⌛ 等泡面时刷刷短视频...");
}
/* 输出结果：
👨‍🍳 开始煮泡面...
⌛ 等泡面时刷刷短视频...
🥢 第 1 分钟：加入调料包...
🥢 第 2 分钟：面条开始变软...
🥢 第 3 分钟：香气扑鼻～
✅ 泡面完成！开吃啦！🍜
*/
```

### 5.2.2 处理 Future（async/await）
---
#### 1. 异步函数声明
- 在函数前加 `async` 关键字，表示该函数是异步的，会返回一个 `Future` 对象。
- 即使函数直接返回普通值，Dart 也会自动将其包装成 `Future`。
```dart
Future<String> fetchData() async {
  return 'Data fetched'; // 自动包装成 Future<String>
}
```

---

#### 2. 调用异步函数
- **不使用 `await`**：函数立即返回 `Future`，不等待异步完成。
```dart
void main() {
  Future<String> futureData = fetchData(); // 立即返回 Future
  print('Function called, waiting for data...');
}
```
- **使用 `await`**：必须在 `async` 函数内使用，会暂停执行直到 `Future` 完成。
```dart
Future<void> processData() async {
  print('Starting data processing...');
  String data = await fetchData(); // 暂停，等待数据返回
  print('Data received: $data');
}
```

---

#### 3. 执行流程
1. **启动异步函数**  
   调用 `processData()` 时，`main` 函数会立即继续执行后续代码（如果有），而 `processData` 内部开始执行同步代码。

2. **遇到 `await` 时暂停**  
   当执行到 `await fetchData()` 时，`processData` 会挂起（暂停），并将控制权交还给调用者（如 `main`）。此时：
   - `fetchData()` 开始执行其异步操作（如网络请求）。
   - `processData` 不会继续执行后续代码，直到 `fetchData()` 完成。

3. **异步操作完成后恢复**  
   当 `fetchData()` 的 `Future` 完成（返回结果或抛出异常），`processData` 会从暂停处恢复执行，继续处理返回的数据。

---

#### 4. 错误处理
- 使用 `try-catch` 捕获 `await` 可能抛出的异常：
```dart
Future<void> fetchDataWithErrorHandling() async {
  try {
    String data = await fetchData(); // 若 fetchData 抛出异常，进入 catch
    print('Data received: $data');
  } catch (e) {
    print('Error occurred: $e');
  }
}
```

---

#### 5. 完整示例
```dart
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2)); // 模拟耗时操作
  return 'Hello, Dart!';
}

Future<void> processData() async {
  print('1. 开始处理数据...');
  try {
    String data = await fetchData(); // 暂停，等待 2 秒
    print('2. 收到数据: $data');     // 2 秒后继续执行
  } catch (e) {
    print('Error: $e');
  }
  print('3. 数据处理完成。');
}

void main() async {
  print('A. 程序启动');
  await processData(); // 主线程在此暂停，直到 processData 完成
  print('B. 程序结束');
}
```

**输出结果**：
```
A. 程序启动
1. 开始处理数据...
（等待 2 秒）
2. 收到数据: Hello, Dart!
3. 数据处理完成。
B. 程序结束
```

---

#### 关键点总结
- **同步风格**：`async - await` 让异步代码看起来像同步代码，逻辑更清晰。
- **顺序执行**：`await` 会按顺序等待每个异步操作完成，适合需要严格顺序的场景。
- **错误捕获**：通过 `try-catch` 处理异步异常，避免程序崩溃。
- **非阻塞**：未使用 `await` 时，函数会立即返回 `Future`，不阻塞调用者。

通过这种机制，Dart 的异步编程既保持了代码的可读性，又能高效处理并发任务。

### 5.2.3 链式调用
按顺序执行多个异步操作，就像依次完成多个任务。
```dart
Future<void> processUserData() async {
  final user = await fetchUser();
  final posts = await fetchPosts(user.id);
  await savePosts(posts);
}
```

以下是几个生动有趣且简单的 Dart `async - await` 示例：

#### 1. 模拟太空探险任务
```dart
import 'dart:async';

// 模拟发射火箭的异步操作
Future<void> launchRocket() async {
  print('正在准备发射火箭...');
  await Future.delayed(Duration(seconds: 3));
  print('火箭发射成功！');
}

// 模拟在太空中探索的异步操作
Future<String> exploreSpace() async {
  print('宇航员已进入太空...');
  await Future.delayed(Duration(seconds: 5));
  print('探索完成，发现新星球！');
  return '新星球数据';
}

// 主函数，模拟整个太空探险任务
void main() async {
  print('太空探险任务开始！');
  await launchRocket();
  String data = await exploreSpace();
  print('带回的数据：$data');
  print('太空探险任务结束！');
}
```
在这个例子中，`launchRocket` 函数模拟了发射火箭的过程，需要 3 秒钟的准备时间。`exploreSpace` 函数模拟了宇航员在太空中探索的过程，需要 5 秒钟。`main` 函数按顺序执行这两个异步操作，就像太空探险任务一样，必须先发射火箭，才能进行太空探索。

#### 2. 模拟餐厅点餐流程
```dart
import 'dart:async';

// 模拟顾客点餐的异步操作
Future<String> placeOrder() async {
  print('顾客开始点餐...');
  await Future.delayed(Duration(seconds: 2));
  print('点餐完成，订单已提交！');
  return '订单号：12345';
}

// 模拟厨房准备食物的异步操作
Future<String> prepareFood(String orderId) async {
  print('厨房开始根据订单 $orderId 准备食物...');
  await Future.delayed(Duration(seconds: 4));
  print('食物准备完成！');
  return '美食大餐';
}

// 模拟服务员上菜的异步操作
Future<void> serveFood(String food) async {
  print('服务员正在上菜：$food');
  await Future.delayed(Duration(seconds: 2));
  print('菜已上桌！');
}

// 主函数，模拟整个餐厅点餐流程
void main() async {
  print('餐厅营业开始！');
  String orderId = await placeOrder();
  String food = await prepareFood(orderId);
  await serveFood(food);
  print('餐厅营业结束！');
}
```
这里，`placeOrder` 函数模拟顾客点餐，需要 2 秒钟。`prepareFood` 函数根据订单号准备食物，需要 4 秒钟。`serveFood` 函数模拟服务员上菜，需要 2 秒钟。`main` 函数按顺序执行这些操作，就像餐厅里顾客点餐、厨房准备食物、服务员上菜的流程一样。

#### 3. 模拟下载和安装游戏
```dart
import 'dart:async';

// 模拟下载游戏的异步操作
Future<void> downloadGame() async {
  print('开始下载游戏...');
  await Future.delayed(Duration(seconds: 6));
  print('游戏下载完成！');
}

// 模拟安装游戏的异步操作
Future<void> installGame() async {
  print('开始安装游戏...');
  await Future.delayed(Duration(seconds: 4));
  print('游戏安装完成！');
}

// 主函数，模拟整个下载和安装游戏的过程
void main() async {
  print('游戏获取过程开始！');
  await downloadGame();
  await installGame();
  print('游戏获取过程结束，可以开始玩游戏啦！');
}
```
在这个示例中，`downloadGame` 函数模拟游戏下载，需要 6 秒钟。`installGame` 函数模拟游戏安装，需要 4 秒钟。`main` 函数按顺序执行这两个操作，就像我们获取游戏时先下载再安装的过程。

## **5.3 Stream 基础**

### （略）

## **5.4 异常处理**
### 5.4.1 Future 异常
使用 `try - catch` 捕获 `Future` 中的异常，避免程序崩溃。
```dart
Future<void> riskyOperation() async => throw Exception('Something went wrong');

void main() {
  riskyOperation().catchError((error) => print('Caught error: $error'));
}
```

## **5.5 使用规范**
- **优先使用 `async - await`**：代码更简洁易读，避免回调地狱。
- **错误处理**：始终用 `try - catch` 包裹异步代码，防止程序崩溃。
---

# **6. 异常处理**

## **6.1 认识"异常处理"**
在Dart编程里，异常处理是非常重要的，它能保障程序的稳定、可靠运行。以下从异常处理的原因、定义、优势、类型、处理机制几方面详细介绍：

### 6.1.1 异常处理的原因
程序在运行过程中，难免会遇到各种意外情况，如文件操作时可能遇到权限问题或文件不存在，网络请求时可能因网络不稳定或服务器错误导致失败，数据库交互时可能出现连接失败或查询错误等。若不进行异常处理，这些意外情况会引发程序崩溃，给用户带来不好的体验，还可能导致数据丢失或程序状态异常。异常处理可以捕获并处理这些意外情况，避免程序因未预料的错误而终止，增强程序的健壮性和可靠性。

### 6.1.2 异常的定义
异常是指程序执行过程中出现的错误或意外情况，它会打断程序的正常流程。在Dart中，异常是一个事件，当程序遇到异常时，后续代码将不会继续执行，除非该异常被捕获并处理。

### 6.1.3 异常处理的优势
- **提高程序稳定性**：通过捕获和处理异常，可防止程序因未预料的错误而崩溃，保证程序在遇到问题时仍能继续运行或优雅地退出。
- **增强错误报告**：能提供详细的错误信息，帮助开发者快速定位和修复问题，提高开发效率。
- **资源管理**：使用`finally`块可确保即使在发生异常的情况下，关键资源（如文件、数据库连接、网络连接等）也能得到适当的释放，避免资源泄漏。
- **错误日志记录**：便于查找程序出错原因，方便后续的调试和维护。

### 6.1.4 异常类型

Dart 中的异常主要分为两类：
- **Error**：指程序中发生的严重错误，这类错误通常是不可恢复的，程序会立即终止。例如内存不足引发的错误。
- **`Exception`**：指程序中可以预见的异常，这类错误是可进行恢复的。例如网络中断导致的异常。通常会通过 `throw` 主动抛出，结合 `try-catch` 来处理。

#### 6.1.4.1 常见的 Error 类型：
- **`ArgumentError`**   
  当调用函数时传入了非法参数时抛出。**虽然它继承自 `Error`，但更接近 `Exception`，因为它通常在函数内部通过 `throw` 主动抛出，并且可以通过 `try-catch` 捕获**。

  示例：
```dart
  void greet(String name) {
    if (name.isEmpty) {
      throw ArgumentError("名字不能为空");
    }
    print("你好，$name");
  }

  void main() {
    try {
      greet(""); // 抛出 ArgumentError
    } catch (e) {
      print("捕获到异常: $e");
    }
  }
```

输出：

```markdown
捕获到异常: Invalid argument(s): 名字不能为空
```

- **`AssertionError`**
  当断言失败时抛出，通常用于开发阶段的调试用途。在使用 `assert()` 时，如果表达式为 false，则会抛出该错误。**它主要用于开发阶段发现逻辑错误，在生产环境中断言默认是被禁用的。**

  示例：

```dart
  void checkAge(int age) {
    assert(age >= 0, "年龄不能为负数");
    print("年龄是: $age");
  }

  void main() {
    try {
      checkAge(-5); // 抛出 AssertionError（仅在调试模式下）
    } catch (e) {
      print("捕获到异常: $e");
    }
  }
```

  输出（仅在调试模式下）：

```
  捕获到异常: Assertion failed: "年龄不能为负数"
```

#### 6.1.4.2 `Exception` 示例代码：

```dart
void divideNumbers(int a, int b) {
  try {
    if (b == 0) {
      throw Exception("除数不能为 0");
    }
    int result = a ~/ b; // 整除运算
    print("结果是: $result");
  } catch (e) {
    print("捕获到异常: $e");
  } finally {
    print("运算结束，无论成功与否我都会执行");
  }
}

void main() {
  divideNumbers(10, 2); // 正常情况
  print("<分隔线>");
  divideNumbers(10, 0); // 会触发异常
}
```

输出结果：

```markdown
结果是: 5
运算结束，无论成功与否我都会执行

<分隔线>

捕获到异常: Exception: 除数不能为 0
运算结束，无论成功与否我都会执行
```
#### 6.1.4.3 对比：

| 类型                 | 继承自    | 用途                           | 是否通常通过 try-catch 捕获 |
| ------------------ | ------ | ---------------------------- | ------------------- |
| **Error**          | Object | 程序严重错误，通常不可恢复，程序会终止。         | 否                   |
| **Exception**      | Object | 程序可恢复的错误，可以通过 try-catch 捕获   | 是                   |
| **ArgumentError**  | Error  | 函数参数非法时抛出，通常可以恢复。            | 是                   |
| **AssertionError** | Error  | 断言失败时抛出，用于开发阶段调试逻辑错误，非运行时异常。 | 否（调试模式下可捕获）         |

**总结**：

* **Error** 通常用于不可恢复的系统错误，开发者不应在运行时捕获。
* **Exception** 用于可预见并可以恢复的错误，开发者应通过 try-catch 捕获。
* **ArgumentError** 虽然继承自 Error，但它是可预见的、可恢复的异常，实际应用中经常通过 try-catch 捕获处理。
* **AssertionError** 多用于调试阶段的逻辑断言，能帮助开发者尽早发现错误，生产环境中默认不会触发。

### 6.1.5 **try-catch-finally**
Dart使用`try-catch-finally`结构来实现异常处理：
- **try块**：包含可能会抛出异常的代码。如果`try`块中的代码抛出异常，控制流会立即跳转到相应的`catch`块。
- **catch块**：用于捕获并处理异常。可以指定要捕获的异常类型，若不指定则捕获所有类型的异常。`catch`块可以接收一个参数（通常命名为`e`），该参数是捕获到的异常对象，可在其中记录日志、提示用户、执行回滚操作等。
- **finally块（可选）**：无论是否发生异常，`finally`块中的代码都会执行，常用于释放资源、关闭文件或网络连接等清理操作。

#### 6.1.5.1 示例1 分母不为0
```dart
void main() {
  try {
    // 可能会抛出异常的代码
    int result = 12 ~/ 0; 
  } catch (e) {
    // 捕获异常并处理
    print('发生了一个错误: $e'); 
  } finally {
    // 无论是否发生异常，这里的代码都会执行
    print('异常处理结束'); 
  }
}
```

在上述代码中，`try`块里的除法操作会抛出除以零的异常，该异常被`catch`块捕获并输出错误信息，最后`finally`块中的代码无论异常是否发生都会执行。

#### 6.1.5.2 示例2 喂猫系统
```dart
void feedCat(String food) {
  try {
    print("👦 拿出了一些 $food 准备喂小猫...");

    if (food != '鱼' && food != '猫粮' && food != '鸡肉') {
      throw Exception("😾 小猫生气了！它不吃 $food！");
    }

    print("😺 小猫开心地吃着 $food～");
  } catch (e) {
    print("💥 喂食失败: $e");
  } finally {
    print("📦 喂猫任务结束，收拾碗筷。\n");
  }
}

void main() {
  feedCat("鱼");        // ✅ 小猫开心
  feedCat("辣条");      // ❌ 小猫生气
  feedCat("猫粮");      // ✅ 小猫开心
}
```
输出结果：
```markdown
👦 拿出了一些 鱼 准备喂小猫...
😺 小猫开心地吃着 鱼～
📦 喂猫任务结束，收拾碗筷。

👦 拿出了一些 辣条 准备喂小猫...
💥 喂食失败: Exception: 😾 小猫生气了！它不吃 辣条！
📦 喂猫任务结束，收拾碗筷。

👦 拿出了一些 猫粮 准备喂小猫...
😺 小猫开心地吃着 猫粮～
📦 喂猫任务结束，收拾碗筷。
```

### 6.1.6 **`ArgumentError`** 和 **`Exception`** 的区别
让我们通过一些不能互换的例子，来说明 **`ArgumentError`** 和 **`Exception`** 的不同之处。

#### 例子 1：使用 `ArgumentError` —— 参数错误

`ArgumentError` 用于传入无效参数的情况。当你传递给函数的参数不符合要求时，应该使用 `ArgumentError`，它专门用于标明“你提供的参数无效”。

##### 场景：

你编写一个函数，要求传入的年龄必须是正数。如果传入负数或零，就抛出 `ArgumentError`。

```dart
void checkAge(int age) {
  if (age <= 0) {
    throw ArgumentError("年龄必须大于零！");
  }
  print("年龄是 $age 岁！");
}

void main() {
  try {
    checkAge(0);  // 触发 ArgumentError
  } catch (e) {
    print("捕获到异常: $e");
  }
}
```

##### 输出：

```markdown
捕获到异常: Invalid argument(s): 年龄必须大于零！
```

##### 为什么不用 `Exception` ？

* **`ArgumentError`** 明确表示这是一个关于参数的错误，告诉开发者传入的值无效。
* 如果用 **`Exception`** 替换，就失去了这个明确的语义，虽然程序仍然会抛出错误，但不会表明是因为参数无效导致的问题。

---

#### 例子 2：使用 `Exception` —— 可恢复的错误

`Exception` 用于表示程序运行中的错误，这些错误通常是可以处理并恢复的，比如网络请求失败、文件读取失败等。这些错误不一定是程序本身的 bug，而是可以通过一些逻辑进行恢复的。

##### 场景：

你编写一个函数，从网络请求数据。如果网络不可用，就抛出 `Exception`，而不是 `ArgumentError`，因为这是一个可恢复的错误。

```dart
void fetchDataFromNetwork() {
  bool isNetworkAvailable = false;  // 模拟网络不可用

  if (!isNetworkAvailable) {
    throw Exception("无法连接到网络！");
  }

  print("数据获取成功！");
}

void main() {
  try {
    fetchDataFromNetwork();  // 触发 Exception
  } catch (e) {
    print("捕获到异常: $e");
  }
}
```

##### 输出：

```markdown
捕获到异常: Exception: 无法连接到网络！
```

##### 为什么不用 `ArgumentError` ？

* **`Exception`** 是用来处理程序中可以预见并恢复的错误，比如网络错误。它的语义明确表明这是一个可以通过重试或其他方式解决的问题。
* 如果用 **`ArgumentError`** 替换，就会误导开发者，认为错误是由无效参数引起的，而实际上是网络连接问题，不能用相同的处理方式。

---

#### 关键总结：

* **`ArgumentError`** 用于无效参数的错误，通常是在函数或方法中检查传入值是否合法时使用。**不能用 `Exception` 替换**，因为 `Exception` 没有明确的参数错误含义。
* **`Exception`** 用于处理可以预见并恢复的错误，通常在处理外部资源（如网络、文件等）时使用。**不能用 `ArgumentError` 替换**，因为 `ArgumentError` 是用于无效参数，而不是外部错误。

这两个异常类型的使用场景是有明显区别的，并不能互换！

---

## **6.2 实际应用示例** [选学]
### **6.2.1 表单验证**
```dart
class FormValidator {
  static String? validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return '邮箱不能为空';
    }
    if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
      return '邮箱格式不正确';
    }
    return null; // 无错误
  }
}

// 使用
final error = FormValidator.validateEmail('invalid-email');
if (error != null) {
  showError(error); // 自定义错误处理
}
```

### **6.2.2 数据库操作**
```dart
Future<void> saveUser(User user) async {
  try {
    await db.insert('users', user.toMap());
  } on SqliteException catch (e) {
    if (e.message.contains('UNIQUE constraint failed')) {
      throw UserAlreadyExistsException(user.id);
    }
    rethrow;
  }
}
```

### **6.2.3 API 调用封装**
```dart
Future<T> apiCall<T>(Future<T> Function() operation) async {
  try {
    return await operation();
  } on SocketException {
    throw NetworkException('网络连接失败');
  } on FormatException {
    throw ApiException('数据解析失败');
  } catch (e) {
    throw ApiError('请求失败: ${e.toString()}');
  }
}

// 使用
try {
  final user = await apiCall(() => fetchUser(1));
} on UserAlreadyExistsException {
  // 特定错误处理
} on NetworkException {
  // 网络错误处理
} catch (e) {
  // 其他错误处理
}
```

---

## **6.3 注意事项** [选学]
1. **不要捕获所有异常而不处理**  
   ❌ 错误示例：
 ```dart
   try { /* ... */ } catch (e) {} // 隐藏错误！
 ```
   ✅ 正确做法：
 ```dart
   try { /* ... */ } catch (e) {
     logError(e); // 至少记录错误
     rethrow; // 或适当处理
   }
 ```

2. **避免过度捕获**  
   只捕获你能处理的异常，让其他异常向上传播。

3. **异步代码中的异常传播**  
 ```dart
   // 错误：未处理的 Future 异常会导致应用崩溃
   fetchData(); // 未 await 或 .catchError()

   // 正确
   await fetchData().catchError((e) => print(e));
 ```

4. **Flutter 中的错误边界**  
   在 Flutter 中，使用 `runZonedGuarded` 捕获未处理的异常：
 ```dart
   void main() {
     runZonedGuarded(() {
       runApp(MyApp());
     }, (error, stack) {
       reportError(error, stack); // 上报错误
     });
   }
 ```

---

# **7. 核心库与工具** [选学]
- **标准库**：`dart:core`（基础功能）、`dart:io`（文件/网络）、`dart:async`（异步）。
- **包管理**：通过 `pubspec.yaml` 引入第三方库（如 `http`、`flutter`）。
- **文档工具**：`dartdoc` 生成 API 文档。

---

# **8. 高级特性** [选学]

## **8.1 枚举（Enum）**
### **8.1.1 基础用法**
```dart
enum Color { red, green, blue } // 创建枚举成员

void main() {
  Color c = Color.red;
  print(c); // 输出: Color.red
  print(c.index); // 输出: 0（枚举值的索引）
}
```

### **8.1.2 增强型枚举（Dart 2.17+）**
```dart
enum Planet {
  mercury(3.303e+23, 2.4397e6),
  venus(4.869e+24, 6.0518e6),
  earth(5.976e+24, 6.37814e6);

  final double mass; // 质量（千克）
  final double radius; // 半径（米）

  const Planet(this.mass, this.radius);

  // 计算表面重力
  double surfaceGravity() => 
      6.67300E-11 * mass / (radius * radius);
}

void main() {
  print(Planet.earth.surfaceGravity()); // 输出: 9.81962...
}
```

### **8.1.3 枚举实用方法**
```dart
enum Status { pending, processing, completed }

void main() {
  // 获取所有枚举值
  print(Status.values); 
  
  // 通过名称查找
  print(Status.values.firstWhere(
    (e) => e.name == 'completed',
    orElse: () => Status.pending
  ));
  
  // 切换状态
  Status current = Status.pending;
  current = Status.values[(current.index + 1) % Status.values.length];
}
```

---

## **8.2 泛型（Generics）**
### **8.2.1 基础泛型**
```dart
// 泛型类
class Box<T> {
  T content;
  Box(this.content);
}

void main() {
  var intBox = Box(42);
  var strBox = Box('Hello');
  
  print(intBox.content.runtimeType); // 输出: int
  print(strBox.content.runtimeType); // 输出: String
}
```

### **8.2.2 泛型约束**
```dart
// 限制T必须是num或其子类
class NumericBox<T extends num> {
  T value;
  NumericBox(this.value);
  
  T add(T other) => value + other;
}

void main() {
  var box = NumericBox(5);
  print(box.add(3)); // 输出: 8
  
  // var invalid = NumericBox('text'); // 编译错误
}
```

### **8.2.3 泛型方法**
```dart
T firstElement<T>(List<T> list) => list.first;

void main() {
  print(firstElement([1, 2, 3])); // 输出: 1
  print(firstElement(['a', 'b'])); // 输出: a
}
```

### **8.2.4 泛型集合**
```dart
void main() {
  // 明确指定类型
  List<int> numbers = [1, 2, 3];
  
  // 类型推断
  var names = ['Alice', 'Bob']; // 推断为 List<String>
  
  // 原始类型（不推荐）
  var rawList = <dynamic>[1, 'two']; // 可包含任意类型
}
```

---

## **8.3 元数据（Metadata）**
### **8.3.1 内置注解**
```dart
// 标记方法已重写
@override
String toString() => 'Custom toString';

// 标记已弃用
@deprecated
void oldMethod() {}

// 条件编译（仅当dart.library.html存在时生效）
@JS()
external void jsFunction();

// 自定义注解
class ApiEndpoint {
  final String path;
  const ApiEndpoint(this.path);
}

@ApiEndpoint('/users')
class UserService {}
```

### **8.3.2 自定义注解处理**
```dart
// 定义注解
class Todo {
  final String author;
  final DateTime? dueDate;
  
  const Todo(this.author, {this.dueDate});
}

// 使用注解
@Todo('Alice', dueDate: DateTime(2023, 12, 31))
void implementFeature() {
  // 待实现功能
}

// 反射读取注解（需dart:mirrors，生产环境慎用）
// import 'dart:mirrors';
// 
// void checkAnnotations() {
//   var mirror = reflect(implementFeature) as ClosureMirror;
//   var classMirror = mirror.function.owner as ClassMirror;
//   var annotations = classMirror.metadata;
//   print(annotations); // 输出注解信息
// }
```

### **8.3.3 实际应用**
```dart
// API路由注解
class Route {
  final String path;
  const Route(this.path);
}

@Route('/login')
class LoginPage {}

// 自动生成路由表
List<RouteInfo> generateRoutes() {
  // 实际项目中通过代码生成工具实现
  return [];
}
```

---

## **8.4 反射（Reflection）**
### **8.4.1 基础用法（dart:mirrors）**
```dart
import 'dart:mirrors';

class Person {
  String name = 'Alice';
  int age = 30;
}

void main() {
  var p = Person();
  var mirror = reflect(p);
  
  // 获取类信息
  var classMirror = mirror.type;
  print(classMirror.simpleName); // 输出: Person
  
  // 获取字段
  for (var decl in classMirror.declarations.values) {
    if (decl is VariableMirror && !decl.isStatic) {
      var name = MirrorSystem.getName(decl.simpleName);
      var value = mirror.getField(decl.simpleName).reflectee;
      print('$name = $value');
    }
  }
}
```

### **8.4.2 动态调用方法**
```dart
class Calculator {
  int add(int a, int b) => a + b;
}

void main() {
  var calc = Calculator();
  var mirror = reflect(calc);
  
  // 获取方法
  var method = mirror.type.declarations[#add] as MethodMirror;
  
  // 调用方法
  var result = mirror.invoke(
    method.simpleName,
    [10, 20]
  ).reflectee;
  
  print(result); // 输出: 30
}
```

### **8.4.3 注意事项**
1. **性能影响**：反射比直接调用慢10-100倍
2. **AOT限制**：Flutter的AOT编译会移除反射代码
3. **替代方案**：
 ```dart
   // 使用代码生成而非反射
   // 例如：json_serializable、freezed等
 ```

---

## **8.5 建议与规范**
### **8.5.1 枚举使用建议**
- 优先使用增强型枚举（Dart 2.17+）
- 为枚举值添加有意义的计算属性
- 避免在枚举中存储大量数据

### **8.5.2 泛型设计原则**
- 为泛型参数添加有意义的名称（如`<TElement>`而非`<T>`）
- 合理使用泛型约束
- 避免过度复杂的嵌套泛型

### **8.5.3 元数据规范**
- 为自定义注解添加文档注释
- 使用常量构造函数
- 考虑通过代码生成工具处理注解

### **8.5.4 反射替代方案**
| 需求 | 推荐方案 |
|------|----------|
| 序列化/反序列化 | json_serializable |
| 依赖注入 | get_it, provider |
| 路由 | go_router, auto_route |
| ORM | floor, moor |

---

## **8.6 完整示例**
### **8.6.1 泛型+枚举组合**
```dart
enum Status { success, error }

class ApiResponse<T> {
  final T data;
  final Status status;
  final String? message;
  
  ApiResponse.success(this.data) 
      : status = Status.success,
        message = null;
  
  ApiResponse.error(this.message) 
      : status = Status.error,
        data = null as T;
}

void main() {
  var success = ApiResponse.success(42);
  var error = ApiResponse.error('Failed');
  
  if (success.status == Status.success) {
    print('Got data: ${success.data}');
  }
}
```

### **8.6.2 注解驱动开发**
```dart
// 定义验证注解
class Validate {
  final String rule;
  const Validate(this.rule);
}

// 使用注解
class User {
  @Validate(r'^[a-z]+$')
  String username;
  
  User(this.username);
}

// 模拟注解处理器
void validateUser(User user) {
  // 实际项目中通过代码生成实现
  print('Validating user...');
}
```

