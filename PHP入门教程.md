# 1. 基础入门阶段：PHP 地基工程

欢迎来到 PHP 的世界！PHP 是一种运行在**服务器端**的脚本语言，它是构建动态网站（如 WordPress、维基百科）的顶梁柱。

## 1.1 环境搭建：你的“练功房”
PHP 代码不能直接在浏览器里运行，它需要一个服务器环境。
* **推荐工具**：下载 **phpStudy (小皮面板)** 或 **XAMPP**。
* **操作**：安装后点击“启动” Apache 和 MySQL，将你的 `.php` 文件放在 `www` 或 `htdocs` 目录下，通过浏览器访问 `localhost/文件名.php` 即可。


## 1.2 基础语法：规矩与招呼
PHP 脚本就像一封信，需要特定的开头和结尾。

```php
<?php
// 1. 脚本标记：这是 PHP 的“大门”。所有的 PHP 代码都要写在这里面。
// 浏览器看不到这些标记，只会看到代码执行后的结果。

/*
   2. 注释：代码的说明书
   这是多行注释，可以写很长的逻辑说明。
   注释不会被执行，是写给人类看的。
*/

# 这也是单行注释（类似 Unix 风格），但 // 更常用。

// 3. 输出指令：跟世界打个招呼
echo "你好，PHP！"; // echo 是最常用的输出方式，速度快，支持输出多个字符串。
print "准备开始学习。"; // print 也有同样功能，但它有返回值（总是1），稍慢一点。

// 4. 技巧：在 HTML 中嵌入 PHP
?>
<h1><?php echo "我是嵌入在 HTML 里的 PHP"; ?></h1>
```

`echo` 虽然好用，**但它不能打印数组或对象**, 此时，可用 `json_encode()`
```php
<?php
$colors = ["红色", "蓝色"];

// 把数组转成 JSON 字符串再 echo
echo json_encode($colors, JSON_UNESCAPED_UNICODE);
// 输出：["红色","蓝色"]
?>
```

**PHP 中最容易踩的坑：双引号会翻译变量，单引号只当它是普通字符。**

```php
<?php
$fruit = "苹果";

echo "我爱吃 $fruit";  // 输出：我爱吃 苹果
echo '我爱吃 $fruit';  // 输出：我爱吃 $fruit （变量没被翻译！）
?>
```

## 1.3 变量与常量

### 1.3.1 变量 (Variables)
PHP 的变量非常灵活，你不需要告诉它这个盒子装的是数字还是文字（动态类型）。

```php
<?php
// 变量必须以 $ 符号开头
$name = "张三";     // 字符串类型
$age = 25;          // 整型
$is_student = true; // 布尔类型（真或假）

// 动态类型：你可以随时改变盒子里装的东西
$data = 100;
$data = "现在我变成了字符串"; 

// 变量拼接：使用“点”号 (.)
echo "姓名：" . $name . "，年龄：" . $age; 
?>
```

### 1.3.2 常量 (Constants)
常量一旦定义，在脚本执行过程中就**不能被修改**。

```php
<?php
// 方式 1：使用 define() 函数（全局有效）
define("PI", 3.14159);

// 方式 2：使用 const 关键字（通常用于类定义，但现在也常用于普通定义）
const APP_NAME = "我的第一个PHP应用";

echo PI;       // 输出 3.14159
// PI = 3.14;  // 报错！常量不能重新赋值
?>
```


## 1.4 数据类型：认清你的“货物”
了解数据类型，才能决定如何处理它们。

### 1.4.1 示例代码

```php
<?php
$str = "Hello";         // String（字符串）：用单引号或双引号包裹
$int = 123;             // Integer（整型）：整数
$float = 12.34;         // Float（浮点型）：小数
$bool = true;           // Boolean（布尔型）：只有 true 或 false
$null = NULL;           // NULL：表示变量没有值

// Array（数组）：一个变量存多个值
$colors = array("红色", "蓝色", "绿色");
// 或者简写：
$fruits = ["苹果", "香蕉"];

// var_dump() 是小白的神器，它可以告诉你变量的“详细身世”（类型和值）
var_dump($colors); 
?>
```

### 1.4.2 `var_dump()`
当你通过浏览器访问 `http://localhost/test.php` 时：
* `var_dump()` 的结果会直接出现在 **网页的正文里**。
* 它会将变量的类型、长度、具体内容统统打印出来，帮助你调试。

**示例代码：**
```php
<?php
$colors = ["红色", "蓝色", "绿色"];

// 浏览器会直接在页面左上角显示这些内容
var_dump($colors); 
?>
```

**浏览器看到的画面：**
> `array(3) { [0]=> string(6) "红色" [1]=> string(6) "蓝色" [2]=> string(6) "绿色" }`


## 1.5 运算符：数据的加工厂
没有运算，代码就只是静态的文字。

### 1.5.1 算术与赋值
```php
<?php
$a = 10;
$b = 3;

echo $a + $b; // 加：13
echo $a % $b; // 取模（余数）：1 (10除以3余1)

$a += 5;      // 等同于 $a = $a + 5; (现在 $a 是 15)
?>
```

### 1.5.2 比较与逻辑
这是程序做决策（判断）的基础。

```php
<?php
$x = 10;
$y = "10";

// 比较运算符
var_dump($x == $y);  // 等于：true（只比值，不比类型）
var_dump($x === $y); // 全等于：false（值相同，但类型不同：一个是整型，一个是字符串）
var_dump($x != 20);  // 不等于：true

// 逻辑运算符
$is_admin = true;
$has_permission = false;

// AND (&&): 两者都为真才为真
var_dump($is_admin && $has_permission); // false

// OR (||): 只要有一个为真就为真
var_dump($is_admin || $has_permission); // true

// 三元运算符：简单的“如果...就...”
// 语法：(条件) ? 结果A : 结果B
$status = ($x > 5) ? "比5大" : "比5小";
echo $status; // 输出：比5大
?>
```

## 1.6 给小白的特别提醒：
1. **分号**：每一行代码结尾一定要加 `;`，否则 PHP 会罢工并报错。
2. **区分大小写**：变量名 `$name` 和 `$Name` 是两个不同的变量！
3. **报错信息**：如果运行出来是白屏，请检查你的开发环境是否开启了 `display_errors`，报错信息是你进步最快的老师。


# 2. 流程控制

## 2.1 条件判断：程序的“决策者”

条件判断让代码具备了思考能力。PHP 根据表达式的真（`true`）或假（`false`）来决定执行哪一段代码。

### 2.1.1 if...else 系列
这是最常用的逻辑分支。

```php
<?php
$score = 85;

if ($score >= 90) {
    echo "优秀！拿奖学金了。";
} elseif ($score >= 60) {
    // 如果上面的条件不满足，检查这个
    echo "及格，再接再厉。";
} else {
    // 以上都不满足时执行
    echo "不及格，准备补考。";
}
?>
```

### 2.1.2 switch 语句
当你有很多种固定的情况（比如根据星期几输出不同内容）时，`switch` 比多个 `if` 更整洁。

```php
<?php
$day = "Monday";

switch ($day) {
    case "Monday":
        echo "开启忙碌的一周。";
        break; // 必须加 break，否则会继续执行下面的 case
    case "Friday":
        echo "周五啦，准备放假！";
        break;
    default:
        echo "普通的打工日。";
}
?>
```

## 2.2 循环控制：重复的力量

循环可以帮你处理大量重复的任务，比如打印 100 遍“我不玩游戏了”。

### 2.2.1 while 与 do...while
* `while`：先判断，后执行。
* `do...while`：先执行一次，再判断（保证代码至少运行一次）。

```php
<?php
$i = 1;
while ($i <= 3) {
    echo "这是第 $i 次循环<br>";
    $i++; // 记得更新计数器，否则会死循环！
}

$j = 10;
do {
    echo "由于是 do...while，我至少会跑一次。";
} while ($j < 5); // 条件为假，循环停止
?>
```

### 2.2.2 for 循环
当你明确知道要跑多少次时，`for` 是最佳选择。

```php
<?php
// (初始化; 检查条件; 步进)
for ($i = 0; $i < 5; $i++) {
    echo "当前计数：$i <br>";
}
?>
```

### 2.2.3 foreach 遍历（PHP 的灵魂）
在 PHP 中，数组非常常用，而 `foreach` 是遍历数组最简单、最安全的方式。

```php
<?php
$fruits = ["苹果", "香蕉", "橙子"];

// 语法 1：只获取值
foreach ($fruits as $fruit) {
    echo "水果名字：$fruit <br>";
}

// 语法 2：同时获取键（下标）和值
$user_ages = ["张三" => 20, "李四" => 25];
foreach ($user_ages as $name => $age) {
    echo "$name 的年龄是 $age 岁。<br>";
}
?>
```

## 2.3 函数：代码的“复用器”

函数是一段被命名的、可以反复调用的代码块。

### 2.3.1 自定义函数与参数传递

```php
<?php
/**
 * 示例：计算矩形面积
 * @param int $width 宽度 (值传递)
 * @param int &$height 高度 (引用传递，前面带 &)
 */
function calculateArea($width, $height) {
    return $width * $height;
}

echo "面积是：" . calculateArea(10, 5);

// --- 值传递 vs 引用传递 ---
$num = 10;
function addFive($n) { $n += 5; }      // 只是改了副本
function addTen(&$n) { $n += 10; }     // 直接改了原本的变量

addFive($num);
echo "值传递后：$num"; // 还是 10

addTen($num);
echo "引用传递后：$num"; // 变成了 20
?>
```

### 2.3.2 变量作用域 (Scope)

```php
<?php
$outer = "外部变量"; // 全局作用域

function testScope() {
    $inner = "内部变量"; // 局部作用域
    
    // 1. 如果想在函数内访问外部变量，需要 global 关键字
    global $outer;
    echo $outer;

    // 2. static 静态变量：函数执行完后，它的值不会消失
    static $count = 0;
    $count++;
    echo "该函数被调用了 $count 次。";
}

testScope(); // 输出 1
testScope(); // 输出 2
?>
```


## 2.4 超级全局变量：无处不在的助手

PHP 内置了一些特殊的变量，它们在任何地方（函数内、文件间）都可以直接访问。

| 变量名 | 用途 |
| :--- | :--- |
| `$_GET` | 收集浏览器 URL 传来的参数（如 `?id=1`） |
| `$_POST` | 收集 HTML 表单提交的数据（更安全，对用户不可见） |
| `$_REQUEST` | 包含了 GET 和 POST 的合集 |
| `$_SERVER` | 包含服务器环境信息（如 IP 地址、路径等） |
| `$GLOBALS` | 包含程序中所有的全局变量 |

**代码示例：处理一个简单的表单**

```php
<?php
// 假设用户访问了 test.php?name=Tom
echo "URL 中的名字是：" . $_GET['name'];

// 获取用户的 IP 地址
echo "你的 IP 是：" . $_SERVER['REMOTE_ADDR'];

// 获取所有的全局变量（谨慎输出，内容会很多）
// print_r($GLOBALS);
?>
```


## 2.5 小贴士：
1.  **分号**：PHP 每一行代码结束必须加 `;`，这是新手最容易犯的错。
2.  **报错**：如果页面一片空白，检查你的代码逻辑是否有死循环（比如 `while` 忘了写 `$i++`）。
3.  **引用传递**：除非你真的想在函数内部修改外部变量的值，否则请默认使用“值传递”。


# 3. 数组与字符串处理
PHP 最大的特点就是对数组和字符串的强大处理能力。

## 3.1 数组操作：PHP 的万能容器

PHP 的数组非常灵活，它既可以是简单的列表，也可以是复杂的键值对映射。

### 3.1.1 三种数组类型

```php
<?php
// 1. 数值数组（自动编号，index 从 0 开始）
$colors = ["Red", "Green", "Blue"];
echo $colors[0]; // 输出: Red

// 2. 关联数组（自定义键名，类似 JSON）
$user = [
    "id" => 101,
    "name" => "Gemini",
    "role" => "AI"
];
echo $user["name"]; // 输出: Gemini

// 3. 多维数组（嵌套数组）
$data = [
    ["name" => "张三", "score" => 90],
    ["name" => "李四", "score" => 85]
];
echo $data[0]["name"]; // 输出: 张三
?>
```

### 3.1.2 常用数组函数

```php
<?php
$fruits = ["Apple", "Banana"];

// count(): 获取数组长度
echo count($fruits); // 输出: 2

// array_push(): 在末尾添加元素
array_push($fruits, "Orange", "Grape");

// sort(): 对数组进行升序排序（注意：会改变原数组）
sort($fruits);

// array_merge(): 合并两个或多个数组
$more_fruits = ["Berry", "Mango"];
$all_fruits = array_merge($fruits, $more_fruits);

/**
 * 💡 小白笔记：
 * 关联数组排序要用 asort() (按值排序) 或 ksort() (按键排序)
 * 否则你会丢失原本的键名。
 */
?>
```


## 3.2 字符串处理：玩转文字

字符串是 PHP 处理最频繁的数据，熟练使用内置函数能让你少写很多 `if...else`。

### 3.2.1 基础操作与并置

```php
<?php
$firstName = "Hello";
$lastName = "World";

// 使用 "." 运算符连接字符串（注意：PHP 不用 + 连接字符串）
$full = $firstName . " " . $lastName; 

// 双引号内可以直接解析变量，单引号则原样输出
echo "消息是：$full"; // 输出: 消息是：Hello World
echo '变量名是：$full'; // 输出: 变量名是：$full
?>
```

### 3.2.2 常用字符串函数

```php
<?php
$str = "  Hello PHP World!  ";

// 1. trim(): 去除两端空格（常用于处理表单输入）
$cleanStr = trim($str); // "Hello PHP World!"

// 2. strlen(): 获取字符串长度
echo strlen($cleanStr); // 输出: 17

// 3. strpos(): 查找子字符串首次出现的位置
$pos = strpos($cleanStr, "PHP"); // 返回 6 (从0开始数)

// 4. explode(): 将字符串拆分为数组（类似切割机）
$parts = explode(" ", $cleanStr); 
// 结果: ["Hello", "PHP", "World!"]

// 5. implode(): 将数组元素组合为字符串（类似粘合剂）
$newStr = implode("-", $parts);
// 结果: "Hello-PHP-World!"
?>
```


## 3.3 正则表达式：文本搜索的神兵利器

正则表达式（Regular Expression）用于定义一种搜索模式。在 PHP 中，它们通常以 `/` 开头和结尾。

### 3.3.1 常用正则函数

```php
<?php
$email = "test@example.com";
$pattern = "/^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+\.[a-zA-Z0-9_-]+$/";

// 1. preg_match(): 执行匹配检查
if (preg_match($pattern, $email)) {
    echo "这是一个有效的邮箱格式。";
} else {
    echo "邮箱格式错误。";
}

// 2. preg_replace(): 执行搜索和替换
$text = "Visit Microsoft!";
$newText = preg_replace("/Microsoft/i", "PHP.net", $text);
// "i" 表示忽略大小写
echo $newText; // 输出: Visit PHP.net!
?>
```


### 3.3.2 综合示例：处理用户标签
假设你有一个从数据库拿到的标签字符串 `" 编程, 学习 , PHP, Web "`，你想把它转成整洁的数组：

```php
<?php
$rawTags = " 编程, 学习 , PHP, Web ";

// 第一步：先按逗号切割成数组
$tagArray = explode(",", $rawTags);

// 第二步：利用 array_map 循环对每个元素执行 trim 去空格
$cleanTags = array_map('trim', $tagArray);

print_r($cleanTags);
/*
输出：
Array (
    [0] => 编程
    [1] => 学习
    [2] => PHP
    [3] => Web
)
*/
?>
```

## 3.4 学习建议：
* **数组**：重点练习 `foreach` 配合数组函数的用法，这在显示数据库列表数据时是必经之路。
* **字符串**：多练习 `explode` 和 `implode`，它们在处理 URL 路径或格式化数据时极其强大。
* **正则**：正则很深，初学者先学会常用的“邮箱验证”、“手机号验证”模板即可，不需要死记硬背复杂的规则。



# 4. Web 交互与数据库
这是 PHP 最常用的场景：处理表单数据并存入数据库。

## 4.1 表单处理：GET vs POST

当你在网页上点击“提交”按钮时，数据通过这两种方式传给 PHP。

### 4.1.1 GET 与 POST 的区别
| 特性 | GET | POST |
| :--- | :--- | :--- |
| **数据位置** | 附在 URL 后面 (`?id=1`) | 包含在 HTTP 请求体中 |
| **安全性** | 较低（数据在历史记录中可见） | 较高（适合密码、敏感信息） |
| **容量限制** | 有限制（通常约 2KB） | 无限制（适合上传文件） |
| **场景** | 搜索、分页、查看文章 | 注册、登录、发表评论 |

### 4.1.2 安全过滤（防止 XSS 攻击）
**永远不要信任用户的输入！** 在显示用户提交的内容前，必须过滤。

```php
<?php
// 假设表单提交了一个名为 "username" 的字段
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // 1. 过滤 HTML 标签，防止恶意脚本执行 (XSS)
    $user = htmlspecialchars($_POST['username']);
    
    // 2. 简单的去空格
    $user = trim($user);

    echo "你好，" . $user;
}
?>
```


## 4.2 会话管理：Cookie 与 Session

Web 开发是“无状态”的（服务器记不住你是谁）。为了维持登录状态，我们需要这两种技术。

### 4.2.1 Cookie（存放在用户浏览器）
适合存储一些非敏感、长期保存的数据（如“记住用户名”）。

```php
<?php
// 设置一个 Cookie，有效期 1 小时 (3600秒)
setcookie("user_theme", "dark", time() + 3600, "/");

// 读取 Cookie
if(isset($_COOKIE["user_theme"])) {
    echo "你选择的主题是：" . $_COOKIE["user_theme"];
}
?>
```

### 4.2.2 Session（存放在服务器，更安全）
适合存储敏感信息，如“是否已登录”。

```php
<?php
session_start(); // 使用 Session 前必须先开启！

// 模拟登录成功，存储用户 ID
$_SESSION['user_id'] = 42;
$_SESSION['username'] = 'Admin';

// 在另一个页面读取
echo "欢迎回来，" . $_SESSION['username'];

// 退出登录：清空 Session
// session_destroy(); 
?>
```


## 4.3 文件上传

PHP 处理文件上传分为两步：文件先传到**临时目录**，你再把它移动到**正式目录**。

```php
<?php
if ($_FILES['my_file']['error'] === UPLOAD_ERR_OK) {
    $temp_name = $_FILES['my_file']['tmp_name']; // 临时文件名
    $dest_name = "uploads/" . $_FILES['my_file']['name']; // 目标路径
    
    // 将文件从临时位置移动到你的文件夹
    if (move_uploaded_file($temp_name, $dest_name)) {
        echo "上传成功！";
    }
}
?>
```


## 4.4 MySQL 数据库交互（核心）

推荐使用 **PDO (PHP Data Objects)**。它更安全，且支持多种数据库（MySQL, PostgreSQL 等）。

### 4.4.1 连接数据库与 CRUD


```php
<?php
$host = 'localhost';
$db   = 'my_test_db';
$user = 'root';
$pass = 'root';
$charset = 'utf8mb4';

$dsn = "mysql:host=$host;dbname=$db;charset=$charset";

try {
    // 创建 PDO 实例
    $pdo = new PDO($dsn, $user, $pass);
    
    // --- 1. 插入数据 (C) ---
    // 使用“预处理语句”防止 SQL 注入！不要直接在 SQL 里写变量
    $sql = "INSERT INTO users (username, email) VALUES (?, ?)";
    $stmt = $pdo->prepare($sql);
    $stmt->execute(['Alice', 'alice@example.com']);

    // --- 2. 查询数据 (R) ---
    $stmt = $pdo->query("SELECT * FROM users");
    while ($row = $stmt->fetch()) {
        echo $row['username'] . "<br>";
    }

    // --- 3. 更新数据 (U) ---
    $sql = "UPDATE users SET email = :email WHERE username = :name";
    $stmt = $pdo->prepare($sql);
    $stmt->execute(['email' => 'new_alice@example.com', 'name' => 'Alice']);

} catch (PDOException $e) {
    echo "连接失败: " . $e->getMessage();
}
?>
```

### 4.4.2 关键：如何预防 SQL 注入？
**SQL 注入**是 Web 安全的头号杀手。黑客通过在表单输入 `1 OR 1=1` 来骗过数据库。

* **错误写法**（会被攻击）：
    `$pdo->query("SELECT * FROM users WHERE name = '$user_input'");`
* **正确写法**（使用占位符 `?`）：
    `$stmt = $pdo->prepare("SELECT * FROM users WHERE name = ?");`
    `$stmt->execute([$user_input]);`


## 4.5 阶段性总结：
1.  **安全第一**：处理表单用 `htmlspecialchars()`，操作数据库用 **预处理语句 (Prepared Statements)**。
2.  **状态保持**：登录功能一定要用 `Session`，而不是只靠 `Cookie`。
3.  **PDO**：它是现代 PHP 开发的标配，学会 PDO 就不用担心切换数据库的问题了。



# 5. 进阶与高级特性

## 5.0 `->` 和 `::`
这两个符号是 PHP 面向对象编程（OOP）中最基础也最容易混淆的工具。简单来说：**`->` 是“对象的钥匙”，而 `::` 是“类的地址”。**

### 5.0.1 `->` 对象运算符 (Object Operator)

当你通过 `new` 关键字创建了一个具体的**实例（对象）** 后，你就要用 `->` 来访问这个**实例（对象）** 的属性或方法。

* **使用场景**：访问非静态（Non-static）的属性和方法。
* **记忆点**：必须先有 `new` 出来的活生生的个体。

```php
<?php
class Cat {
    public $name = "小咪";

    public function meow() {
        echo "喵喵喵~";
    }
}

// 1. 实例化：产生一个具体的对象
$myCat = new Cat();

// 2. 使用 -> 访问属性和方法
echo $myCat->name;  // 输出：小咪
$myCat->meow();     // 输出：喵喵喵~
?>
```

### 5.0.2 `::` 范围解析操作符 (Scope Resolution Operator)

这个符号（也叫双冒号）用于直接访问**类本身**的成员，或者在类定义内部引用。你不需要（通常也不能）通过 `new` 实例化对象来使用它。

* **使用场景**：
    1.  访问 **静态属性/静态方法** (`static`)。
    2.  访问 **类常量** (`const`)。
    3.  在子类中调用父类的方法 (`parent::`)。

```php
<?php
class Calculator {
    // 类常量：全类通用，不可更改
    // 类常量没写修饰符，默认为 public
    const PI = 3.14159;

    // 静态属性：属于类，不属于某个对象
    public static $count = 0;

    // 静态方法
    public static function add($a, $b) {
        return $a + $b;
    }
}

// 无需 new，直接通过类名访问
echo Calculator::PI;          // 输出：3.14159
echo Calculator::add(5, 10);  // 输出：15
echo Calculator::$count;      // 输出：0
?>
```


### 5.0.3 核心区别对照表

| 特性 | `->` (箭头) | `::` (双冒号) |
| :--- | :--- | :--- |
| **术语** | 对象运算符 | 范围解析操作符 |
| **操作对象** | **实例** (Object) | **类** (Class) |
| **前提条件** | 必须先 `$obj = new Class()` | 直接使用 `ClassName::` |
| **访问内容** | 普通变量 `$this->prop` | 静态变量 `self::$prop` |
| **访问常量** | 不可以 | 可以 (`ClassName::CONST`) |



### 5.0.4 特殊用法：`$this` vs `self`

在类的方法内部编写逻辑时，这两个词经常配合上述符号出现：

* **`$this->`**：指向“当前这个对象”。用于访问自己的普通属性。
* **`self::`**：指向“当前这个类”。用于访问自己的静态属性或常量。

```php
<?php
class User {
    public $nickname;
    public static $userCount = 0;

    public function __construct($name) {
        $this->nickname = $name; // 设置当前对象的昵称
        self::$userCount++;      // 增加类全局的计数器
    }
}
?>
```

### 5.0.5 小白避坑指南
1.  **静态变量要带 `$`**：用 `::` 访问静态变量时，`$` 符号不能省，例如 `User::$count`；但访问普通属性用 `->` 时，属性名前面**不加** `$`，例如 `$user->nickname`。
2.  **报错提示**：如果你看到 "Using $this when not in object context"，说明你在 `static` 静态方法里误用了 `$this->`。记住：**静态方法里没有 `$this`**。



## 5.1 面向对象编程 (OOP)：构建你的世界

如果说之前的代码是“按步骤做事”，那么 **OOP** 就是“按角色做事”。你不再是写一段代码来修车，而是先定义一个“车”类，然后让这个“车”对象自己跑起来。

### 5.1.1 类与对象的基础

```php
<?php
// 1. 定义一个类 (类是模板)
class SmartPhone {
    // 属性 (变量)
    public $brand; // 公有的：外部可以直接访问
    protected $os; // 受保护的：只有自己和子类能访问
    private $battery; // 私有的：只有自己内部能访问

    // 2. 构造函数 (创建对象时自动执行)
    public function __construct($brand, $os) {
        $this->brand = $brand;
        $this->os = $os;
        $this->battery = 100; // 新手机电量满格
        echo "一部新的 {$this->brand} 手机已出厂！<br>";
    }

    // 方法 (函数)
    public function showInfo() {
        echo "品牌: " . $this->brand . " | 系统: " . $this->os . " | 电量: " . $this->battery . "%<br>";
    }
}

// 3. 实例化对象
$myPhone = new SmartPhone("Apple", "iOS");
$myPhone->showInfo();
?>
```

### 5.1.2 继承与访问控制
继承让你可以在现有类的基础上扩展功能，而不需要重写代码。



```php
<?php
// 继承 SmartPhone 类
class GamingPhone extends SmartPhone {
    public $gpu;

    public function __construct($brand, $os, $gpu) {
        // 调用父类的构造函数
        parent::__construct($brand, $os);
        $this->gpu = $gpu;
    }

    public function playGames() {
        // 可以访问父类的 protected 属性 $os
        echo "正在使用 {$this->os} 系统和 {$this->gpu} 显卡玩游戏...<br>";
    }
}

$proPhone = new GamingPhone("ASUS", "Android", "RTX 4090 Mobile");
$proPhone->playGames();
?>
```


## 5.2 错误与异常处理：程序的“安全气囊”

代码总会遇到意外（如数据库连接失败、文件找不到）。`try...catch` 块能防止程序崩溃并给用户友好的提示。

```php
<?php
function divide($dividend, $divisor) {
    if ($divisor == 0) {
        // 主动抛出一个异常
        throw new Exception("除数不能为零！");
    }
    return $dividend / $divisor;
}

try {
    // 尝试执行可能出错的代码
    echo divide(10, 2) . "<br>";
    echo divide(10, 0) . "<br>"; 
} catch (Exception $e) {
    // 如果出错，捕获异常并处理
    echo "捕获到错误: " . $e->getMessage() . "<br>";
} finally {
    // 无论是否出错都会执行的代码（可选）
    echo "清理资源中...<br>";
}
?>
```


## 5.3 JSON 处理：数据的“通用语言”

我们在上一章提到过 JSON。在现代开发中，PHP 经常作为 API 后端，给前端（Flutter/Vue/React）发送数据。

```php
<?php
// --- 数组转 JSON (发送给前端) ---
$userData = [
    "status" => "success",
    "data" => [
        "id" => 1,
        "name" => "小明"
    ]
];
// json_encode: 编码
echo json_encode($userData, JSON_UNESCAPED_UNICODE); 
// 输出: {"status":"success","data":{"id":1,"name":"小明"}}

// --- JSON 转 数组 (接收外部数据) ---
$jsonInput = '{"city":"北京","weather":"晴"}';
// json_decode: 解码 (第二个参数为 true 转为数组)
$result = json_decode($jsonInput, true);
echo "城市: " . $result['city'];
?>
```


## 5.4 命名空间 (Namespace)：代码的“文件夹”

当你的项目变大，引入了很多第三方库时，可能会有两个类都叫 `User`。命名空间就像是文件夹，用来区分它们。

```php
<?php
// 定义在商城模块下
namespace App\Shop;

class User {
    public function say() {
        echo "我是商城用户。";
    }
}

// --- 在另一个文件中使用 ---
// use App\Shop\User; 
// $user = new User();
?>
```

## 5.5 综合应用：一个简单的 OOP 用户管理逻辑

让我们把这些概念串联起来：

```php
<?php
namespace App\Service;

class UserAdmin {
    private $users = [];

    // 模拟添加用户
    public function addUser($name) {
        if (empty($name)) {
            throw new \Exception("用户名不能为空");
        }
        $this->users[] = $name;
    }

    // 获取所有用户的 JSON 格式
    public function getUserJson() {
        return json_encode($this->users);
    }
}

// 使用
try {
    $admin = new UserAdmin();
    $admin->addUser("张三");
    $admin->addUser("李四");
    echo "当前用户列表: " . $admin->getUserJson();
} catch (\Exception $e) {
    echo "操作失败: " . $e->getMessage();
}
?>
```

## 5.6 核心建议：
1.  **OOP 思路**：刚开始你会觉得 OOP 很繁琐，但当你需要管理几十个文件时，你会感谢类和继承带来的整洁。
2.  **异常捕获**：在处理网络请求、数据库操作、文件读写时，**务必**使用 `try...catch`。
3.  **JSON**：记住 `json_encode` 是 PHP 给世界递名片的手段，学会它你就能开发 API 了。