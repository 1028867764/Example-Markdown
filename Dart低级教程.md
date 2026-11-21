# 第一章 Dart 语言基础

## 1.1 Dart 简介

Dart 是一种由 Google 开发的编程语言，主要用于 Flutter 应用程序开发。

Dart 具有以下特点：

- **面向对象**：支持类、对象、继承、封装和多态等特性。
- **静态类型**：变量通常具有明确的类型，可以在编译期间发现许多类型错误。
- **空安全**：通过类型系统区分可空变量和不可空变量，减少空值引发的错误。
- **支持异步编程**：提供 `Future`、`async` 和 `await` 等机制，方便处理网络请求、文件操作等耗时任务。
- **跨平台**：可以编译为原生机器码，也可以编译为 JavaScript 或 WebAssembly 等形式。

Dart 不仅可以用于 Flutter 移动应用开发，还可以用于 Windows、macOS、Linux 桌面应用以及 Web 应用开发。

## 1.2 注释与输出

### 1.2.1 注释

注释是写给开发者看的说明文字，不会作为程序代码执行。

Dart 支持三种注释方式：

```dart
// 单行注释

/*
  多行注释
  可以跨越多行
*/

/// 文档注释
/// 通常用于说明类、方法、函数等
/// 可以被 IDE 识别并生成 API 文档
```

### 1.2.2 输出

使用 `print()` 函数可以在控制台输出内容。

```dart
// 输出字符串
print('Hello Dart');

// 输出数字
print(123);

// 输出变量
String name = '小明';
print(name);

// 使用字符串插值
int age = 20;
print('我今年 $age 岁');

// 表达式也可以放在 ${} 中
print('明年我 ${age + 1} 岁');
```

## 1.3 变量与常量

变量用于存储数据，常量用于存储不允许重新赋值的数据。

### 1.3.1 var 与显式类型声明

Dart 可以使用 `var` 声明变量，也可以直接指定变量类型。

```dart
// 使用 var 声明变量
var name = 'Tom'; // Dart 会根据初始值推断为 String 类型
var age = 20;     // 推断为 int 类型

// 显式声明变量类型
String city = 'Tokyo';
int score = 100;
double price = 19.9;
bool isStudent = true;

// 修改变量的值
age = 21;

// age = 'Hello'; // 错误：不能将 String 赋值给 int 类型变量
```

注意：

- `var` 只是让 Dart 自动推断变量类型，并不代表变量可以随意存储不同类型的数据。
- 变量一旦确定类型，后续就不能赋值为其他不兼容的类型。
- 如果变量声明时没有指定初始值，使用 `var` 声明的变量默认推断为 `dynamic`。

```dart
var value;

value = 100;
value = 'Hello'; // 可以，因为此时 value 是 dynamic 类型
```

另外，Dart 还有一个特殊类型 `dynamic`，允许变量在运行期间存储不同类型的数据。

```dart
dynamic value = 100;

value = 'Hello';
value = true;
```

`dynamic` 会降低类型检查的严格程度，初学者应尽量优先使用明确的类型或 `var`。

### 1.3.2 null 与空安全

在 Dart 中，`null` 表示空值。

Dart 默认不允许普通变量存储 `null`，这项机制称为**空安全（Null Safety）**。

```dart
String name = 'Tom';

// name = null; // 错误：String 类型不允许为 null
```

如果希望变量可以存储 `null`，需要在类型后面添加 `?`。

```dart
String? name = null; // 允许存储 null

name = 'Tom';
name = null;
```

常见操作：

```dart
String? name;

// 使用 ?? 提供默认值
String result = name ?? '未知用户';

// 使用 ?. 安全访问属性或方法
int? length = name?.length;

// 使用 ! 强制将可空类型转换为非空类型
String value = name!; // 如果 name 为 null，会在运行时抛出异常
```

注意：

- `String`：不允许为 `null`。
- `String?`：允许为 `null`。
- `??`：如果左侧为 `null`，则使用右侧的值。
- `?.`：如果对象为 `null`，则不继续访问其属性或方法。
- `!`：告诉 Dart 当前值一定不为 `null`，但如果判断错误，程序可能发生运行时异常。

### 1.3.3 final 与 const

`final` 和 `const` 都用于声明不能重新赋值的变量。

```dart
// final：只能赋值一次
final name = 'Tom';

// const：编译期常量
const age = 20;
```

两者的主要区别在于：

- `final`：只允许赋值一次，可以在运行期间确定值。
- `const`：必须在编译期间就能确定值。

例如：

```dart
// final 可以在运行期间获取值
final currentTime = DateTime.now();

// const 必须在编译期间确定值
const pi = 3.14159;

// const currentTime = DateTime.now(); // 错误：运行期间才能确定当前时间
```

另外，`const` 声明的对象具有编译期常量特性，其值不可修改。

```dart
final numbers = [1, 2, 3];
numbers.add(4); // 可以：允许修改列表内容
// numbers = [5, 6]; // 错误：不能重新赋值

const scores = [1, 2, 3];
// scores.add(4); // 错误：不能修改常量列表
```

简单记忆：

- `final`：不能重新赋值。
- `const`：编译期确定，不能重新赋值，其常量对象也不能修改。

### 1.3.4 late

`late` 用于延迟初始化变量，即声明变量时暂时不赋值，等到后续需要时再进行初始化。

```dart
late String username;

// 后续再赋值
username = 'Tom';

print(username);
```

`late` 也可以配合变量初始化表达式使用，让变量在第一次被访问时才执行初始化。

```dart
late String message = getMessage();

String getMessage() {
  print('执行初始化');
  return 'Hello Dart';
}

void main() {
  print('程序开始');

  print(message); // 第一次访问时才执行初始化
  print(message); // 不会重复初始化
}
```

注意：

- `late` 变量必须在使用之前完成初始化。
- 如果尚未初始化就访问变量，会产生运行时异常。
- `late` 常用于初始化需要依赖其他对象或暂时无法获取的数据。

### 1.3.5 编译期与运行期

程序从编写到执行，通常需要经历编译和运行两个阶段。

**编译期（Compile Time）**

编译期是程序被编译器检查、分析和转换的阶段。

例如：

```dart
int age = '20'; // 编译期类型错误
```

这里试图将 `String` 类型的值赋给 `int` 类型变量，Dart 可以在编译期发现这个错误。

**运行期（Runtime）**

运行期是程序已经开始执行代码的阶段。

例如：

```dart
void main() {
  int a = 10;
  int b = 0;

  print(a ~/ b); // 运行期错误：不能除以 0
}
```

这段代码可以通过语法和类型检查，但执行到除以零的操作时，会产生运行时异常。

**编译期与运行期的区别：**

| 对比   | 编译期            | 运行期       |
| ---- | -------------- | --------- |
| 发生时间 | 程序执行之前         | 程序执行期间    |
| 主要工作 | 语法检查、类型检查、代码编译 | 实际执行程序代码  |
| 常见错误 | 类型错误、语法错误      | 除以零、空值访问等 |

补充：

`const` 要求其值在编译期就能确定，而 `final` 允许在运行期确定值。这也是两者最重要的区别之一。

## 1.4 数据类型

Dart 是一种强类型语言，每个变量都有对应的数据类型。常见的数据类型包括数值、布尔值、字符串、列表、映射和集合等。

### 1.4.1 数值（int、double、num）

Dart 中主要有三种数值类型：

- `int`：整数，例如 `10`、`-5`。
- `double`：浮点数，例如 `3.14`、`-0.5`。
- `num`：数值类型的父类型，可以存储整数或浮点数。

```dart
// 整数
int age = 18;

// 浮点数
double price = 19.99;

// num 可以存储整数或浮点数
num number = 10;
number = 3.14;

print(age);     // 18
print(price);   // 19.99
print(number);  // 3.14

// 常见数学运算
print(10 + 3);  // 13：加法
print(10 - 3);  // 7：减法
print(10 * 3);  // 30：乘法
print(10 / 3);  // 3.333...：除法，结果为 double
print(10 ~/ 3); // 3：整除，舍弃小数部分
print(10 % 3);  // 1：取余
```

注意：

- `int` 和 `double` 都是 `num` 的子类型。
- `/` 除法的结果始终是 `double`，即使能够整除。
- `~/` 表示整除，结果为整数。

### 1.4.2 布尔值（bool）

`bool` 用于表示真假，只有两个值：

- `true`：真。
- `false`：假。

通常用于条件判断和逻辑运算。

```dart
bool isLogin = true;
bool isAdmin = false;

// 比较运算
print(10 > 5);   // true
print(10 == 5);  // false
print(10 != 5);  // true

// 逻辑运算
print(true && false); // false：并且
print(true || false); // true：或者
print(!true);         // false：取反
```

注意：Dart 不会自动将数字或字符串转换成布尔值，条件表达式必须是 `bool` 类型。

### 1.4.3 字符串（String）

`String` 用于存储文本，可以使用单引号或双引号创建。

```dart
String name = '小明';
String message = "你好，Dart！";

// 字符串拼接
print('你好，' + name);

// 字符串插值：使用 $ 插入变量
print('我的名字是 $name');

// 使用 ${} 插入表达式
print('明年我 ${18 + 1} 岁');

// 获取字符串长度
print(name.length); // 2

// 判断是否为空字符串
print(name.isEmpty);    // false
print(name.isNotEmpty); // true

// 多行字符串
String content = '''
这是第一行
这是第二行
这是第三行
''';
```

常见字符串操作：

```dart
String text = ' Hello Dart ';

print(text.trim());          // 去除首尾空格
print(text.toUpperCase());   // 转换为大写
print(text.toLowerCase());   // 转换为小写
print(text.contains('Dart'));// 是否包含指定内容，返回布尔值
print(text.replaceAll('Dart', 'Flutter')); // 替换内容
```

注意：

- 字符串是不可变的，不能直接修改原字符串中的某个字符。
- 字符串插值比使用 `+` 拼接更加方便。
- `$变量名` 用于插入变量，`${表达式}` 用于插入表达式。

### 1.4.4 列表（List）

`List` 是列表，用于存储多个有顺序的数据，类似于 Python 中的列表。

列表中的元素可以通过索引访问，索引从 `0` 开始。

```dart
// 创建列表
// List<列表存储数据的类型>
List<String> fruits = ['苹果', '香蕉', '橘子'];

// 访问元素
print(fruits[0]); // 苹果
print(fruits[1]); // 香蕉

// 修改元素
fruits[0] = '葡萄';

// 添加元素
fruits.add('西瓜');

// 删除元素
fruits.remove('香蕉');

// 获取列表长度
print(fruits.length);

// 判断是否包含某个元素，返回布尔值
print(fruits.contains('橘子'));

// 判断是否为空列表，返回布尔值
print(fruits.isEmpty);
print(fruits.isNotEmpty);

// 遍历列表
for (String fruit in fruits) {
  print(fruit);
}
```

列表常见操作：

```dart
List<int> numbers = [3, 1, 2];

numbers.add(4);       // 添加元素
numbers.insert(0, 0); // 在指定索引插入元素
numbers.remove(2);    // 删除指定元素
numbers.removeAt(0);  // 删除指定索引的元素

numbers.sort();       // 升序排序

numbers = numbers.reversed.toList();    // 反转列表顺序

print(numbers);

// split()：字符串拆分为列表
String text = '苹果,香蕉,橘子';
List<String> fruits = text.split(',');

print(fruits); // [苹果, 香蕉, 橘子]

// join()：列表拼接为字符串
String result = fruits.join('、');

print(result); // 苹果、香蕉、橘子
```

指定列表元素的类型：

```dart
List<int> numbers = [1, 2, 3];

// numbers.add('hello'); // 错误：不能添加 String
```

如果不指定泛型类型，默认可以使用 `List<dynamic>`：

```dart
List<dynamic> values = [1, 'hello', true];

values.add(3.14);
```

注意：

- 列表索引从 `0` 开始。
- 访问不存在的索引会产生运行时错误。
- `List<T>` 中的 `T` 表示列表元素的类型。

### 1.4.5 映射（Map）

`Map` 是映射，用于存储键值对（Key-Value），类似于 Python 中的字典。

每个键（Key）对应一个值（Value），可以通过键快速获取对应的值。

```dart
// 创建映射
// Map<键的类型, 值的类型>
Map<String, dynamic> person = {
  'name': '小明',
  'age': 18,
  'isStudent': true,
};

// 根据键获取值
print(person['name']); // 小明
print(person['age']);  // 18

// 修改值
person['age'] = 19;

// 添加键值对
person['city'] = '北京';

// 删除键值对
person.remove('isStudent');

// 获取 Map 长度
print(person.length);

// 判断是否包含某个键
print(person.containsKey('name')); // true

// 判断是否为空 Map ，返回布尔值
print(person.isEmpty);
print(person.isNotEmpty);

// 获取所有键
print(person.keys);

// 获取所有值
print(person.values);

// 遍历映射
person.forEach((key, value) {
  print('$key: $value');
});
```

注意：

- Map 的键（Key）必须唯一，不能存在重复的键。
- 如果使用已经存在的键添加数据，会覆盖原来的值。
- 获取不存在的键时，返回 `null`。

### 1.4.6 集合（Set）

`Set` 是集合，用于存储不重复的元素。

与 `List` 不同，Set 不允许存在重复元素，并且不能通过索引访问元素。

```dart
// 创建集合
// Set<集合存储元素的类型>
Set<String> fruits = {'苹果', '香蕉', '橘子'};

// 添加元素
fruits.add('西瓜');

// 添加重复元素，不会产生效果（自动去重）
fruits.add('苹果');

// 删除元素
fruits.remove('香蕉');

// 判断是否包含某个元素
print(fruits.contains('橘子')); // true

// 判断是否为空集合
print(fruits.isEmpty);
print(fruits.isNotEmpty);

// 获取元素数量
print(fruits.length);

// 遍历集合
for (String fruit in fruits) {
  print(fruit);
}
```

集合还可以进行数学上的集合运算：

```dart
Set<int> a = {1, 2, 3};
Set<int> b = {2, 3, 4};

// 交集
print(a.intersection(b)); // {2, 3}

// 并集
print(a.union(b)); // {1, 2, 3, 4}

// 差集：存在于 a，但不存在于 b 的元素
print(a.difference(b)); // {1}
```

注意：

- Set 中的元素不能重复。
- Set 不支持通过索引访问元素。
- 如果需要自动去重，可以将 List 转换为 Set。

```dart
List<int> numbers = [1, 2, 2, 3, 3];

Set<int> uniqueNumbers = numbers.toSet();

print(uniqueNumbers); // {1, 2, 3}
```

### 1.4.7 动态类型（dynamic）与 Object

`dynamic` 和 `Object` 都可以用于存储不同类型的数据，但它们的区别在于类型检查机制。

**1. dynamic**

`dynamic` 表示动态类型，变量可以在运行过程中改变数据类型，并且允许调用任意成员，成员是否存在通常在运行时才检查。

```dart
dynamic value = 10;

value = 'hello';
value = true;

// dynamic 可以调用任意成员
// 是否存在，运行时才会检查
print(value);
```

**2. Object**

`Object` 是 Dart 中大多数非空对象类型的共同父类型。

它可以存储不同类型的数据，但不能直接调用具体子类型才有的成员。

```dart
Object value = 10;

value = 'hello';
value = true;

// print(value.length); // 错误：Object 不一定具有 length 属性

print(value.toString()); // 可以调用 Object 提供的方法
```

**3. 两者的区别**

| 对比        | dynamic     | Object            |
| --------- | ----------- | ----------------- |
| 存储不同类型的数据 | 可以          | 可以                |
| 调用任意成员    | 可以，运行时检查    | 不可以，编译时检查         |
| 类型安全性     | 较低          | 较高                |
| 适用场景      | 确实需要动态类型的场景 | 不确定具体类型，但希望保留类型检查 |

注意：

- `dynamic` 会弱化 Dart 的静态类型检查，应尽量避免滥用。
- `Object` 默认不能存储 `null`，如果需要允许空值，可以使用 `Object?`。
- `Object` 和 `dynamic` 都不同于 `var`，`var` 会根据初始值推断类型，而不是动态改变类型。

### 1.4.8 类型转换与类型判断

在实际开发中，经常需要将一种数据类型转换为另一种数据类型。

**1. 常见类型转换**

```dart
// String 转 int
int age = int.parse('18');

// String 转 double
double price = double.parse('19.99');

// int / double 转 String
String a = 100.toString();
String b = 3.14.toString();

// int 转 double
double number = 10.toDouble();

// double 转 int（舍弃小数部分）
int result = 3.14.toInt();

print(age);    // 18
print(price);  // 19.99
print(a);      // 100
print(b);      // 3.14
print(number); // 10.0
print(result); // 3
```

**2. 安全转换（tryParse）**

使用 `parse()` 转换时，如果字符串内容不符合要求，会产生运行时异常。

如果不确定字符串能否转换成功，可以使用 `tryParse()`。

```dart
String text = '123';

// 转换成功返回对应数值，失败返回 null
int? number = int.tryParse(text);

print(number); // 123

String text2 = 'hello';

int? result = int.tryParse(text2);

print(result); // null
```

**3. 类型判断（is 和 is!）**

可以使用 `is` 判断一个变量是否属于某种类型，使用 `is!` 判断是否不属于某种类型。

```dart
dynamic value = 'hello';

// 判断是否为 String
print(value is String); // true

// 判断是否不是 int
print(value is! int);   // true

if (value is String) {
  print(value.length); // 5
}
```

注意：Dart 支持类型提升（Type Promotion），当通过 `is` 确认变量属于某种类型后，在对应作用域内可以直接使用该类型的成员。

**4. 类型转换（as）**

`as` 可以将对象显式转换为指定类型。

```dart
Object value = 'hello';

// 将 Object 转换为 String
String text = value as String;

print(text.length); // 5
```

注意：

- `as` 不会真正改变对象的数据类型，而是告诉编译器按照指定类型使用该对象。
- 如果对象实际类型不符合转换要求，会产生运行时异常。
- 不确定类型时，优先使用 `is` 判断，再进行操作。

**5. 空值转换（??）**

如果一个值可能为 `null`，可以使用 `??` 提供默认值。

```dart
String? name;

// 如果 name 为 null，则使用默认值
String result = name ?? '游客';

print(result); // 游客
```

## 1.5 运算符

运算符（Operator）用于对变量或数据进行运算和比较。

### 1.5.1 算术运算符

算术运算符用于进行数学运算。

| 运算符 | 说明 | 示例 |
|---|---|---|
| `+` | 加法 | `10 + 3` |
| `-` | 减法 | `10 - 3` |
| `*` | 乘法 | `10 * 3` |
| `/` | 除法（结果为 double） | `10 / 3` |
| `~/` | 整除（向零取整） | `10 ~/ 3` |
| `%` | 取余数 | `10 % 3` |

```dart
int a = 10;
int b = 3;

print(a + b);  // 13
print(a - b);  // 7
print(a * b);  // 30
print(a / b);  // 3.3333333333333335
print(a ~/ b); // 3
print(a % b);  // 1
```

### 1.5.2 关系运算符

关系运算符（Relational Operators）用于比较两个值，运算结果为 `bool` 类型（`true` 或 `false`）。

| 运算符 | 说明 |
|---|---|
| `==` | 等于 |
| `!=` | 不等于 |
| `>` | 大于 |
| `<` | 小于 |
| `>=` | 大于或等于 |
| `<=` | 小于或等于 |

```dart
int a = 10;
int b = 5;

print(a == b); // false
print(a != b); // true
print(a > b);  // true
print(a < b);  // false
print(a >= b); // true
print(a <= b); // false
```

### 1.5.3 逻辑运算符

逻辑运算符（Logical Operators）用于组合或取反布尔表达式，运算结果为 `bool` 类型。

| 运算符 | 说明 |
|---|---|
| `&&` | 与：两边都为 true，结果才为 true |
| `\|\|` | 或：两边至少一个为 true，结果就为 true |
| `!` | 非：将 true 变为 false，false 变为 true |

```dart
bool hasAccount = true;
bool hasPassword = false;

// 与：必须同时满足两个条件
print(hasAccount && hasPassword); // false

// 或：满足其中一个条件即可
print(hasAccount || hasPassword); // true

// 非：取反
print(!hasAccount); // false
```

**注意：** `&&` 和 `||` 具有短路特性：

- `&&`：如果左边已经是 `false`，右边就不会执行。
- `||`：如果左边已经是 `true`，右边就不会执行。

### 1.5.4 赋值运算符

赋值运算符（Assignment Operators）用于给变量赋值或修改变量的值。

| 运算符 | 等价写法 | 说明 |
|---|---|---|
| `=` | `=` | 赋值 |
| `+=` | `a = a + b` | 加后赋值 |
| `-=` | `a = a - b` | 减后赋值 |
| `*=` | `a = a * b` | 乘后赋值 |
| `/=` | `a = a / b` | 除后赋值 |
| `~/=` | `a = a ~/ b` | 整除后赋值 |
| `%=` | `a = a % b` | 取余后赋值 |

```dart
int a = 10;

a += 5;
print(a); // 15

a -= 3;
print(a); // 12

a *= 2;
print(a); // 24

a ~/= 5;
print(a); // 4

a %= 3;
print(a); // 1
```

另外，Dart 还支持空值赋值运算符 `??=`：

```dart
String? name;

// 只有变量为 null 时，才会进行赋值
name ??= '小明';

print(name); // 小明
```

### 1.5.5 条件运算符

条件运算符用于根据条件选择不同的值。

**1. 三元运算符（`? :`）**

根据条件是否成立，返回不同的值。

```dart
int age = 20;

// 条件 ? 条件成立时的值 : 条件不成立时的值
String result = age >= 18 ? '成年人' : '未成年人';

print(result); // 成年人
```

**2. 空值合并运算符（`??`）**

如果左边的值不为 `null`，就使用左边的值；否则使用右边的值。

```dart
String? name;

String result = name ?? '游客';

print(result); // 游客
```

**3. 空值感知运算符（`?.`）**

只有左边的对象不为 `null` 时，才会访问其属性或调用其方法；否则返回 `null`。

```dart
String? name;

print(name?.length); // null，不会报错
```

### 1.5.6 类型运算符

类型运算符（Type Operators）用于判断对象的类型或进行类型转换。

| 运算符 | 说明 |
|---|---|
| `is` | 判断对象是否属于某种类型 |
| `is!` | 判断对象是否不属于某种类型 |
| `as` | 将对象转换为指定类型 |

```dart
dynamic value = 'Hello Dart';

// is：判断类型
print(value is String);  // true

// is!：判断是否不属于某种类型
print(value is! int);    // true

// as：类型转换
String text = value as String;
print(text); // Hello Dart
```

**注意：** `as` 不是万能转换工具，如果对象的实际类型不兼容，转换时会发生错误。

### 1.5.7 自增与自减运算符

用于让数值增加或减少 1。

```dart
int a = 10;

a++; // 等价于 a += 1
print(a); // 11

a--; // 等价于 a -= 1
print(a); // 10
```

自增和自减还分为前置和后置：

```dart
int a = 10;

print(++a); // 11，先增加再使用
print(a++); // 11，先使用再增加
print(a);   // 12
```

### 1.5.8 级联操作符

级联操作符（Cascade Notation）允许对同一个对象连续执行多个操作，不需要重复写出对象名称。

```dart
var list = <String>[]
  ..add('苹果')
  ..add('香蕉')
  ..add('橘子');

print(list); // [苹果, 香蕉, 橘子]
```

`?..` 是空安全级联操作符，当对象为 `null` 时，不会执行后续操作。

```dart
List<String>? list;

list
  ?..add('苹果')
  ..add('香蕉');

print(list); // null
```

**注意：** 级联操作符操作的是同一个对象，整个级联表达式的结果也是该对象，而不是最后一个操作的返回值。

# 第二章 流程控制

## 2.1 条件判断

Dart 使用 `if`、`else if` 和 `else` 进行条件判断，根据不同条件执行不同的代码。

```dart
int age = 20;

if (age >= 18) {
  print('成年人');
} else if (age >= 12) {
  print('青少年');
} else {
  print('儿童');
}
```

- `if`：如果条件成立，则执行对应代码。
- `else if`：如果前面的条件不成立，则继续判断其他条件。
- `else`：如果以上条件都不成立，则执行这里的代码。
- 条件表达式必须返回 `bool` 类型（`true` 或 `false`）。

**注意：** Dart 不支持将整数或字符串直接作为条件判断，例如 `if (1)` 是错误的，必须使用布尔表达式。

```dart
bool isLogin = true;

if (isLogin) {
  print('已登录');
}
```

## 2.2 switch 语句与模式匹配

`switch` 用于根据一个值匹配不同的情况，从而执行对应的代码。

Dart 3 的现代 `switch` 语句**不再需要** `break;`，每一个非空 `case` 分支默认独立执行，执行完毕后会自动结束，不会自动贯穿。

**1. switch 语句**

```dart
String fruit = '香蕉';

switch (fruit) {
  case '苹果':
    print('Apple');
  case '香蕉':
    print('Banana');
  case '橘子':
    print('Orange');
  default:
    print('未知水果');
}   // 输出：Banana



String day = 'Sunday';

switch (day) {
  case 'Monday':
    print('星期一');

  case 'Tuesday':
    print('星期二');

  case 'Wednesday':
    print('星期三');

  case 'Thursday':
    print('星期四');

  case 'Friday':
    print('星期五');

  case 'Saturday' || 'Sunday': // 多条件合并
    print('周末');

  default:
    print('其他');
}   // 输出：周末
```

- `case`：指定需要匹配的值。
- `default`：当所有 `case` 都不匹配时执行。
- **注意：** 这里的 `||` 是模式匹配语法，不是布尔表达式。

**2. 模式匹配（Pattern Matching）**

Dart 支持模式匹配，可以在 `switch` 中根据数据类型、结构以及条件进行更灵活的判断。

```dart
Object value = 100;

switch (value) {
  case int number:
    print('整数：$number');

  case String text:
    print('字符串：$text');

  default:
    print('其他类型');
}
```

上面的 `int number` 表示：如果 `value` 是整数，就将其匹配并赋值给变量 `number`。

**3. switch 表达式**

除了 `switch` 语句，Dart 还支持 `switch` 表达式，可以直接返回一个值。

```dart
int score = 85;

String result = switch (score) {
  >= 90 => '优秀',
  >= 60 => '及格',
  _ => '不及格',
};

print(result); // 及格
```

- `=>`：表示匹配成功后返回对应的结果。
- `_`：通配符，表示匹配其他所有情况。
- `switch` 表达式必须覆盖所有可能的情况。

## 2.3 循环语句

循环用于重复执行一段代码，Dart 常用的循环语句有 `for`、`while` 和 `do-while`。

### 2.3.1 for 循环

适合已知循环次数，或者需要通过计数器控制循环的场景。

```dart
// 从 0 循环到 4
for (int i = 0; i < 5; i++) {
  print(i);
}
```

`for` 循环包含三个部分：

- 初始化：`int i = 0`，创建并初始化循环变量。
- 循环条件：`i < 5`，条件成立时继续循环。
- 迭代器：`i++`，每次循环结束后执行。

### 2.3.2 for-in 遍历

用于遍历列表、Map 、集合等可迭代对象（Iterable），不需要手动管理索引。

```dart
List<String> fruits = ['苹果', '香蕉', '橘子'];

for (String fruit in fruits) {
  print(fruit);
}
```

也可以使用 `var` 让 Dart 自动推断变量类型：

```dart
for (var fruit in fruits) {
  print(fruit);
}
```

实战训练：

```dart
void main() {
  // <======== for-in 遍历列表 ========>
  List<String> fruits = ['Apple', 'Banana', 'Orange'];

  for (var fruit in fruits) {
    print(fruit); // 依次输出: Apple, Banana, Orange
  }



  // <======== for-in 遍历 Map ========>
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

### 2.3.3 forEach 遍历
```dart
void main() {
  // <======== forEach 遍历列表 ========>
  List<String> fruits = ['Apple', 'Banana', 'Orange'];

  fruits.forEach((fruit) {
    print(fruit); // 依次输出: Apple, Banana, Orange
  });

  // forEach 打印的简化写法（不适用于 Map ）
  fruits.forEach(print); // 依次输出: Apple, Banana, Orange




  // <======== forEach 遍历 Map ========>
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

### 2.3.4 用哪一种遍历
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

### 2.3.5 while 循环

只要条件成立，就会不断执行循环体。

```dart
int i = 0;

while (i < 5) {
  print(i);
  i++; // 更新循环变量，避免无限循环
}
```

### 2.3.6 do-while 循环

与 `while` 类似，但会先执行一次循环体，再判断条件。因此，即使条件一开始就不成立，也至少会执行一次。

```dart
int i = 0;

do {
  print(i);
  i++;
} while (i < 5);
```

## 2.4 循环控制

循环控制语句用于改变循环的正常执行流程。

**1. break**

立即终止当前循环，不再执行后续循环。

```dart
for (int i = 0; i < 10; i++) {
  if (i == 5) {
    break; // i 等于 5 时终止循环
  }

  print(i);
}

// 输出：0 1 2 3 4
```

**2. continue**

跳过当前这一次循环，直接进入下一次循环。

```dart
for (int i = 0; i < 5; i++) {
  if (i == 2) {
    continue; // 跳过 i 等于 2 的这一次循环
  }

  print(i);
}

// 输出：0 1 3 4
```

**3. 标签（Label）**

Dart 支持使用标签为循环命名，配合 `break` 和 `continue` 可以控制外层循环。

标签通常使用 `outerLoop`、`innerLoop` 等名称。

```dart
outerLoop:
for (int i = 0; i < 3; i++) {
  for (int j = 0; j < 3; j++) {
    if (i == 1 && j == 1) {
      break outerLoop; // 直接终止整个外层循环
    }

    print('$i, $j');
  }
}
```

如果使用 `continue outerLoop`，则会跳过当前外层循环的剩余部分，直接进入外层循环的下一次迭代。

```dart
outerLoop:
for (int i = 0; i < 3; i++) {
  for (int j = 0; j < 3; j++) {
    if (j == 1) {
      continue outerLoop; // 直接进入下一次外层循环
    }

    print('$i, $j');
  }
}
```

## 2.5 流程控制的注意事项

1. **避免无限循环**：使用 `while` 或 `for` 时，要确保循环条件最终能够变为 `false`，否则可能导致程序一直运行。
2. **注意循环边界**：例如 `i < 5` 和 `i <= 5` 的循环次数不同，要特别注意是否会出现多循环或少循环的情况。
3. **注意循环变量的作用域**：在 `for` 循环中声明的变量，通常只能在该循环内部使用。

# 第三章 函数

## 3.1 函数基础

函数（Function）是一段具有特定功能的代码，可以将其封装起来，在需要时重复调用。

```dart
// 定义函数
void sayHello() {
  print('Hello, Dart!');
}

// 调用函数
sayHello();
```

- `void`：表示函数没有返回值。
- `sayHello`：函数名称。
- `()`：参数列表，可以为空。
- `{}`：函数体，存放需要执行的代码。

函数也可以接收参数：

```dart
// 定义一个带参数的函数
void greet(String name) {
  print('你好，$name！');
}

// 调用函数并传入参数
greet('小明'); // 你好，小明！
```

## 3.2 函数参数

Dart 函数支持多种参数形式，主要包括位置参数、可选位置参数和命名参数。

**1. 位置参数（Positional Parameters）**

调用函数时，必须按照参数定义的顺序传入对应的值。

```dart
void introduce(String name, int age) {
  print('姓名：$name，年龄：$age');
}

introduce('小明', 18);
```

**2. 可选位置参数（Optional Positional Parameters）**

使用 `[]` 将参数括起来，表示这些参数可以不传入。

```dart
void greet(String name, [String? message]) {
  print('$name：${message ?? '你好！'}');
}

greet('小明');         // 小明：你好！
greet('小红', '早上好'); // 小红：早上好
```

- 可选位置参数必须放在必需位置参数之后。
- 可选参数通常需要使用可空类型（如 `String?`），或者设置默认值。

**3. 命名参数（Named Parameters）**

使用 `{}` 定义命名参数，调用时通过参数键名传值，参数顺序可以改变。

```dart
void introduce({required String name, int age = 18}) {
  print('姓名：$name，年龄：$age');
}

introduce(name: '小明');
introduce(age: 20, name: '小红');
```

- `required`：表示该命名参数必须传入。
- `=`：可以为参数设置默认值。
- 命名参数默认是可以不传入的，除非使用 `required`。

**4. 混合写法**

**注意：命名参数要在位置参数后面**

```dart
  // 必需位置参数 + 命名参数
  void introduce(String name, {required int age, String city = '北京'}) {
    print('姓名：$name，年龄：$age，城市：$city');
  }

  // 调用函数
  introduce('小明', age: 18);
  introduce('小红', city: '上海', age: 18);
```

* 一般来说，可选位置参数用得很少。
* 参数多时，推荐使用命名参数，**默认值 + `required` 更安全**；
* **必需位置参数不支持设置默认值**。

## 3.3 函数返回值

函数可以通过 `return` 返回一个结果，供调用者使用。

```dart
// int 表示函数返回值是整数类型
int add(int a, int b) {
  return a + b;
  /*
  函数一旦用 'return ;' 返回值之后，函数就会结束执行
  所以，'return ;' 后面的代码是不会被执行的
  */
}

// 接收函数的返回值
int result = add(10, 20);

print(result); // 30
```

- `int`：表示函数返回值的类型。
- `return`：结束函数执行，并返回指定的值。
- 如果函数不需要返回值，可以使用 `void`。

函数也可以根据不同条件返回不同结果：

```dart
// String 表示函数返回值是字符串类型
String checkAge(int age) {
  if (age >= 18) {
    return '成年人';
  } else {
    return '未成年人';
  }
}

print(checkAge(20)); // 成年人
```

在 Flutter 中，你还会经常看到函数返回值的类型是 `Widget`（组件）

```dart
// Widget 表示函数返回一个界面组件
Widget buildTitle() {
  return Text('Hello Flutter');
}

// 在 Flutter 界面中使用
@override
Widget build(BuildContext context) {
  return Scaffold(
    body: Center(
      child: buildTitle(),
    ),
  );
}
```

**补充说明：**

- `Widget`：Flutter 中所有界面组件的基础类型之一。
- `buildTitle()`：返回一个 `Text` 组件。
- Flutter 中经常将界面拆分成多个函数或组件，以便复用和管理代码。

**注意：** `Widget` 是返回值类型，`Text` 是具体返回的组件类型。

## 3.4 匿名函数与箭头函数

**1. 匿名函数（Anonymous Function）**

匿名函数是没有名称的函数，通常赋值给变量，或者作为参数传递给其他函数。

```dart
// 将匿名函数赋值给变量
var greet = (String name) {
  print('你好，$name！');
};

// 调用匿名函数
greet('小明');
```

**2. 箭头函数（Arrow Function）**

当函数体只有一条表达式时，可以使用 `=>` 简化函数的写法。

```dart
// 普通函数
int add(int a, int b) {
  return a + b;
}

// 箭头函数
int add2(int a, int b) => a + b;

print(add2(10, 20)); // 30
```

`=>` 会自动返回值。

**3. 匿名箭头函数**

匿名函数也可以使用箭头语法，常见于 `List` 的 `forEach()`、`map()` 等方法中。

```dart
List<String> fruits = ['苹果', '香蕉', '橘子'];

// 使用匿名箭头函数遍历列表
fruits.forEach((fruit) => print(fruit));
```

## 3.5 回调函数

### 3.5.1 回调函数（Callback）

回调函数是指将一个函数作为参数传递给另一个函数，在适当的时候由接收方调用。

```dart
// 定义一个普通函数
void sayHello() {
  print('Hello!');
}

// 接收函数作为参数
void execute(Function callback) {
  callback(); // 调用传入的函数
}

// 将 sayHello 作为回调函数传入
execute(sayHello); // Hello!
```

### 3.5.2 定义时的书写格式1

回调函数作为参数传入时，书写格式为 `返回值类型` `Function(回调函数参数值的类型)` `回调函数的名字`" 

```dart
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

### 3.5.3 定义时的书写格式2

若回调函数既不接受参数，也不返回任何值时候，`void` `Function()` `回调函数的名字` 还可以写作 `VoidCallback` `回调函数的名字` 

```dart
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

## 3.6 高阶函数

Dart 中常见的高阶函数（Higher-Order Function）有列表的 `forEach()`、`map()`、`where()` 等方法。

```dart
List<int> numbers = [1, 2, 3, 4, 5];

// forEach：遍历列表
numbers.forEach((number) => print(number));

// map：转换列表中的每个元素
var doubled = numbers.map((number) => number * 2).toList();

// where：筛选符合条件的元素，条件是一个布尔表达式，False 的被过滤掉
var evenNumbers = numbers.where((number) => number % 2 == 0).toList();

print(doubled);     // [2, 4, 6, 8, 10]
print(evenNumbers); // [2, 4]
```

## 3.7 递归函数

递归（Recursion）是指函数在执行过程中调用自身，通常用于解决可以分解为相同类型子问题的问题。

递归函数需要设置结束条件，否则会不断调用自身，最终导致堆栈溢出。

```dart
// 使用递归计算阶乘
int factorial(int n) {
  if (n <= 1) {
    return 1; // 结束条件
  }

  return n * factorial(n - 1); // 调用自身
}

print(factorial(5)); // 120（5 × 4 × 3 × 2 × 1）
```

递归虽然可以简化某些问题的代码，但调用层级过深可能导致堆栈溢出，因此需要合理控制递归深度。

## 3.8 变量作用域

变量作用域（Variable Scope）是指变量能够被访问的代码范围。

**1. 局部变量（Local Variable）**

在函数或代码块内部定义的变量，只能在其作用域内访问。

```dart
void test() {
  int age = 18; // 局部变量
  print(age);   // 可以访问
}

test();

// print(age); // 错误：无法访问函数内部的局部变量
```

**2. 全局变量（Global Variable）**

在所有函数和类之外定义的变量，可以在同一个库（Library）内的其他代码中访问。

```dart
String name = '小明'; // 顶层变量

void sayHello() {
  print('你好，$name！');
}

sayHello(); // 你好，小明！
```

**3. 代码块作用域**

在 `{}` 内定义的局部变量，其作用域通常限制在对应的代码块及其内部。

```dart
void test() {
  if (true) {
    int age = 18;
    print(age); // 18
  }

  // print(age); // 错误：无法访问 if 代码块中的变量
}
```

## 3.9 私有成员

Dart 使用下划线 `_` 表示私有成员。变量、函数或类的名称以 `_` 开头时，表示该成员只能在当前库（Library）内部访问。

**1. 私有变量**

变量名称以 `_` 开头时，表示该变量是私有的，不能在其他库中直接访问。

```dart
// 公有变量
String name = '小明';

// 私有变量
int _age = 18;

void main() {
  print(name); // 小明
  print(_age); // 18
}
```

**2. 私有函数**

函数名称以 `_` 开头时，表示该函数是私有的，不能在其他库中直接调用。

```dart
// 公有函数
void sayHello() {
  print('Hello!');
}

// 私有函数
void _saySecret() {
  print('这是一个私有函数');
}

void main() {
  sayHello();
  _saySecret();
}
```

**3. 私有类**

类名称以 `_` 开头时，表示该类是私有的，不能在其他库中直接使用。

```dart
// 私有类
class _Person {
  String name = '小明';
}

void main() {
  var person = _Person();
  print(person.name);
}
```

需要注意：

- 默认情况下，成员都是公有的。
- 名称以 `_` 开头的变量、函数和类都是私有的。
- Dart 的私有访问控制是以库（Library）为单位，而不是以类或文件为单位。
- 同一个库中的其他文件也可以访问私有成员，前提是这些文件属于同一个库。

## 3.10 闭包（Closure）

闭包是指函数可以访问并保留其定义时所在作用域中的变量，即使外层函数已经执行结束，这些变量仍然可以被访问。

```dart
// 定义一个返回函数的函数
Function createCounter() {
  int count = 0; // 外层函数的局部变量

  // 返回内部函数
  return () {
    count++; // 访问并修改外层函数的变量
    print(count);
  };
}

void main() {
  var counter = createCounter();

  counter(); // 1
  counter(); // 2
  counter(); // 3
}
```

这里的 `createCounter()` 执行结束后，局部变量 `count` 原本应该随着函数执行结束而离开普通作用域，但由于内部函数仍然引用它，因此 `count` 会被保留。

闭包常见用途：

- 保存函数内部的状态。
- 创建具有独立状态的计数器。
- 封装变量，避免外部直接访问。
- 在回调函数中保留需要使用的数据。

# 第四章 面向对象编程（OOP）

## 4.1 类与对象

**类（Class）** 是创建对象的模板，用于定义对象具有的属性（变量）和行为（方法）。

**对象（Object）** 是类的具体实例，可以通过类创建多个对象，每个对象都有自己的属性值。

```dart
// 定义一个类
class Person {
  // 属性
  String name = '张三';
  int age = 18;

  // 方法
  void introduce() {
    print('我叫$name，今年$age岁');
  }
}

void main() {
  // 创建对象（实例化）
  Person person = Person();

  // 访问属性
  print(person.name); // 张三

  // 调用方法
  person.introduce(); // 我叫张三，今年18岁

  // 修改属性
  person.age = 20;
  person.introduce(); // 我叫张三，今年20岁
}
```

**注意：**
- 使用 `class` 关键字定义类。
- 使用 `类名()` 创建对象，也称为实例化（Instantiation）。
- 使用 `.` 访问对象的属性和方法。
- 同一个类可以创建多个对象，各个对象的属性互不影响。

## 4.2 构造函数（Constructor）

构造函数是一种特殊的函数，在创建对象时自动调用，主要用于初始化对象的属性。

### 4.2.1 默认构造函数（Default Constructor）

如果类中没有定义任何构造函数，Dart 会自动提供一个无参数的默认构造函数。

```dart
class Person {
  String name = '张三';
  int age = 18;
}

void main() {
  // 使用默认构造函数创建对象
  Person person = Person();

  print(person.name); // 张三
  print(person.age);  // 18
}
```

**注意：**
- 默认构造函数不需要手动定义。
- 如果类中已经定义了其他构造函数，Dart 就不会再自动提供默认构造函数。

### 4.2.2 生成式构造函数（Generative Constructor）

生成式构造函数用于创建类的实例，可以通过参数在创建对象时初始化属性。

构造函数的名称与类名相同，并且不需要声明返回值类型。

```dart
class Person {
  String name;
  int age;

  // 生成式构造函数
  Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
}

void main() {
  // 创建对象时传入参数
  Person person = Person('张三', 18);

  print(person.name); // 张三
  print(person.age);  // 18
}
```

**注意：**
- `this` 表示当前对象，用于区分实例变量和构造函数参数。
- 生成式构造函数可以包含代码，在创建对象时执行。

### 4.2.3 初始化形式参数（Initializing Formal Parameters）

当构造函数的参数与实例变量名称相同时，可以使用 `this.变量名` 简化属性初始化。

```dart
class Person {
  String name;
  int age;

  // 使用 this 简化属性初始化
  Person(this.name, this.age);
}

void main() {
  Person person = Person('张三', 18);

  print(person.name); // 张三
  print(person.age);  // 18
}
```

这种写法等价于在生成式构造函数中使用 `this.name = name` 和 `this.age = age`。

**注意：**
- `this.name` 表示将传入的参数直接赋值给当前对象的 `name` 属性。
- 这种写法可以减少重复代码，是 Dart 中非常常见的构造函数写法。

### 4.2.4 命名构造函数（Named Constructor）

Dart 支持为同一个类定义多个不同名称的构造函数，以满足不同的对象创建需求。

使用 `类名.名称()` 定义命名构造函数。

```dart
class Person {
  String name;
  int age;

  // 生成式构造函数
  Person(this.name, this.age);

  // 命名构造函数
  Person.guest()
      : name = '游客',
        age = 0;
}

void main() {
  // 使用生成式构造函数
  Person person1 = Person('张三', 18);

  // 使用命名构造函数
  Person person2 = Person.guest();

  print(person1.name); // 张三
  print(person2.name); // 游客
}
```

**注意：**
- 命名构造函数使用 `类名.名称()` 的形式定义。
- 一个类可以定义多个命名构造函数。
- 不同构造函数可以用于不同场景。

**示例：使用 fromJson() 将 JSON 数据反序列化为对象**

```dart
class Person {
  String name;
  int age;

  // 普通构造函数
  Person(this.name, this.age);

  // 命名构造函数：从 JSON 数据创建对象
  Person.fromJson(Map<String, dynamic> json)
      : name = json['name'],
        age = json['age'];
}

void main() {
  // 模拟从服务器获取的 JSON 数据
  Map<String, dynamic> json = {
    'name': '张三',
    'age': 18,
  };

  // 将 JSON 数据转换为 Person 对象
  Person person = Person.fromJson(json);

  print(person.name); // 张三
  print(person.age);  // 18
}
```

**补充说明：**

- `fromJson` 是一种常见的命名构造函数名称，并不是 Dart 规定的固定名称。
- `Map<String, dynamic>` 用于接收 JSON 对象转换后的 Map 数据。
- `Person.fromJson(json)` 会根据 JSON 中的字段创建一个 `Person` 对象。

**Flutter 开发中的常见用法：**

实际项目中，经常使用 `fromJson()` 将服务器 API 返回的数据反序列化为 Dart 模型对象。例如：

```dart
// 将服务器返回的 JSON 字符串转换为 Map
Map<String, dynamic> json = jsonDecode(response.body);

// 将 Map 转换为 User 对象
User user = User.fromJson(json);
```

### 4.2.5 常量构造函数（Constant Constructor）

使用 `const` 定义常量构造函数，可以创建编译时常量对象。

常量构造函数要求实例变量必须是 `final`，并且不能在构造函数主体中执行普通初始化操作。

```dart
class Point {
  final int x;
  final int y;

  // 常量构造函数
  const Point(this.x, this.y);
}

void main() {
  // 创建编译时常量对象
  const point = Point(10, 20);

  print(point.x); // 10
  print(point.y); // 20
}
```

**注意：**
- 常量构造函数使用 `const` 关键字声明。
- 常量对象创建后，其内部状态不能被修改。
- 创建常量对象时，通常使用 `const` 调用构造函数。
- 常量对象在满足条件时可以复用相同的实例。

### 4.2.6 主构造函数（Primary Constructor）

Dart 3.13 引入了主构造函数（Primary Constructor），可以直接在类名后面的括号中声明构造参数，简化类的定义。

与传统构造函数相比，主构造函数不需要在类内部单独编写构造函数，可以直接在类声明时定义参数。

**1. 位置参数**

```dart
// 使用 var 声明实例变量，允许后续修改
class Person(var String name, var int age);

void main() {
  Person person = Person('张三', 18);

  print(person.name); // 张三
  print(person.age);  // 18

  // 修改实例变量
  person.age = 20;
}
```

**2. 命名参数**

主构造函数也支持命名参数，可以使用 `{}` 定义，并通过参数名称传递数据。

```dart
class Person({
  var String name = '游客',
  var int age = 18,
});

void main() {
  // 使用默认值
  Person person1 = Person();

  // 传入指定参数
  Person person2 = Person(name: '张三', age: 20);

  print(person1.name); // 游客
  print(person2.name); // 张三
}
```

也可以使用 `required` 将命名参数设置为必填参数：

```dart
class Person({
  required var String name,
  required var int age,
});

void main() {
  Person person = Person(name: '张三', age: 18);
}
```

**3. var 与 final**

主构造函数中，可以使用 `var` 或 `final` 决定是否将参数声明为实例变量。

```dart
// var：创建可修改的实例变量
class Person(var String name);

// final：创建不可重新赋值的实例变量
class User(final String name);

// 不使用修饰符：只声明构造函数参数，不创建实例变量
class Animal(String name);
```

区别如下：

| 写法 | 是否创建实例变量 | 是否可以重新赋值 |
|---|---|---|
| `var String name` | 是 | 可以 |
| `final String name` | 是 | 不可以 |
| `String name` | 否 | 不适用 |

例如：

```dart
class Person(var String name);

class User(final String name);

class Animal(String name);

void main() {
  Person person = Person('张三');
  person.name = '李四'; // 允许修改

  User user = User('王五');
  // user.name = '赵六'; // 错误：final 不能重新赋值

  Animal animal = Animal('小猫');
  // print(animal.name); // 错误：name 不是实例变量
}
```

**4. 定义普通方法**

主构造函数也可以与普通方法一起使用。

```dart
class Person(var String name, var int age) {
  // 普通方法
  void introduce() {
    print('我叫$name，今年$age岁');
  }
}

void main() {
  Person person = Person('张三', 18);

  person.introduce(); // 我叫张三，今年18岁
}
```

**5. 与传统构造函数对比**

传统写法：

```dart
class Person {
  String name;
  int age;

  Person(this.name, this.age);

  void introduce() {
    print('我叫$name，今年$age岁');
  }
}
```

主构造函数写法：

```dart
class Person(var String name, var int age) {
  void introduce() {
    print('我叫$name，今年$age岁');
  }
}
```

两种写法都可以实现相同的功能，主构造函数能够减少重复代码，使类的定义更加简洁。

**注意：**
- 主构造函数是 Dart 3.13 引入的语法，需要使用 Dart 3.13 或更高版本。
- 主构造函数支持位置参数和命名参数。
- 使用 `var` 或 `final` 可以将参数声明为实例变量；不使用修饰符时，参数不会成为实例变量，**所以建议带上修饰符**。
- `var` 声明的实例变量可以重新赋值，`final` 声明的实例变量只能初始化一次。
- 主构造函数并不能完全取代传统构造函数，复杂的初始化场景仍然可以使用传统构造函数。

## 4.3 初始化列表（Initializer List）

**初始化列表** 用于在构造函数执行之前初始化实例变量，使用 `:` 放在构造函数参数之后。

常见用途：
- 初始化 `final` 实例变量。
- 根据构造函数参数计算属性值。
- 对参数进行初始化处理。

```dart
class Person {
  String name;
  int age;
  final String description;

  Person(String name, int age)
      : name = name,
        age = age,
        description = '$name，今年$age岁';

}

void main() {
  Person person = Person('张三', 18);

  print(person.description); // 张三，今年18岁
}
```

**注意：**
- 初始化列表使用 `:` 开始，多个初始化表达式使用逗号分隔。
- 初始化列表在构造函数主体 `{}` 执行之前完成。
- 使用 `this.name` 这种形式的参数初始化属性，本身就是一种简化的初始化方式。

## 4.4 命名规范

Dart 推荐使用以下命名规范：

| 命名类型 | 命名规范 | 示例 |
|---|---|---|
| 类名 | 大驼峰命名法（UpperCamelCase） | `Person`、`UserInfo` |
| 变量名 | 小驼峰命名法（lowerCamelCase） | `userName`、`userAge` |
| 函数名 | 小驼峰命名法（lowerCamelCase） | `getUser()`、`showInfo()` |
| 常量名 | 小驼峰命名法（lowerCamelCase） | `maxCount`、`defaultName` |
| 文件名 | 小写加下划线（snake_case） | `user_info.dart` |

**补充：**
- 私有成员以 `_` 开头，例如 `_password`、`_getData()`。
- 布尔类型变量通常使用 `is`、`has`、`can` 等前缀，例如 `isLogin`、`hasPermission`。
- Dart 官方推荐使用 `lowerCamelCase` 命名常量，而不是全部使用大写字母和下划线。

## 4.5 封装（Encapsulation）

**封装** 是面向对象编程的重要特性，指将数据和操作数据的方法放在类中，并控制外部对内部数据的访问。

Dart 通过以下方式实现封装：

- **私有成员：** 名称以 `_` 开头，只能在当前库（Library）内部访问。
- **Getter：** 使用 `get` 定义，用于读取属性。
- **Setter：** 使用 `set` 定义，用于修改属性，可以在修改时进行条件判断。

```dart
class Person {
  // 私有属性，外部无法直接访问
  int _age = 18;

  // Getter：获取年龄
  int get age => _age;

  // Setter：设置年龄
  set age(int value) {
    if (value >= 0) {
      _age = value;
    } else {
      print('年龄不能小于 0');
    }
  }
}

void main() {
  Person person = Person();

  // 通过 Getter 读取属性
  print(person.age); // 18

  // 通过 Setter 修改属性
  person.age = 20;
  print(person.age); // 20

  person.age = -5; // 年龄不能小于 0
}
```

**注意：**
- Dart 没有像 Java、C++ 那样的 `public`、`private` 访问修饰符。
- 名称以 `_` 开头的成员属于库级私有，而不是严格意义上的类级私有。
- Getter 和 Setter 可以像普通属性一样使用，不需要添加 `()`。
- 封装可以避免外部随意修改内部数据，提高代码的安全性和可维护性。

## 4.6 继承（Inheritance）

### 4.6.1 继承

**继承** 是面向对象编程的重要特性，允许一个类继承另一个类的属性和方法，从而实现代码复用。

- **父类（Parent Class）：** 被继承的类，也称为基类（Base Class）。
- **子类（Child Class）：** 继承其他类的类，也称为派生类（Derived Class）。
- 使用 `extends` 关键字实现继承。

```dart
// 父类
class Animal {
  String name = '动物';

  void eat() {
    print('$name 正在吃东西');
  }
}

// 子类继承父类
class Dog extends Animal {
  // 子类自己的方法
  void bark() {
    print('$name 正在汪汪叫');
  }
}

void main() {
  Dog dog = Dog();

  // 使用继承自父类的属性和方法
  print(dog.name); // 动物
  dog.eat();       // 动物 正在吃东西

  // 调用子类自己的方法
  dog.bark();      // 动物 正在汪汪叫
}
```

**注意：**
- Dart 使用 `extends` 实现单继承，一个类只能直接继承一个父类。
- 子类可以使用父类中可访问的属性和方法。
- 子类可以在继承父类的基础上，定义自己的属性和方法。
- 子类继承父类后，可以在不重复编写父类代码的情况下，直接使用父类已有的功能。

### 4.6.2 super 关键字

`super` 用于访问父类的成员（属性和方法），也可以用于调用父类的构造函数。

**1. 访问父类成员**

使用 `super.成员名` 可以访问父类中定义的属性或方法。

```dart
class Animal {
  String name = '动物';

  void eat() {
    print('$name 正在吃东西');
  }
}

class Dog extends Animal {
  void showInfo() {
    // 访问父类属性
    print(super.name); // 动物

    // 调用父类方法
    super.eat(); // 动物 正在吃东西
  }
}

void main() {
  Dog dog = Dog();
  dog.showInfo();
}
```

**2. 调用父类构造函数**

使用 `super(参数)` 可以调用父类的生成式构造函数；如果父类使用了命名构造函数，则使用 `super.名称(参数)` 调用。

```dart
class Animal {
  String name;

  Animal(this.name);
}

class Dog extends Animal {
  int age;

  // 调用父类构造函数，初始化父类的 name 属性
  Dog(String name, this.age) : super(name);
}

void main() {
  Dog dog = Dog('小狗', 3);

  print(dog.name); // 小狗
  print(dog.age);  // 3
}
```

**注意：**
- `super.成员名` 用于访问父类的属性或方法。
- `super(参数)` 用于调用父类的生成式构造函数。
- `super.名称(参数)` 用于调用父类的命名构造函数。
- 调用父类构造函数时，通常在子类构造函数的初始化列表中使用 `super()`。

### 4.6.3 方法重写（Method Overriding）

子类可以重新定义父类已有的方法，从而实现不同的行为。通常使用 `@override` 注解标记。

```dart
class Animal {
  void makeSound() {
    print('动物发出声音');
  }
}

class Dog extends Animal {
  @override
  void makeSound() {
    print('汪汪汪');
  }
}

void main() {
  Dog dog = Dog();
  dog.makeSound(); // 汪汪汪
}
```

## 4.7 多态（Polymorphism）

简单来说就是，不同的子类重写了父类的同一个方法，之后，虽然调用了同一个方法，但是在不同对象上却可以表现出不同的结果。

```dart
// 父类
class Animal {
  void makeSound() {
    print('动物发出声音');
  }
}

// 子类
class Dog extends Animal {
  @override
  void makeSound() {
    print('汪汪汪');
  }
}

class Cat extends Animal {
  @override
  void makeSound() {
    print('喵喵喵');
  }
}

void main() {
  Animal animal1 = Dog();
  Animal animal2 = Cat();

  animal1.makeSound(); // 汪汪汪
  animal2.makeSound(); // 喵喵喵
}
```
多态通常通过继承和重写方法实现。

## 4.8 混入（Mixin）

**Mixin（混入）** 是 Dart 提供的一种代码复用机制，可以将一个类中的属性和方法混入另一个类中，而不需要通过继承实现。

使用 `mixin` 定义混入，使用 `with` 将混入应用到类中。

```dart
// 定义一个混入
mixin Swimming {
  void swim() {
    print('正在游泳');
  }
}

// 定义另一个混入
mixin Flying {
  void fly() {
    print('正在飞行');
  }
}

// 将多个混入应用到类中
class Duck with Swimming, Flying {
  void quack() {
    print('嘎嘎嘎');
  }
}

void main() {
  Duck duck = Duck();

  duck.swim();  // 正在游泳
  duck.fly();   // 正在飞行
  duck.quack(); // 嘎嘎嘎
}
```

**注意：**
- 使用 `mixin` 定义混入，使用 `with` 应用混入。
- 一个类可以同时混入多个 Mixin。
- Mixin 可以包含属性和方法。
- 混入与继承不同，一个类可以混入多个 Mixin，因此可以实现类似多重继承的代码复用效果。

**限制混入条件（on）**

可以使用 `on` 限制 Mixin 只能混入指定类型的类。

```dart
class Animal {
  void eat() {
    print('正在吃东西');
  }
}

// 只能混入 Animal 或其子类
mixin Running on Animal {
  void run() {
    eat(); // 可以访问 Animal 中的方法
    print('正在奔跑');
  }
}

class Dog extends Animal with Running {}

void main() {
  Dog dog = Dog();
  dog.run();
}
```

## 4.9 静态成员（Static）

**静态成员** 是使用 `static` 声明的类成员，包括静态变量和静态方法。

静态成员属于类本身，而不是某个对象，因此可以直接通过类名访问，不需要创建对象。

```dart
class Calculator {
  // 静态变量
  static double pi = 3.14159;

  // 静态方法
  static int add(int a, int b) {
    return a + b;
  }
}

void main() {
  // 直接通过类名访问静态变量
  print(Calculator.pi); // 3.14159

  // 直接通过类名调用静态方法
  print(Calculator.add(10, 20)); // 30
}
```

**注意：**
- 使用 `static` 声明静态变量和静态方法。
- 静态成员属于类，不属于对象。
- 静态成员可以直接通过 `类名.成员名` 访问。
- 静态方法不能直接访问非静态成员，因为非静态成员属于具体对象。
- 静态成员通常用于工具方法、共享数据和通用常量等场景。

## 4.10 枚举（Enum）

### 4.10.1 定义与使用枚举

**枚举（Enumeration）** 是一种特殊的数据类型，用于定义一组固定的常量选项。

使用 `enum` 关键字定义枚举，适合表示状态、类型、方向等固定选项。

```dart
// 定义枚举
enum Status {
  pending,  // 待处理
  success,  // 成功
  failed,   // 失败
}

void main() {
  // 声明枚举变量
  Status status = Status.success;

  print(status);      // Status.success
  print(status.name); // success

  // 判断枚举值
  if (status == Status.success) {
    print('操作成功');
  }
}
```

**注意：**
- 枚举的每个选项都是一个固定的枚举值。
- 使用 `枚举名.选项名` 访问枚举值。
- 可以通过 `.name` 获取枚举值的名称。

### 4.10.2 遍历枚举值

使用 `.values` 可以获取枚举的所有选项，并通过 for-in 循环遍历。

```dart
enum Direction {
  north,
  south,
  east,
  west,
}

void main() {
  // 遍历枚举的所有选项
  for (Direction direction in Direction.values) {
    print(direction.name);
  }
}
```

输出结果：

```text
north
south
east
west
```

### 4.10.3 枚举与 switch

枚举经常与 `switch` 配合使用，根据不同的枚举值执行不同的代码。

```dart
enum Status {
  pending,
  success,
  failed,
}

void main() {
  Status status = Status.success;

  switch (status) {
    case Status.pending:
      print('正在处理中');
    case Status.success:
      print('操作成功');
    case Status.failed:
      print('操作失败');
  }
}
```

**注意：**
- `switch` 可以根据不同的枚举值执行对应的分支。
- Dart 3.10 及以上版本支持省略 `case` 中的枚举类型前缀。
- 每个非空 `case` 分支执行完毕后会自动结束，不需要使用 `break`。

# 第五章 异步编程

## 5.1 异步编程简介

**异步编程（Asynchronous Programming）** 是一种不必等待任务完成，就可以继续执行其他代码的编程方式，常用于处理耗时操作。

例如：

- 网络请求：从服务器获取数据。
- 文件操作：读取或写入文件。
- 数据库操作：查询或保存数据。
- 定时任务：等待一段时间后执行代码。

**同步与异步的区别：**

- **同步（Synchronous）：** 按照代码顺序执行，前面的操作没有完成，后面的操作就需要等待。
- **异步（Asynchronous）：** 发起耗时操作后，不必一直等待，可以先执行其他代码，等操作完成后再处理结果。

Dart 使用 `Future`、`async` 和 `await` 实现异步编程。

## 5.2 Future

**Future** 表示一个将来可能完成的操作，其结果可能是一个值，也可能是一个异常。

可以把 `Future` 理解为一张**取餐凭证**：

- 取餐操作尚未完成：Future 处于等待状态。
- 取餐成功：Future 返回结果。
- 取餐失败：Future 返回异常。

Future 有三种状态：

- `pending`：等待中，操作尚未完成。
- `completed with a value`：成功完成，并返回结果。
- `completed with an error`：执行失败，并返回异常。

### 5.2.1 创建 Future

使用 `Future` 构造函数可以创建一个异步任务。

```dart
// 创建一个 Future，延迟 2 秒后返回字符串
Future<String> fetchData() {
  return Future.delayed(Duration(seconds: 2), () {
    return '数据加载成功';
  });
}

// 调用异步函数
Future<void> main() async {
  print('开始加载');

  // 等待异步操作完成
  String result = await fetchData();

  print(result); // 2 秒后输出：数据加载成功

  print('继续执行其他代码');
}
```

`Future.delayed()` 用于延迟一段时间后执行操作。

### 5.2.2 async 与 await

`async` 和 `await` 可以让异步代码看起来更像同步代码，更容易阅读和维护。

- `async`：声明一个异步函数，该函数会返回一个 `Future`。
- `await`：等待一个 Future 完成，并获取它的执行结果。在等待期间，当前异步函数会暂停执行，但不会阻塞整个程序。

**基本用法：**

```dart
// async 声明异步函数
Future<String> fetchData() async {
  // await 等待异步操作完成
  await Future.delayed(Duration(seconds: 2));

  return '数据加载成功';
}

Future<void> main() async {
  print('开始加载');

  // 等待 fetchData() 执行完成
  String result = await fetchData();

  print(result); // 2 秒后输出：数据加载成功

  print('加载完成');
}
```

**await 的位置会影响代码的执行顺序：**

`await` 写在不同的位置，可能产生不同的执行效果。下面通过两个例子进行对比。

**示例一：使用 await 等待任务完成**

```dart
Future<void> task() async {
  await Future.delayed(Duration(seconds: 2));
  print('任务完成');
}

Future<void> main() async {
  print('开始');

  await task(); // 等待 task() 执行完成

  print('继续执行');
}
```

输出结果：

```
开始
（等待 2 秒）
任务完成
继续执行
```

这里的 `await` 会让当前函数等待 `task()` 完成后，才继续执行后面的代码。

**示例二：不使用 await 等待任务完成**

```dart
Future<void> task() async {
  await Future.delayed(Duration(seconds: 2));
  print('任务完成');
}

Future<void> main() async {
  print('开始');

  task(); // 启动任务，但不等待它完成

  print('继续执行');
}
```

输出结果：

```
开始
继续执行
（等待 2 秒）
任务完成
```

这里调用 `task()` 后，异步任务就会开始执行，但由于没有使用 `await`，主函数不会等待它完成，而是继续执行后面的代码。

**示例三：多个异步任务的执行顺序**

```dart
Future<void> task1() async {
  await Future.delayed(Duration(seconds: 2));
  print('任务 1 完成');
}

Future<void> task2() async {
  await Future.delayed(Duration(seconds: 1));
  print('任务 2 完成');
}

Future<void> main() async {
  print('开始');

  await task1();
  await task2();

  print('全部完成');
}
```

输出结果：

```
开始
（等待 2 秒）
任务 1 完成
（等待 1 秒）
任务 2 完成
全部完成
```

这里两个任务是**依次执行**的：必须等 `task1()` 完成后，才会开始执行 `task2()`。

如果不需要等待前一个任务完成，可以先启动多个任务，再分别使用 `await` 等待：

```dart
Future<void> main() async {
  print('开始');

  // 先启动两个任务，不立即等待
  Future<void> future1 = task1();
  Future<void> future2 = task2();

  // 分别等待两个任务完成
  await future1;
  await future2;

  print('全部完成');
}
```

此时两个任务可以同时进行，不需要等任务 1 完成后才开始任务 2。

输出结果：

```
开始
（等待 1 秒）
任务 2 完成
（再等待 1 秒）
任务 1 完成
全部完成
```

**说明：**

- `task1()` 和 `task2()` 先后启动，但两个任务的计时是同时开始的。
- `task2()` 只需要等待 1 秒，因此先完成。
- `task1()` 需要等待 2 秒，因此后完成。
- 两个任务都完成后，才会执行 `print('全部完成')`。
- `await future1` 虽然写在 `await future2` 前面，但不代表任务 1 一定先完成。任务的实际完成顺序取决于各自的执行时间。

**注意：**

1. `await` 通常只能在 `async` 函数内部使用。
2. `async` 函数即使直接返回一个普通值，实际返回的也是 `Future`。
3. `await` 会暂停当前异步函数的执行，直到等待的 Future 完成，但不会阻塞整个程序。
4. 有 `await`：等待任务完成后再执行后面的代码；没有 `await`：启动任务后继续执行，不等待任务完成。
5. 多个异步任务可以先启动，再分别等待，这样可以避免不必要的顺序等待。

**小结：** `async` 负责声明异步函数，`await` 负责等待异步操作完成。是否使用 `await`，以及将 `await` 放在哪里，会直接影响异步任务的执行顺序。

## 5.3 Stream（略）
## 5.4 Isolate（略）

# 第六章 异常处理

## 6.1 异常基础

**异常（Exception）** 是程序运行过程中发生的错误或意外情况，可能导致程序无法按照预期执行。

例如：
- 将字符串转换为数字时，字符串格式不正确。
- 访问列表中不存在的索引。
- 执行除法时，除数为零。

异常处理（Exception Handling）可以让程序在发生异常时执行相应的处理逻辑，而不是直接中断当前执行流程。

```dart
void main() {
  // 将字符串转换为整数
  int number = int.parse('abc');

  print(number);
}
```

上面的代码会产生 `FormatException`，因为 `'abc'` 不是有效的整数格式，程序无法正常执行后续代码。

## 6.2 异常类型

Dart 中常见的异常和错误主要分为以下几类：

- **Exception（异常）**：表示程序运行过程中可能出现的异常情况，通常可以通过 `try-catch` 处理。
- **Error（错误）**：通常表示程序存在严重问题，例如内存使用错误或程序设计错误，一般不建议通过捕获错误来掩盖问题。

常见类型：

| 类型 | 说明 |
|---|---|
| `FormatException` | 数据格式不正确，例如字符串无法转换为数字 |
| `RangeError` | 索引或数值超出允许范围 |
| `ArgumentError` | 函数接收到不合法的参数 |
| `StateError` | 程序当前状态不允许执行某项操作 |
| `TypeError` | 类型使用不正确 |
| `UnsupportedError` | 尝试执行不受支持的操作 |
| `AssertionError` | 断言条件不成立 |

注意：并不是所有异常都需要提前记住，遇到具体问题时再查阅对应类型即可。

## 6.3 try-catch-finally

Dart 使用 `try-catch-finally` 处理异常。

- **try**：放置可能产生异常的代码。
- **catch**：捕获异常，并执行相应的处理逻辑。
- **finally**：无论是否发生异常，通常都会执行其中的代码，常用于资源清理。

```dart
void main() {
  try {
    // 可能产生异常的代码
    int number = int.parse('abc');

    print(number);
  } catch (e) {
    // 捕获异常
    print('发生异常：$e');
  } finally {
    // 无论是否发生异常，都会执行
    print('程序执行结束');
  }

  print('继续执行其他代码');
}
```

输出结果：

```text
发生异常：FormatException: Invalid radix-10 number (at character 1)
abc
^
程序执行结束
继续执行其他代码
```

**同时获取异常堆栈信息：**

`catch` 可以接收两个参数：
- 第一个参数 `e`：异常对象。
- 第二个参数 `s`：异常发生时的堆栈信息（Stack Trace），用于定位错误发生的位置。

```dart
try {
  int number = int.parse('abc');
} catch (e, s) {
  print('异常信息：$e');
  print('堆栈信息：$s');
}
```

**注意：** `catch` 和 `finally` 都是可选的，但 `try` 后面至少需要跟一个 `catch` 或 `finally`。

## 6.4 throw 与 rethrow

**throw** 用于主动抛出异常。当程序遇到不符合预期的情况时，可以使用 `throw` 产生异常。

```dart
void checkAge(int age) {
  if (age < 18) {
    // 主动抛出异常
    throw Exception('年龄必须满 18 岁');
  }

  print('验证通过');
}

void main() {
  try {
    checkAge(16);
  } catch (e) {
    print('发生异常：$e');
  }
}
```

输出结果：

```text
发生异常：Exception: 年龄必须满 18 岁
```

**rethrow** 用于在 `catch` 语句中重新抛出当前捕获的异常，通常用于记录异常信息后，将异常交给上层代码继续处理。

```dart
void readData() {
  try {
    int number = int.parse('abc');
  } catch (e) {
    print('记录异常：$e');

    // 重新抛出当前异常
    rethrow;
  }
}

void main() {
  try {
    readData();
  } catch (e) {
    print('上层捕获异常：$e');
  }
}
```

输出结果：

```text
记录异常：FormatException: ...
上层捕获异常：FormatException: ...
```

**区别：**
- `throw`：主动抛出一个异常，也可以重新抛出已有异常对象。
- `rethrow`：只能在 `catch` 中使用，用于重新抛出当前捕获的异常，并保留原始堆栈信息。

## 6.5 异常与异步联合使用

在 Dart 中，异步操作（例如网络请求）可能发生异常。可以结合 `async`、`await` 和 `try-catch` 捕获并处理异常。

下面以 Flutter 网络请求为例：

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<void> fetchData() async {
  try {
    // 发送网络请求，并等待响应
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users/1'),
    );

    // 判断 HTTP 状态码
    if (response.statusCode == 200) {
      print('请求成功：${response.body}');
    } else {
      print('请求失败，状态码：${response.statusCode}');
    }
  } catch (e) {
    // 捕获网络请求过程中产生的异常
    print('发生异常：$e');
  }
}

void main() {
  fetchData();
}
```

**说明：**

- `async`：声明异步函数。
- `await`：等待网络请求完成，再继续执行后续代码。
- `try-catch`：捕获网络请求过程中产生的异常，例如网络连接失败、请求超时等。
- `statusCode`：HTTP 响应状态码，通常 `200` 表示请求成功。

**注意：** HTTP 404、500 等错误状态码本身不会自动触发 `catch`，需要通过 `statusCode` 自行判断；`catch` 主要用于捕获请求过程中抛出的异常。

`http` 库需要先添加依赖：

```bash
flutter pub add http
```