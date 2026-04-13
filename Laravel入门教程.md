# 0. 前言
Laravel 是一个流行的开源 PHP Web 开发框架，由 Taylor Otwell 创建，主打“优雅语法 + 高开发效率”。

## 0.1 🧩 一句话理解

Laravel = **帮你快速开发网站/接口的 PHP 框架**，把常见功能都帮你封装好了。


## 0.2 核心特点

### 0.2.1. MVC 架构

* **Model（模型）**：处理数据（数据库）
* **View（视图）**：页面展示
* **Controller（控制器）**：业务逻辑
  👉 结构清晰，方便维护


### 0.2.2. 强大的 ORM（Eloquent）

* 用“面向对象”方式操作数据库

```php
User::where('id', 1)->first();
```

👉 不用手写 SQL 也能操作数据库


### 0.2.3 路由系统简单

```php
Route::get('/user', function () {
    return 'Hello User';
});
```

👉 URL 和逻辑绑定非常直观


### 0.2.4 内置功能丰富

开箱即用：

* 用户认证（登录注册）
* 队列（Queue）
* 缓存（Cache）
* 文件上传
* API 开发（RESTful）


### 0.2.5 Artisan 命令行工具

```bash
php artisan make:controller UserController
```

👉 自动生成代码，提升效率


### 0.2.6 Blade 模板引擎

```blade
{{ $name }}
```

👉 写页面更简洁、安全


## 0.3 适合做什么

* 后端 API（比如给 Flutter 用 👍）
* 管理系统（CMS、后台）
* 电商网站
* 博客系统


## 0.4 学习建议（结合你当前情况）

你现在：

* ✅ 会 Flutter
* ✅ 正在做 Django + DRF
* ✅ 想尝试 Laravel

👉 推荐路线：

1. 先用 Laravel 做 **简单 API**
2. 用 Flutter 调接口（你已经会）
3. 对比 Django 和 Laravel 的差异


## 0.5 简单对比

| 框架      | 语言     | 风格      |
| ------- | ------ | ------- |
| Django  | Python | 强规范     |
| Laravel | PHP    | 灵活 + 优雅 |


# 1. 环境搭建与基础概念

在正式写代码之前，我们要先把“地基”打好，否则后面会各种报错、迷路。

## 1.1 开发环境安装

Laravel 运行需要三个核心东西：

* PHP（>= 8.1）
* Composer（依赖管理）
* Web 服务环境（Nginx / Apache / 内置服务器）



### 1.1.1 方案一：使用 Laravel Herd（推荐新手 ⭐）

👉 **优点：极其简单，一键启动**

#### 1.1.1.1 安装步骤：

1. 下载 **Laravel Herd**
2. 安装完成后，它会自动帮你：

   * 安装 PHP
   * 配置环境变量
   * 启动本地服务器



#### 1.1.1.2 创建 Laravel 项目

```bash
composer create-project laravel/laravel my_project
```

👉 解释：

```bash
composer                # 使用 Composer 工具
create-project          # 创建项目
laravel/laravel         # Laravel 官方模板
my_project              # 项目名称（文件夹名）
```



#### 1.1.1.3 启动项目

```bash
cd my_project
php artisan serve
```

打开浏览器：

```
http://127.0.0.1:8000
```

看到 Laravel 欢迎页面 = 成功 🎉



### 1.1.2 方案二：Docker（Laravel Sail）

👉 **适合想学后端工程化的人**



#### 1.1.2.1 初始化 Sail

```bash
curl -s "https://laravel.build/my_project" | bash
cd my_project
./vendor/bin/sail up
```



#### 1.1.2.2 运行 Artisan

```bash
./vendor/bin/sail artisan list
```

👉 注意：Docker 里必须用 `sail artisan`



### 1.1.3 Composer 是什么？

👉 PHP 世界的“npm”



#### 1.1.3.1 常用命令：

```bash
composer install     # 安装依赖
composer update      # 更新依赖
composer require xxx # 安装新包
```



## 1.2 目录结构详解（重点！！！🔥）

Laravel 项目结构非常清晰，必须掌握。

```
your-project/
├── app/                # 【核心逻辑】控制台、模型、控制器都在这
│   ├── Http/           # 
│   │   └── Controllers/# ← 控制器文件（处理业务逻辑）
│   └── Models/         # ← 模型文件（对应数据库表）
├── config/             # 所有配置文件（数据库、邮件、缓存等设置）
├── database/           # 数据库相关
│   ├── migrations/     # ← 数据库迁移文件（用代码建表）
│   └── seeders/        # 填充测试数据的地方
├── public/             # 外部访问入口（入口文件 index.php 和 静态资源）
├── resources/          # 
│   └── views/          # ← 视图文件（写 HTML/Blade 模板的地方）
├── routes/             # 【路由】
│   ├── api.php         # ← 定义 API 接口路由
│   └── web.php         # ← 定义普通网页路由
├── storage/            # 存放日志、缓存、上传的文件（系统自动写入）
├── .env                # 【核心配置】数据库密码、密钥全写在这里
└── artisan             # 命令行工具（比如 php artisan make:...）
```

### 1.2.1 📁 app/ —— 核心代码区

👉 你写的“业务逻辑”基本都在这里


#### 1.2.1.1 示例结构：

```
app/
 ├── Models/
 ├── Http/
 │    ├── Controllers/
 │    └── Middleware/
```



#### 1.2.1.2 示例：创建一个控制器

```bash
php artisan make:controller UserController
```

生成：

```
app/Http/Controllers/UserController.php
```



#### 1.2.1.3 控制器代码示例（带注释）

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * 显示用户列表
     */
    public function index()
    {
        // 模拟数据（实际来自数据库）
        $users = [
            ['id' => 1, 'name' => 'Tom'],
            ['id' => 2, 'name' => 'Jerry'],
        ];

        // 返回 JSON 数据
        return response()->json($users);
    }

    /**
     * 显示单个用户
     */
    public function show($id)
    {
        return "当前用户ID是：" . $id;
    }
}
```



### 1.2.2 📁 routes/ —— 路由（入口）

👉 所有请求从这里进来！



#### 1.2.2.1 关键文件：

```
routes/web.php   # 浏览器访问
routes/api.php   # API 接口
```



#### 1.2.2.2 示例：定义路由

```php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\UserController;

/**
 * 基础路由
 */
Route::get('/', function () {
    return 'Hello Laravel!';
});

/**
 * 控制器路由
 */
Route::get('/users', [UserController::class, 'index']);

/**
 * 带参数的路由
 */
Route::get('/users/{id}', [UserController::class, 'show']);
```



#### 1.2.2.3 路由解释：

```php
Route::get('/users', ...)  
// GET 请求
// URL: /users

Route::get('/users/{id}', ...)
// {id} 是动态参数
```



### 1.2.3 📁 resources/ —— 视图与前端资源

👉 用来写页面（Blade 模板）



#### 1.2.3.1 示例结构：

```
resources/
 ├── views/
 │    ├── welcome.blade.php
```



#### 1.2.3.2 Blade 示例：

```html
<!-- resources/views/user.blade.php -->

<!DOCTYPE html>
<html>
<head>
    <title>用户页面</title>
</head>
<body>

<h1>用户列表</h1>

<ul>
@foreach ($users as $user)
    <li>{{ $user['name'] }}</li>
@endforeach
</ul>

</body>
</html>
```



#### 1.2.3.3 控制器返回视图：

```php
public function index()
{
    $users = [
        ['name' => 'Tom'],
        ['name' => 'Jerry']
    ];

    // 返回 Blade 视图
    return view('user', ['users' => $users]);
}
```



## 1.3 Artisan 命令行（Laravel 的瑞士军刀 🔧）

👉 Laravel 自带 CLI 工具，超级重要！



### 1.3.1 查看所有命令

```bash
php artisan list
```


### 1.3.2 常用命令（必须会）



#### 1.3.2.1 创建控制器

```bash
php artisan make:controller PostController
```



#### 1.3.2.2 创建模型

```bash
php artisan make:model Post
# 这里的 Post 是模型名称
```


#### 1.3.2.3 创建模型 + 迁移 + 控制器（超常用）

```bash
php artisan make:model Post -mcr
# 或 php artisan make:model Post -mcr
# 针对 JWT/API 开发：php artisan make:model Post -ma
```

👉 `-mcr` 等价于：

* `-m` → migration（生成数据库迁移文件）
* `-c` → controller（生成控制器文件）
* `-r` → resource controller 

👉 `-mc` 等价于：

* `-m` → migration（生成数据库迁移文件）
* `-c` → controller（生成**空的**控制器文件）

👉更推荐的组合（针对 JWT/API 开发）：
```bash
php artisan make:model Post -ma
```
👉 **`-ma` 等价于：**
* **`-m`**：Migration（迁移文件）。
* **`-a`**：**Controller (API)**。它会生成一个“API 资源控制器”，只包含 `index`, `store`, `show`, `update`, `destroy` 这 5 个方法，**去掉了** API 不需要处理的页面跳转方法。

#### 1.3.2.4 生成结果：

```
app/Models/Post.php
database/migrations/xxxx_create_posts_table.php
app/Http/Controllers/PostController.php
```



#### 1.3.2.5 启动服务器
默认端口是 8000
```bash
php artisan serve
```
可以自定义端口
```bash
php artisan serve --port=7890
```

#### 1.3.2.6 数据库迁移

```bash
php artisan migrate
```



#### 1.3.2.7 示例：迁移文件

```php
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();              // 主键
        $table->string('title');   // 标题
        $table->text('content');  // 内容
        $table->timestamps();     // created_at & updated_at
    });
}
```



#### 1.3.2.8 清缓存（开发必备）

```bash
php artisan cache:clear
php artisan route:clear
php artisan config:clear
```


## 1.4 小总结（重点记住）

你现在已经掌握：

✔ Laravel 怎么安装
✔ 项目怎么跑起来
✔ 三大核心目录：

* `app/` 👉 写逻辑
* `routes/` 👉 控制入口
* `resources/` 👉 页面展示

✔ Artisan 常用命令



# 2. Laravel 核心架构 (The Big Three)

这是 Laravel 最核心的三大模块：
👉 路由 + 控制器
👉 数据库 + 模型（Eloquent）
👉 Blade 模板

掌握这三块，基本就能做 80% 的 Web 应用了。

## 2.1 路由与控制器 (Routing & Controllers)

### 2.1.1 路由：代码写在哪里？

在 Laravel 中，路由就像是公司的“前台”。
* **文件位置**：项目根目录下的 `routes/` 文件夹。
* **网页路由**：`routes/web.php`（你在浏览器地址栏输入网址访问的）。
* **接口路由**：`routes/api.php`（给 App 或小程序调用的，**注意：访问时会自动带上 `/api` 前缀**）。


### 2.1.2 路由的“内置语法”总结

路由最常用的语法是 `Route::方法名('路径', 处理逻辑)`。

| 语法方法 | 作用 | 场景举例 |
| :--- | :--- | :--- |
| `Route::get()` | **获取**数据/页面 | 访问首页、查看文章 |
| `Route::post()` | **提交**数据 | 注册账号、发布评论 |
| `Route::put()` | **修改**数据 | 修改个人资料、重置密码 |
| `Route::delete()` | **删除**数据 | 删除某条动态 |

#### 2.1.2.1 语法示例（小白最爱用的闭包写法）：
```php
// 文件路径：routes/api.php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

// 1. 【GET】访问地址：http://localhost/api/about
Route::get('/about', function () {
    // API 通常返回数组，Laravel 会自动转为 JSON
    return [
        'app_name' => '我的第一个 API',
        'version' => '1.0.0'
    ];
});

// 2. 【GET】带必选参数：http://localhost/api/user/10
Route::get('/user/{id}', function ($id) {
    return [
        'message' => "正在查询 ID 为 {$id} 的用户",
        'status' => 'success'
    ];
});

// 3. 【POST】提交数据：通常用工具（如 Postman）模拟访问 http://localhost/api/register
Route::post('/register', function (Request $request) {
    // request()->all() 可以获取用户上传的所有 JSON 数据
    $data = $request->all();
    
    return [
        'message' => '账号创建成功！',
        'user_data' => $data
    ];
});

// 4. 【DELETE】删除数据：访问 http://localhost/api/user/5
Route::delete('/user/{id}', function ($id) {
    return [
        'action' => 'delete',
        'target_id' => $id,
        'result' => '用户已被删除'
    ];
});
```

### 2.1.3 控制器：代码写在哪里？（API 改写版）

由于 API 通常返回 JSON 数据，我们的控制器写法和路由绑定需要稍作调整。

#### 2.1.3.1 第一步：编写 API 控制器逻辑
**文件位置**：`app/Http/Controllers/Api/UserController.php`
*(注：为了整洁，API 的控制器通常放在 Api 文件夹下，但放在根目录下也可以)*

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    // 查看所有用户：GET /api/users
    public function index()
    {
        return [
            ['id' => 1, 'name' => 'Tom'],
            ['id' => 2, 'name' => 'Jack']
        ]; // 直接返回数组，Laravel 自动转 JSON
    }

    // 创建新用户：POST /api/users
    public function store(Request $request)
    {
        // 接收 POST 过来的数据
        $name = $request->input('name');
        
        return [
            'status' => 'success',
            'message' => "用户 {$name} 已创建"
        ];
    }

    // 查看单个用户：GET /api/users/{id}
    public function show($id)
    {
        return [
            'id' => $id,
            'name' => '查询结果',
            'email' => 'user@example.com'
        ];
    }
}
```

#### 2.1.3.2 第二步：在 API 路由里绑定
**文件路径**：`routes/api.php`

```php
use App\Http\Controllers\UserController;
use Illuminate\Support\Facades\Route;

// 1. 获取列表
Route::get('/users', [UserController::class, 'index']);

// 2. 获取详情
Route::get('/users/{id}', [UserController::class, 'show']);

// 3. 提交数据
Route::post('/users', [UserController::class, 'store']);
```


### 2.1.4 常用 API 内置助手速查表

在写 API 时，你不会用到 `view()`（那是返回网页的），你会用到这些：

| 语法 | 作用 | API 示例 |
| :--- | :--- | :--- |
| **`return [数组];`** | 自动转 JSON | `return ['code' => 200];` |
| **`response()->json();`** | 手动控制 JSON 响应 | `return response()->json($data, 201);` // 201 表示创建成功 |
| **`$request->input('key')`** | 获取某个具体的输入值 | `$email = $request->input('email');` |
| **`$request->only(['a','b'])`** | 只接收指定的字段 | `$data = $request->only(['name', 'email']);` |


### 2.1.5 总结口诀（API 版）：

1. **定地址** $\rightarrow$ 去 **`routes/api.php`** 写 `Route::get/post...` (记得访问时加 `/api` 前缀)。
2. **建文件** $\rightarrow$ 运行 **`php artisan make:controller UserController`**。
3. **写逻辑** $\rightarrow$ 去 **`app/Http/Controllers/`** 里的方法直接 `return` 数组。
4. **牵红线** $\rightarrow$ 在路由里用 **`[UserController::class, '方法名']`** 绑定。


### 2.1.6 终极懒人包：API 资源路由

#### 2.1.6.1 第一步：一键生成控制器
在终端输入以下命令：
```bash
# --api 参数会自动帮你生成 index, store, show, update, destroy 这 5 个方法
# 它会跳过网页版才需要的 create 和 edit（因为 API 不需要返回填表页面）
php artisan make:controller UserController --api
```

**文件位置**：`app/Http/Controllers/UserController.php`
你会发现 Laravel 已经帮你把方法名都取好了，你只需要在里面填逻辑。

#### 2.1.6.2 第二步：一键注册路由
打开 **`routes/api.php`**，删掉之前零散的路由，只写这一行：

```php
use App\Http\Controllers\UserController;
use Illuminate\Support\Facades\Route;

// 这一行等于帮你写了 5 行路由！
Route::apiResource('users', UserController::class);
```

### 2.1.7 懒人包生成的“全家桶”对照表

这一行 `apiResource` 到底帮你干了什么？看下表就清楚了：

| 请求方式 | URL 地址 (需加 /api) | 控制器方法 | 语义 |
| :--- | :--- | :--- | :--- |
| **GET** | `/users` | `index` | 获取所有用户列表 |
| **POST** | `/users` | `store` | 创建一个新用户 |
| **GET** | `/users/{user}` | `show` | 获取某个用户的详情 |
| **PUT/PATCH** | `/users/{user}` | `update` | 修改某个用户信息 |
| **DELETE** | `/users/{user}` | `destroy` | 删除某个用户 |



### 2.1.8 练习：完善你的 API 控制器逻辑

现在打开 `app/Http/Controllers/UserController.php`，我们可以快速填入一些模拟逻辑：

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    // 1. 列表
    public function index()
    {
        return ['data' => [['id' => 1, 'name' => 'Tom']]];
    }

    // 2. 新增
    public function store(Request $request)
    {
        return response()->json(['message' => '创建成功'], 201);
    }

    // 3. 详情
    public function show($id)
    {
        return ['id' => $id, 'name' => 'Tom'];
    }

    // 4. 更新
    public function update(Request $request, $id)
    {
        return ['message' => "用户 {$id} 已更新"];
    }

    // 5. 删除
    public function destroy($id)
    {
        return response()->json(null, 204); // 204 表示删除成功且无内容返回
    }
}
```

### 2.1.9 为什么这是“懒人包”？

1. **统一规范**：全公司的程序员看到 `index` 都知道是查列表，看到 `store` 都知道是保存。
2. **代码整洁**：你的路由文件 (`api.php`) 不再会有几十行 `Route::get`，而是一排整齐的 `apiResource`。
3. **自动化**：结合我们之前讲的 **2.2 数据库模型**，你甚至可以在这些方法里直接写 `User::all()` 或 `User::create()`，开发效率极高。

**总结口诀：**
* **生成**：`make:controller --api`
* **路由**：`Route::apiResource`
* **方法**：`index`/`store`/`show`/`update`/`destroy`


## 2.2 数据库与模型 (Eloquent ORM)

Laravel 最强大的地方之一：👉 **ORM（对象关系映射）**

### 2.2.0 数据库配置与连接（以MySQL为例）

在 Laravel 中，你不需要在代码里写 `mysqli_connect`。所有的环境配置都集中在项目根目录的 **`.env`** 文件中。

#### 2.2.0.1 修改 `.env` 文件
找到以下以 `DB_` 开头的配置项，根据你的 MySQL 实际情况进行修改：

```env
DB_CONNECTION=mysql          # 数据库类型
DB_HOST=127.0.0.1            # 数据库地址（本地通常是 127.0.0.1）
DB_PORT=3306                 # 端口
DB_DATABASE=my_laravel_db    # 你在 MySQL 中手动创建的数据库名
DB_USERNAME=root             # 数据库用户名
DB_PASSWORD=root             # 数据库密码
```

#### 2.2.0.2 在终端手动创建数据库
> **注意：** Laravel 不会自动帮你创建数据库（Database），你需要在执行迁移前手动创建。

打开终端，输入以下命令：
```bash
# 1. 登录 MySQL (会提示输入密码)
mysql -u root -p

# 2. 创建数据库 (注意要和 .env 里的 DB_DATABASE 一致)
CREATE DATABASE my_laravel_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

# 3. 退出
exit;
```
*(注：如果你使用的是 Laravel 11，执行 `php artisan migrate` 时如果数据库不存在，系统会询问是否自动创建，输入 yes 即可。)*


#### 2.2.0.3 确认 PHP 扩展
确保你的 PHP 环境已经开启了 `pdo_mysql` 扩展，否则连接会失败。

---


### 2.2.1 Migrations（数据库迁移）

👉 **用代码管理数据库结构**。你可以把它看作数据库的“版本控制系统”。

#### 2.2.1.1 创建迁移文件
```bash
# 命令格式：php artisan make:migration create_表名_table
# ------------------------------------------------------------
# 1. Laravel 看到 "create_"：
#    会自动在文件里写下 Schema::create('...', function...); —— 这是【建表】模板。
#
# 2. Laravel 看到中间的 "users"：
#    会自动把这个词填进代码，设为【数据库表名】。如果你写 orange，表名就叫 orange。
#
# 3. Laravel 看到 "_table"：
#    只是为了符合命名规范，让它看起来更像一个关于数据库表的动作。
# ------------------------------------------------------------

php artisan make:migration create_users_table
```

#### 2.2.1.2 迁移文件示例（含常用内置字段）
文件路径：`database/migrations/xxx_create_users_table.php` 
```php
public function up(): void
{
    Schema::create('users', function (Blueprint $table) {
        // --- 常用内置字段方法 ---
        $table->id();                       // 主键 ID (BigInt UNSIGNED)
        $table->string('name', 100);        // VARCHAR，长度 100
        $table->string('email')->unique();  // 唯一索引
        $table->string('password');         // 密码
        $table->text('bio')->nullable();    // 长文本，允许为空
        $table->integer('age')->default(18);// 整数，默认值 18
        $table->decimal('balance', 8, 2);   // 小数（金额），总8位，2位小数
        $table->boolean('is_active');       // 布尔值 (TinyInt)
        $table->json('settings');           // JSON 类型存储
        
        // --- 记录时间 ---
        $table->timestamps();               // 自动创建 created_at 和 updated_at
        $table->softDeletes();              // 软删除字段 deleted_at
    });
}
```

**迁移字段方法 vs 数据库类型对照表**

| Laravel 方法 | 对应 MySQL 类型 | 适用场景 | 小贴士 |
| :--- | :--- | :--- | :--- |
| **`$table->id()`** | `BIGINT UNSIGNED AUTO_INCREMENT` | **主键 ID** | 每张表必须有一个，默认叫 `id`。 |
| **`$table->string('字段名', 100)`** | `VARCHAR(100)` | 用户名、标题、邮箱 | **最常用**。不传第二个参数默认是 255 位。 |
| **`$table->text('字段名')`** | `TEXT` | 文章内容、个人简介 | 存放长文本，没有字符长度限制（或者说很大）。 |
| **`$table->integer('字段名')`** | `INT` | 年龄、排序、计数 | 纯整数。 |
| **`$table->boolean('字段名')`** | `TINYINT(1)` | 状态开关 | 只有 `0` (假) 和 `1` (真)。 |
| **`$table->decimal('字段名', 8, 2)`** | `DECIMAL(8,2)` | **价格、金额** | 精确小数。`8` 是总位数，`2` 是小数点后几位。 |
| **`$table->json('字段名')`** | `JSON` | 复杂配置、多选项 | 适合存放不固定的格式数据。 |
| **`$table->timestamps()`** | `created_at` & `updated_at` | 数据记录时间 | **必带**。Laravel 自动帮你维护这两个时间。 |


**字段修饰符（给字段加“额外规则”）**

在定义完字段类型后，常需要给字段加一些特殊限制，这时候就要用**链式调用**：

| 修饰符 | 作用 | 示例代码 |
| :--- | :--- | :--- |
| **`->nullable()`** | **允许为空** | `$table->string('avatar')->nullable();` |
| **`->default(默认值)`** | **设置默认值** | `$table->integer('score')->default(0);` |
| **`->unique()`** | **唯一约束**（不能重复） | `$table->string('phone')->unique();` |
| **`->comment('备注')`** | **添加注释** | `$table->string('status')->comment('0:禁用, 1:启用');` |



#### 2.2.1.3 执行与回滚命令

你可以把这看作是数据库的“后悔药”和“前进键”。

| 命令 | 它的作用（白话文） | 适用场景 |
| :--- | :--- | :--- |
| **`php artisan migrate`** | **执行**所有新写的迁移文件。 | 刚写好建表或加字段的代码，需要同步到数据库。 |
| **`php artisan migrate:rollback`** | **撤销**（回滚）最后一次迁移操作。 | 刚才运行的迁移写错了（比如字段名打错），撤回后改代码重来。 |
| **`php artisan migrate:refresh`** | **格式化**：先全部撤销，再全部重新执行。 | 开发初期，想清空所有测试数据并更新所有表结构。 |
| **`php artisan migrate:status`** | **查看**哪些文件运行了，哪些还没运行。 | 确认你的代码是否已经生效。 |


**💡 关于“回滚”的深度笔记（小白必记）：**

1. **原理：Ctrl + Z**
   回滚执行的是迁移文件里的 `down()` 方法。如果你在 `up()` 里建了表，`down()` 就会把这张表 **Drop（彻底删除）**。
   
2. **数据丢失警告 ⚠️**
   回滚不仅仅是撤回代码，它会**删除数据库里对应的真实数据**。
   * **例子**：你回滚了 `create_users_table`，那么 `users` 表里的 1000 个用户数据会瞬间消失，且无法找回。
   
3. **“最后一次”是指什么？**
   Laravel 会记录你每次运行 `php artisan migrate` 的批次。如果你刚才一次性运行了 3 个迁移文件，那么执行一次 `rollback` 会把这 3 个文件代表的操作**全部撤销**。

4. **进阶撤回**：
   如果你只想撤回最后 3 步，可以使用：
   `php artisan migrate:rollback --step=3`

---

**🔑 总结口诀：**
* 没表变有表 $\rightarrow$ **`migrate`**
* 有表变没表 $\rightarrow$ **`rollback`**
* 想推倒重来 $\rightarrow$ **`refresh`**


#### 2.2.1.4 中途添加/修改字段（标准做法）

##### 1️⃣ 第一步：生成专门的“修改”迁移文件
在终端输入以下命令：
```bash
# 语义化命名：add_字段名_to_表名_table
# --table 指定要给哪张表加字段
php artisan make:migration add_phone_to_users_table --table=users
```

##### 2️⃣ 第二步：编写逻辑
找到新生成的文件（在 `database/migrations/` 下最新的一条），你会发现方法从 `Schema::create` 变成了 `Schema::table`：

```php
public function up(): void
{
    Schema::table('users', function (Blueprint $table) {
        // 1. 添加字段（建议加上 nullable，防止旧数据因没有该字段而报错）
        // after('name') 表示把这个新字段放在 'name' 字段后面
        $table->string('phone')->nullable()->after('name'); 
    });
}

public function down(): void
{
    Schema::table('users', function (Blueprint $table) {
        // 回滚时，把这个字段删掉
        $table->dropColumn('phone');
    });
}
```

##### 3️⃣ 第三步：执行迁移
```bash
# 执行迁移
# 1. 【同步】将代码里的表结构真正创建到数据库中
php artisan migrate

# 2. 【撤销】如果发现刚才的迁移写错了（字段名打错等），执行回滚
# 注意：这会删除最后一次迁移所涉及的表和数据！
# php artisan migrate:rollback

# 3. 【重置】推倒重来（慎用！常用于开发初期，快速清空数据并更新所有表）
# php artisan migrate:refresh
```


##### 💡 核心知识点总结表

| 场景 | 命令/操作 | 风险 |
| :--- | :--- | :--- |
| **开发初期** (没啥重要数据) | 直接改原文件，然后跑 `php artisan migrate:refresh` | **高**：会清空表内所有数据。 |
| **项目中期** (已有正式数据) | **新建一个迁移文件**，用 `Schema::table` | **低**：只改变结构，保留原数据。 |
| **字段放哪？** | 使用 `->after('字段名')` | 无：纯粹为了你在数据库看表时更顺眼。 |
| **必填变选填** | 使用 `->nullable()->change()` | 中：需要安装 `doctrine/dbal` 扩展包才能修改已有字段。 |



##### ⚠️ 一个“小白”最容易掉的坑

当你添加了新字段（比如 `phone`）后，你通常会发现：**“为什么我用 `User::create(...)` 存不进手机号？”**

**原因**：你忘记更新 **Model（模型）** 了！
**解决办法**：
去 `app/Models/User.php` 找到 `$fillable` 数组，把新字段名加进去：
```php
protected $fillable = [
    'name', 
    'email', 
    'password', 
    'phone', // ← 别忘了加它！
];
```

### 2.2.2 ORM（模型）

👉 **一个 Model 类 = 数据库中的一张表**。

#### 2.2.2.1 创建模型
```bash
# 同时创建模型、迁移文件和控制器 (常用套路)
php artisan make:model Post -mc
```

#### 2.2.2.2 模型内置属性
文件路径：`app/Models/User.php` 

```php
// 在 Laravel 的标准结构中，所有的模型（Model）默认都存放在 app/Models/ 目录下
class User extends Model
{
    use SoftDeletes; // 开启软删除功能

    // 允许批量赋值的字段（白名单，不写这个 User::create() 会报错）
    protected $fillable = ['name', 'email', 'password'];

    // 隐藏敏感字段（转换为 JSON/数组时自动隐藏，如 API 返回）
    protected $hidden = ['password'];
}
```

#### 2.2.2.3 增删改查（常用内置“终结者”方法）
> **注意：** `get()`、`first()` 等方法被称为“执行器”，只有调用它们，Laravel 才会真正去数据库执行 SQL。

| 操作 | Eloquent 语法示例 |
| :--- | :--- |
| **新增数据** | `User::create(['name' => 'Tom', 'email' => 'tom@example.com']);` |
| **查询所有** | `$users = User::all();` (静态调用，获取表中全部记录) |
| **执行查询** | `$users = User::where('age', 18)->get();` (获取所有符合条件的结果集合) |
| **只取第一条** | `$user = User::where('name', 'Tom')->first();` (获取符合条件的第一条数据) |
| **根据主键查找** | `$user = User::find(1);` (查找 ID 为 1 的数据，找不到返回 `null`) |
| **严格查找** | `$user = User::findOrFail(1);` (**找不到直接抛 404 错误**，常用于控制器) |
| **分页查询** | `$users = User::paginate(10);` (自动分页，每页 10 条，自带分页逻辑) |
| **条件排序查询** | `$user = User::where('age', '>', 18)->orderBy('id', 'desc')->get();` |
| **更新数据** | `$user->update(['name' => 'New Name']);` (批量更新或单条更新) |
| **保存更改** | `$user->name = 'Jack'; $user->save();` (手动赋值后保存) |
| **删除数据** | `$user->delete();` (根据实例删除，若开启软删除则为逻辑删除) |
| **批量删除** | `User::destroy([1, 2, 3]);` (根据主键 ID 直接删除多条) |


### 2.2.3 模型关联（Relationship）

#### 2.2.3.1 定义关联：代码写在哪里？
关联关系必须定义在 `app/Models/` 目录下的模型类中。

* **场景一：一对多（一个用户拥有多篇文章）**
    在 **`app/Models/User.php`** 中：
    ```php
    namespace App\Models;

    use Illuminate\Database\Eloquent\Model;

    class User extends Model {
        // 定义关联：用户可以有多个文章
        public function posts() {
            return $this->hasMany(Post::class); 
        }
    }
    ```

* **场景二：多对一/反向关联（这篇文章属于哪个用户）**
    在 **`app/Models/Post.php`** 中：
    ```php
    namespace App\Models;

    use Illuminate\Database\Eloquent\Model;

    class Post extends Model {
        // 定义关联：文章属于一个用户
        public function user() {
            return $this->belongsTo(User::class); 
        }
    }
    ```


#### 2.2.3.2 关联进阶：如何正确地“拿”数据？
定义好上面的方法后，你就可以在 **控制器（Controller）** 或者 **路由（Route）** 中使用了。

> **💡 核心痛点：N+1 查询问题**
> 如果直接循环读取关联数据，会导致数据库查询次数爆炸。

* **❌ 错误写法（在 Controller 中）**：
    ```php
    // 假设有 100 个用户
    $users = User::all(); // 第 1 次查询：查出所有用户

    foreach ($users as $user) {
        // 坑：每次循环都会产生 1 次 SQL 去查当前用户的文章
        // 总共会执行 1 + 100 = 101 次 SQL 查询！
        $userPosts = $user->posts; 
    }
    ```

* **✅ 正确写法（使用 `with` 预加载）**：
    ```php
    // 使用 with('方法名')
    // 只有 2 条 SQL：一条查用户，一条用 IN 语句查出所有相关的文章
    $users = User::with('posts')->get(); 

    foreach ($users as $user) {
        // 此时数据已经在内存里了，不再产生额外的 SQL 查询
        $userPosts = $user->posts; 
    }
    ```


#### 2.2.3.3 多对多关联 (Many To Many)
常用于“用户与角色”、“文章与标签”。

* **在 `app/Models/User.php` 中定义：**
    ```php
    public function roles() {
        // 一个用户可以拥有多个角色
        return $this->belongsToMany(Role::class);
    }
    ```

* **使用方式：**
    ```php
    $user = User::find(1);
    // 获取该用户的所有角色
    $roles = $user->roles; 
    ```


#### 总结：新手记账秘籍
1.  **定义逻辑**：写在 `app/Models/` 下的对应的 `.php` 文件里。
2.  **方法命名**：一对多通常用复数（如 `posts()`），属于关系通常用单数（如 `user()`）。
3.  **取数据**：在获取主模型时，记得带上 `with('关联方法名')` 来保住数据库的命。


## 2.3 Blade 模板引擎

👉 Blade = Laravel 的前端模板引擎



### 2.3.1 基础语法

#### 2.3.1.1 输出变量

```blade
{{ $name }}
```

👉 自动防 XSS（安全）



#### 2.3.1.2 条件判断

```blade
@if($age > 18)
    成年人
@else
    未成年
@endif
```



#### 2.3.1.3 循环

```blade
@foreach($users as $user)
    <p>{{ $user->name }}</p>
@endforeach
```



### 2.3.2 模板继承



#### 2.3.2.1 父模板

```html
<!-- resources/views/layouts/app.blade.php -->

<html>
<head>
    <title>@yield('title')</title>
</head>
<body>

    <h1>网站头部</h1>

    @yield('content')

    <footer>网站底部</footer>

</body>
</html>
```



#### 2.3.2.2 子模板

```html
@extends('layouts.app')

@section('title', '首页')

@section('content')
    <h2>欢迎来到首页</h2>
@endsection
```



### 2.3.3 组件（Component）

👉 更现代的写法（推荐）



#### 2.3.3.1 创建组件

```bash
php artisan make:component Alert
```



#### 2.3.3.2 使用组件

```html
<x-alert type="success" message="操作成功！" />
```



#### 2.3.3.3 组件模板

```html
<!-- resources/views/components/alert.blade.php -->

<div class="alert alert-{{ $type }}">
    {{ $message }}
</div>
```



## 2.4 总结

你可以这样理解 Laravel：

```
用户请求 → 路由 → 控制器 → 模型 → 数据库
                         ↓
                      返回数据
                         ↓
                     Blade 渲染页面
```



# 3. 请求处理与安全性

## 3.1 表单验证（Validation）

在 Laravel 中，**永远不要相信用户输入**。验证不仅是为了确保数据格式正确，更是为了防止 SQL 注入等安全问题。


### 3.1.1 验证的生命周期

当用户点击“提交”按钮后，Laravel 会按以下顺序处理：
1. **接收数据**：获取 `$request` 中的所有字段。
2. **匹配规则**：对比你定义的 `rules`。
3. **判定结果**：
    * **失败**：如果是普通网页，自动跳回表单页，并将错误信息存入 `Session`；如果是 API 请求，直接返回 `425 Unprocessable Entity` 状态码和 JSON 错误。
    * **成功**：继续执行控制器剩下的代码。



### 3.1.2 三种验证方式对比

| 方式 | 编写位置 | 适用场景 | 优点 |
| :--- | :--- | :--- | :--- |
| **控制器验证** | Controller 方法内 | 临时、简单的逻辑 | 快速、代码紧凑 |
| **Validator 类** | 手动实例化 | 需要手动控制跳转或 Ajax | 极其灵活，可随时中断 |
| **Form Request** | 独立的 Request 类 | **企业级/正式项目** | **推荐！** 控制器只负责业务，代码最整洁 |



### 3.1.3 方式 3：Form Request 实战（最推荐 ⭐）

#### 3.1.3.1 第一步：生成请求类
```bash
php artisan make:request StoreUserRequest
```

#### 3.1.3.2 第二步：配置验证规则
文件位置：`app/Http/Requests/StoreUserRequest.php`

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreUserRequest extends FormRequest
{
    // 1. 权限控制：谁能提交这个表单？
    public function authorize(): bool
    {
        // 示例：只有登录用户才能提交。小白练手时先改写为 true。
        return true; 
    }

    // 2. 定义验证规则
    public function rules(): array
    {
        return [
            // 规则之间用 | 隔开，或使用数组 ['required', 'min:3']
            'name'     => 'required|min:3|max:50',
            'email'    => 'required|email|unique:users,email',
            
            // 重要：confirmed 规则要求前端必须有一个字段叫 password_confirmation
            'password' => 'required|min:6|confirmed', 
        ];
    }

    // 3. 自定义属性名称（让错误提示更好看）
    public function attributes(): array
    {
        return [
            'email' => '电子邮箱地址',
        ];
    }

    // 4. 自定义具体错误消息
    public function messages(): array
    {
        return [
            'name.required' => '亲，起个名字吧！',
            'password.min'  => '密码太短了，至少要 6 位数哦。',
        ];
    }
}
```


### 3.1.4 常用验证规则分类表

#### 3.1.4.1 一览图

| 规则 | 写法示例 | 详细说明 |
| :--- | :--- | :--- |
| **必填** | `required` | 字段必须存在且不能为空 |
| **可选** | `nullable` | 如果没填就不验证，填了才按后续规则验证 |
| **邮箱** | `email` | 必须符合邮箱格式 |
| **唯一** | `unique:表名,字段名` | 数据库中不能重复（常用于注册） |
| **长度** | `min:8` / `max:20` | 限制字符串长度或数字大小 |
| **确认** | `confirmed` | **自动比对** `字段名` 与 `字段名_confirmation` |
| **相同/不同** | `same:field` / `different:field` | 两个字段值必须一致/不一致 |
| **正则表达式**| `regex:/^[a-z]+$/i` | 自定义高级匹配逻辑 |

#### 3.1.4.2 API 请求时怎么用 `字段名_confirmation` ？

**API 请求体 (JSON):**
```json
{
    "name": "Tom",
    "email": "tom@example.com",
    "password": "secret_password",
    "password_confirmation": "secret_password" 
}
```
*注意：这里的 `password_confirmation` 必须和 `password` 的值完全一模一样。*

**返回的错误:**

如果前端传的两次密码不一致，前端会收到一个标准的 **422 状态码**：

```json
{
    "message": "The password confirmation does not match.",
    "errors": {
        "password": [
            "两次输入的密码不匹配。"
        ]
    }
}
```


### 3.1.5 在 Blade 模板中显示错误

当验证失败回跳时，Laravel 会自动把变量 `$errors` 注入到所有视图中。

#### 3.1.5.1 顶部集中显示所有错误
```blade
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
```

#### 3.1.5.2 字段下方精准提示（更美观）
```blade
<input type="text" name="email" value="{{ old('email') }}">
@error('email')
    <div class="text-red-500">{{ $message }}</div>
@enderror
```
> **💡 小技巧：** 使用 `old('字段名')` 函数可以回显用户刚才输入的内容，避免用户因为一个错就得重填整个表单。


### 3.1.6 进阶：自定义验证规则（Rule 类）

如果内置规则不够用，比如你要验证“手机号是否符合特定运营商格式”：

1. **执行命令**：`php artisan make:rule IsMobile`
2. **编写逻辑** (`app/Rules/IsMobile.php`)：
```php
public function validate(string $attribute, mixed $value, Closure $fail): void
{
    // 正则验证中国大陆手机号
    if (!preg_match('/^1[3-9]\d{9}$/', $value)) {
        $fail('手机号格式不正确。');
    }
}
```
3. **在 Request 中使用**：
```php
'phone' => ['required', new IsMobile()],
```


### 3.1.7 三大高频“翻车”现场

1. **`confirmed` 规则失效**：
   * 原因：前端 HTML 里的确认框 `name` 没写对。
   * 解决：如果密码框是 `password`，确认框必须叫 `password_confirmation`。

2. **`unique` 规则报错**：
   * 原因：在“修改资料”时提示 Email 已被占用（其实是被自己占用了）。
   * 解决：需要在规则后排除当前 ID，例如 `'email' => 'unique:users,email,'.$this->user()->id`。

3. **authorize() 返回 false**：
   * 原因：忘记把默认生成的 `return false;` 改成 `true`。
   * 现象：提交表单直接报 `403 Forbidden`。


## 3.2 中间件（Middleware）



### 3.2.1 基本概念

👉 类似“守门员”

请求流程：

```
请求 → 中间件 → 控制器 → 响应 → 中间件
```



### 3.2.2 中间件类型

| 类型 | 说明       |
| -- | -- |
| 全局 | 所有请求都会经过 |
| 路由 | 指定路由使用   |
| 组  | 一组中间件    |



### 3.2.3 常见内置中间件

```php
'auth'       // 登录验证
'guest'      // 未登录用户
'throttle'   // 限流
'verified'   // 邮箱验证
'csrf'       // CSRF 防护
```



### 3.2.4 自定义中间件

```bash
php artisan make:middleware CheckAdmin
```

```php
public function handle($request, Closure $next)
{
    if (auth()->user()?->role !== 'admin') {
        abort(403, '无权限');
    }

    return $next($request);
}
```



注册（Kernel.php）：

```php
protected $routeMiddleware = [
    'admin' => \App\Http\Middleware\CheckAdmin::class,
];
```



### 3.2.5 使用方式

#### 路由中使用

```php
Route::get('/admin', function () {
    return '后台';
})->middleware('admin');
```



#### 控制器中使用

```php
public function __construct()
{
    $this->middleware('auth');
}
```



#### 分组

```php
Route::middleware(['auth', 'admin'])->group(function () {
    Route::get('/dashboard', fn() => '后台');
});
```



### 3.2.6 应用场景

* 登录校验
* 权限控制
* API 限流
* 日志记录



## 3.3 安全机制（Security）



### 3.3.1 CSRF 防护

#### 表单必须写：

```blade
<form method="POST">
    @csrf
</form>
```

👉 Laravel 会自动验证 Token



### 3.3.2 XSS 防护

```blade
{{ $name }}   // 自动转义（安全）
{!! $name !!} // 原始输出（危险）
```



### 3.3.3 SQL 注入防护

❌ 错误写法：

```php
DB::select("SELECT * FROM users WHERE name = '$name'");
```



✅ 正确写法：

```php
User::where('name', $name)->get();
```



### 3.3.4 文件上传安全

```php
$request->validate([
    'file' => 'required|file|mimes:jpg,png|max:2048'
]);
```



### 3.3.5 密码安全

```php
use Illuminate\Support\Facades\Hash;

$password = Hash::make('123456');
```

验证：

```php
Hash::check('123456', $user->password);
```



### 3.3.6 输入数据安全

```php
$name = strip_tags($request->input('name'));
```



## 3.4 用户认证（详见第五章）



### 3.4.1 认证方案

#### 快速安装：

```bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
```



### 3.4.2 认证流程

```
注册 → 登录 → Session → 访问 → 登出
```



### 3.4.3 核心功能

#### 登录

```php
if (Auth::attempt($request->only('email', 'password'))) {
    return redirect()->intended('dashboard');
}
```



#### 当前用户

```php
$user = auth()->user();
```



#### 登出

```php
Auth::logout();
```



### 3.4.4 扩展认证

#### API（Sanctum）

```bash
composer require laravel/sanctum
```



## 3.5 权限控制（详见第五章）



### 3.5.1 权限方式
| 方式     | 使用场景           |
|----------|------------------|
| Gate     | 简单判断         |
| Policy   | 复杂权限（推荐） |



### 3.5.2 Gate

```php
Gate::define('edit-post', function ($user, $post) {
    return $user->id === $post->user_id;
});
```

使用：

```php
if (Gate::allows('edit-post', $post)) {
    // 可以编辑
}
```



### 3.5.3 Policy（推荐）

```bash
php artisan make:policy PostPolicy --model=Post
```

```php
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
```



控制器：

```php
$this->authorize('update', $post);
```



## 3.6 综合最佳实践（重点🔥）

### 3.6.1 技术选型
| 功能 | 推荐          |
|------|-------------|
| 验证 | Form Request |
| 权限 | Policy       |
| 认证 | Breeze       |
| API  | Sanctum      |



### 3.6.2 安全组合

👉 必须同时做：

* CSRF 防护
* 后端验证（最重要）
* 密码 Hash
* ORM 防注入



### 3.6.3 常见错误（新手必看❗）

#### ❌ 错误1：只做前端验证

👉 后端必须再验证一遍



#### ❌ 错误2：不做权限控制

👉 用户能随便改别人数据



#### ❌ 错误3：明文密码

👉 必须 Hash



#### ❌ 错误4：接口没加中间件

```php
Route::get('/user', fn() => auth()->user());
```

👉 应该：

```php
Route::middleware('auth')->get('/user', fn() => auth()->user());
```



## 3.7 总结一句话（帮你记住核心）

👉 Laravel 安全三件套：

```
验证（Validation） + 中间件（Middleware） + 权限（Authorization）
```


# 4. FilamentPHP [选学]

👉 一句话理解：

```text
Filament = Laravel 后台管理系统生成器（不用写前端）
```

你只需要：

* 写模型（Model）
* 写一点点配置

👉 就能得到：

* 后台管理页面（增删改查）
* 表单
* 表格
* 权限控制
* 仪表盘



## 4.1 安装与初始化



### 4.1.1 安装 Filament

```bash
composer require filament/filament
```



### 4.1.2 初始化后台

```bash
php artisan filament:install
```

👉 会自动做这些事情：

* 创建后台路由 `/admin`
* 安装依赖（Livewire、Tailwind）
* 配置后台面板



### 4.1.3 创建管理员用户

```bash
php artisan make:filament-user
```

输入：

```text
Name: admin
Email: admin@test.com
Password: 123456
```



### 4.1.4 启动项目

```bash
php artisan serve
# 或自定义端口 
# php artisan serve --port=7890
```

访问默认端口：

```text
http://127.0.0.1:8000/admin
```

或访问自定义端口：

```text
http://127.0.0.1:7890/admin
```

👉 登录成功 = 后台就搭好了 🎉



## 4.2 Resources（资源管理核心🔥）



### 4.2.1 一键生成 CRUD

假设你有一个模型：

```bash
php artisan make:model Product -m
```

迁移文件：

```php
public function up()
{
    Schema::create('products', function (Blueprint $table) {
        $table->id();
        $table->string('name');       // 商品名
        $table->decimal('price', 8, 2); // 价格
        $table->text('description')->nullable();
        $table->timestamps();
    });
}
```

执行：

```bash
php artisan migrate
```



👉 生成 Filament Resource：

```bash
php artisan make:filament-resource Product
```



生成结构：

```text
app/Filament/Resources/ProductResource.php
app/Filament/Resources/ProductResource/Pages/
```



### 4.2.2 Form 表单构建（重点🔥）

打开：

```php
ProductResource.php
```



#### 4.2.2.1 表单示例（带详细注释）

```php
use Filament\Forms;
use Filament\Forms\Form;

public static function form(Form $form): Form
{
    return $form
        ->schema([
            // 输入框
            Forms\Components\TextInput::make('name')
                ->label('商品名称') // 显示名称
                ->required() // 必填
                ->maxLength(100),

            // 数字输入
            Forms\Components\TextInput::make('price')
                ->label('价格')
                ->numeric() // 只能输入数字
                ->required(),

            // 多行文本
            Forms\Components\Textarea::make('description')
                ->label('描述')
                ->rows(4),

            // 日期选择器
            Forms\Components\DatePicker::make('created_at')
                ->label('创建时间'),

        ]);
}
```



#### 4.2.2.2 常用组件大全（新手必会）

```php
// 输入框
TextInput::make('title')

// 下拉框
Select::make('status')
    ->options([
        'draft' => '草稿',
        'published' => '已发布',
    ])

// 开关
Toggle::make('is_active')

// 文件上传
FileUpload::make('image')

// 富文本编辑器
RichEditor::make('content')
```



### 4.2.3 Table 表格构建



```php
use Filament\Tables;
use Filament\Tables\Table;

public static function table(Table $table): Table
{
    return $table
        ->columns([
            // 文本列
            Tables\Columns\TextColumn::make('name')
                ->label('商品名称')
                ->searchable() // 可搜索
                ->sortable(), // 可排序

            Tables\Columns\TextColumn::make('price')
                ->label('价格'),

            Tables\Columns\TextColumn::make('created_at')
                ->dateTime()
                ->label('创建时间'),
        ])

        ->filters([
            // 筛选器（比如价格筛选）
        ])

        ->actions([
            Tables\Actions\EditAction::make(), // 编辑按钮
            Tables\Actions\DeleteAction::make(), // 删除按钮
        ])

        ->bulkActions([
            Tables\Actions\DeleteBulkAction::make(), // 批量删除
        ]);
}
```



### 4.2.4 搜索 & 筛选示例

```php
Tables\Filters\Filter::make('expensive')
    ->query(fn ($query) => $query->where('price', '>', 100))
```



## 4.3 Widgets 与自定义



### 4.3.1 仪表盘统计（Dashboard）



生成 Widget：

```bash
php artisan make:filament-widget StatsOverview
```



```php
use Filament\Widgets\StatsOverviewWidget as BaseWidget;
use Filament\Widgets\StatsOverviewWidget\Stat;
use App\Models\Product;

class StatsOverview extends BaseWidget
{
    protected function getStats(): array
    {
        return [
            // 商品总数
            Stat::make('商品数量', Product::count()),

            // 总价格
            Stat::make('总库存价值', Product::sum('price')),
        ];
    }
}
```



👉 页面效果：

```text
[ 商品数量: 100 ]
[ 总库存价值: 9999 ]
```



### 4.3.2 图表 Widget

```bash
php artisan make:filament-widget ProductChart --chart
```



```php
protected function getData(): array
{
    return [
        'datasets' => [
            [
                'label' => '商品增长',
                'data' => [10, 20, 30, 50],
            ],
        ],
        'labels' => ['1月', '2月', '3月', '4月'],
    ];
}
```



### 4.3.3 Action（自定义操作🔥）

👉 比如：导出 PDF、发送通知



#### 4.3.3.1 添加按钮

```php
use Filament\Tables\Actions\Action;

Action::make('download')
    ->label('下载')
    ->action(function ($record) {
        // $record = 当前行数据
        dd($record);
    })
```



#### 4.3.3.2 带确认弹窗

```php
Action::make('delete')
    ->requiresConfirmation()
    ->action(fn ($record) => $record->delete())
```



#### 4.3.3.3 通知提示

```php
use Filament\Notifications\Notification;

Notification::make()
    ->title('操作成功')
    ->success()
    ->send();
```



## 4.4 实战：完整 CRUD 流程



### 4.4.1 一步到位流程：

```bash
php artisan make:model Post -m
php artisan migrate
php artisan make:filament-resource Post
```

👉 完成后你已经拥有：

* 列表页
* 创建页
* 编辑页
* 删除功能
* 搜索
* 分页

👉 全部自动生成 😱



## 4.5 小白避坑指南（非常重要❗）



### ❌ 错误1：没迁移数据库

```bash
php artisan migrate
```



### ❌ 错误2：字段名不一致

```php
// 表字段
title

// 表单字段
TextInput::make('title') ✅
TextInput::make('name') ❌
```



### ❌ 错误3：权限问题（403）

👉 检查：

```php
public static function canViewAny(): bool
{
    return true;
}
```



### ❌ 错误4：文件上传失败

👉 必须配置：

```bash
php artisan storage:link
```



## 4.6 最终总结（帮你建立认知）



### 4.6.1 Filament 做了什么？

👉 把这些复杂的东西：

```text
前端页面 + 表单 + 表格 + JS + 样式
```

👉 变成：

```text
PHP 配置
```



### 4.6.2 你只需要会：

* Laravel Model
* 数据库
* 一点点配置



### 4.6.3 一句话理解

```text
Filament = Laravel 后台作弊器
```

# 5. Laravel + Flutter 认证与权限

为了让你更系统地掌握 **Laravel + Flutter** 的认证与权限体系，我为你梳理了一份核心知识点大纲。这套架构不仅适用于登录，也是现代移动端前后端分离开发的通用标准。

## 5.0 Laravel 自带的用户系统

### 5.0.1 默认的用户模型

Laravel 自带的用户模型是：`App\Models\User`
```php
// 在 app/Models/User.php
namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    // 默认已经有这些字段：
    // id, name, email, email_verified_at, password, remember_token, created_at, updated_at
}
```

### 5.0.2 默认的数据库迁移

```php
// 在database/migrations/xxxx_create_users_table.php

Schema::create('users', function (Blueprint $table) {
    $table->id();                       // 自增ID
    $table->string('name');             // 用户名
    $table->string('email')->unique();  // 邮箱（唯一）
    $table->timestamp('email_verified_at')->nullable();  // 邮箱验证时间
    $table->string('password');         // 加密后的密码
    $table->rememberToken();            // 记住我功能
    $table->timestamps();               // 创建时间和更新时间
});
```

### 5.0.3 开箱即用的功能

安装完 Laravel 就有了：
1. 用户注册/登录/邮箱验证
2. 密码重置
3. 记住我功能
4. CSRF 保护
5. 加密存储密码

### 5.0.4 如何添加自定义字段（比如 'role'）

#### 5.0.4.1 创建新的迁移文件

```bash
php artisan make:migration add_role_to_users_table
```

```php
// 在迁移文件中
public function up()
{
    Schema::table('users', function (Blueprint $table) {
        $table->string('role')->default('user');  // 添加角色字段
        // 默认值是 'user'，可以是 'superadmin', 'staff', 'user' 等
    });
}
```

#### 5.0.4.2 运行迁移
```bash
php artisan migrate
```

```php
// 然后在 User 模型中添加
class User extends Authenticatable
{
    protected $fillable = [
        'name', 'email', 'password', 'role'  // 允许批量赋值的字段
    ];
    
    // 也可以定义角色常量
    const ROLE_SUPERADMIN = 'superadmin'; // 超级管理员
    const ROLE_STAFF = 'staff'; // 管理员
    const ROLE_USER = 'user'; // 普通用户
}
```
#### 5.0.4.3 定义角色常量

🎯 基本用法对比

❌ 不用常量（硬编码，容易出错）
```php
if ($user->role === 'superuser') {
    // 问题：字符串容易拼错
}

$user->update(['role' => 'supradmin']);  // 拼错了！应该是 'superadmin'
```

✅ 使用常量（安全可靠）
```php
// 定义在 User 模型中
class User extends Authenticatable
{
    const ROLE_SUPERADMIN = 'superadmin';
    const ROLE_STAFF = 'staff';
    const ROLE_USER = 'user';

    // 判断方法
    public function isSuperAdmin(): bool
    {
        return $this->role === self::ROLE_SUPERADMIN;
    }
}
```

### 5.0.5 重要说明

1. 身份验证（Auth） 和 授权（Gate/Policies） 是分开的：
   • Auth：你是谁？（登录验证）

   • Gate/Policies：你能做什么？（权限检查）

2. 你之前问的 Gate::define 就是建立在 User 模型基础上的授权系统

3. Laravel 的认证系统很灵活：
   • 可以用默认的 users 表

   • 也可以自定义表名、字段

   • 还支持 API Token、Socialite 社交登录等

一句话总结：是的，Laravel 自带了 User 模型和全套认证系统，你只需要添加自己的业务字段（如 role）就可以直接用！

## 5.1 认证机制选择 (Authentication Strategies)

### 5.1.1 认证机制

#### 5.1.1.1 JWT 是什么（先搞懂这个）

👉 JWT = 登录凭证（token）

用户登录后：

```text
服务器 → 生成 token → 返回给 Flutter
Flutter → 每次请求带 token
```

📌 服务器不存 session
📌 token 自己携带身份信息


### 5.1.2 安装 JWT（Laravel）

```bash
composer require tymon/jwt-auth
```


#### 5.1.2.1 发布配置文件（必须做）

```bash
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

👉 作用：生成 JWT 配置文件


#### 5.1.2.2 生成密钥（非常重要）

```bash
php artisan jwt:secret
```

👉 作用：

* 生成加密 token 的 secret
* 不然 token 会报错


### 5.1.3 修改 User 模型（核心）

打开：

```
app/Models/User.php
```


#### ✨ 改成这样（带超详细注释）

```php
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use Tymon\JWTAuth\Contracts\JWTSubject;

/* 继承链条：

User 类继承了 Authenticatable
Authenticatable 是 Model 的子类
这样 User 既是一个“数据库模型”，也是一个“可登录的对象”

*/

class User extends Authenticatable implements JWTSubject
{
    /**
     * 允许批量写入的字段
     * 👉 比如 User::create([...]) 能用这些字段
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * 隐藏字段（不会返回给前端）
     * 👉 防止密码泄露
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * 🧠 JWT 必须方法 1
     * 👉 返回这个用户的唯一 ID（一般就是主键 id）
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * 🧠 JWT 必须方法 2
     * 👉 额外放进 token 的数据（一般不用）
     */
    public function getJWTCustomClaims()
    {
        return [];
    }
}
```


## 5.2 AuthController（登录核心）

创建：

```bash
php artisan make:controller Api/AuthController
```
打开文件：
```
app/Http/Controllers/Api/AuthController.php
```

### 5.2.1 登录接口（重点🔥）

```php
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use App\Models\User;
use Illuminate\Support\Facades\Hash;
use Tymon\JWTAuth\Facades\JWTAuth;

class AuthController extends Controller
{
    /**
     * 🔐 用户登录
     * 👉 email + password → 返回 token
     */
    public function login(Request $request)
    {
        // 1️⃣ 验证前端传来的数据
        $request->validate([
            'email' => 'required|email',   // 必须是邮箱格式
            'password' => 'required'       // 必须有密码
        ]);

        // 2️⃣ 去数据库找用户
        $user = User::where('email', $request->email)->first();

        // 3️⃣ 判断用户是否存在 + 密码是否正确
        if (!$user || !Hash::check($request->password, $user->password)) {

            // ❌ 登录失败
            return response()->json([
                'message' => '账号或密码错误'
            ], 401); // 401 = 没权限
        }

        // 4️⃣ ⭐ 生成 JWT token（核心）
        // 👉 这个 token 就是“登录凭证”
        $token = JWTAuth::fromUser($user);

        // 5️⃣ 返回给 Flutter
        return response()->json([
            'user' => $user,   // 用户信息
            'token' => $token   // 登录凭证
        ]);
    }
}
```


### 5.2.2 注册接口

```php
public function register(Request $request)
{
    // 1️⃣ 验证输入
    $request->validate([
        'name' => 'required|string|max:255',
        'email' => 'required|email|unique:users', // 邮箱不能重复
        'password' => 'required|min:6'
    ]);

    // 2️⃣ 创建用户
    $user = User::create([
        'name' => $request->name,
        'email' => $request->email,

        // 🔐 密码必须加密存储（不能明文！）
        'password' => bcrypt($request->password),
    ]);

    // 3️⃣ 生成 token
    $token = JWTAuth::fromUser($user);

    // 4️⃣ 返回数据
    return response()->json([
        'user' => $user,
        'token' => $token
    ]);
}
```

### 5.2.3 获取当前用户（必须登录）

```php
/**
 * 👤 获取当前登录用户
 * 👉 需要 token 才能访问
 */
public function me()
{
    return response()->json(auth()->user());
}
```

### 5.2.4 登出

```php
use Tymon\JWTAuth\Facades\JWTAuth;

public function logout()
{
    /**
     * ❌ 让当前 token 失效
     * 👉 等于“退出登录”
     */
    JWTAuth::invalidate(JWTAuth::getToken());

    return response()->json([
        'message' => '退出成功'
    ]);
}
```


### 5.2.5 路由保护（重点🔥）

打开 `routes/api.php`

```php
use App\Http\Controllers\Api\AuthController;
use Illuminate\Http\Request;

/**
 * 🛡 需要登录才能访问的接口
 */
Route::middleware('auth:api')->group(function () {

    // 👤 获取当前用户
    Route::get('/user', function () {
        return auth()->user();
    });

    // 🚪 退出登录
    Route::post('/logout', [AuthController::class, 'logout']);
});
```

### 5.2.6 配置 auth

打开：

```
config/auth.php
```

找到：

```php
'guards' => [
    'api' => [
        'driver' => 'jwt', 
        'provider' => 'users',
    ],
],
```


## 5.3 Flutter 前端

因为 JWT 和 Sanctum 对 Flutter 来说一样：

👉 都是：

```text
Authorization: Bearer token
```
👉 每次请求自动带 token：

```dart
options.headers['Authorization'] = 'Bearer $token';
```

📌 作用：

* 自动登录
* 不用每次手动传 token


### 5.3.1 安装依赖

```yaml
dio: ^5.0.0
flutter_secure_storage: ^9.0.0
```


### 5.3.2 Token 安全存储

```dart
import 'package:flutter_secure_storage/flutter_secure_storage.dart';

class TokenStorage {
  final _storage = FlutterSecureStorage();

  // 保存 Token
  Future<void> saveToken(String token) async {
    await _storage.write(key: 'token', value: token);
  }

  // 获取 Token
  Future<String?> getToken() async {
    return await _storage.read(key: 'token');
  }

  // 删除 Token
  Future<void> deleteToken() async {
    await _storage.delete(key: 'token');
  }
}
```


### 5.3.3 Dio 封装（非常重要🔥）

```dart
import 'package:dio/dio.dart';

class ApiClient {
  late Dio dio;
  final storage = TokenStorage();

  ApiClient() {
    dio = Dio(
      BaseOptions(
        baseUrl: "http://your-api.com/api",
        connectTimeout: Duration(seconds: 5),
      ),
    );

    // 🔥 请求拦截器
  dio.interceptors.add(
    InterceptorsWrapper(
      onRequest: (options, handler) async {

        // 🔐 从本地拿 token
        final token = await storage.getToken();

        // 👉 如果有 token，就加到请求头
        if (token != null) {
          options.headers['Authorization'] = 'Bearer $token';
        }

       // 🚀 继续发送请求
        return handler.next(options);

        },
      ),
    );
  }
}
```


### 5.3.4 登录请求

```dart
Future<void> login(String email, String password) async {
  final response = await dio.post('/login', data: {
    'email': email,
    'password': password,
  });

  String token = response.data['token'];

  await storage.saveToken(token);
}
```


### 5.3.5 获取用户信息

```dart
Future<void> getUser() async {
  final response = await dio.get('/user');

  print(response.data);
}
```



## 5.4 权限系统（Authorization / Permission）

在 Laravel 中，权限系统用于控制用户是否可以执行某个操作，例如：

* 是否允许删除文章
* 是否允许更新数据
* 是否允许访问后台

Laravel 提供两种核心方式：

* Gate（简单权限判断）
* Policy（基于模型的权限控制，推荐）


### 5.4.1 Gate 示例

Gate 是 Laravel 提供的一种**轻量级权限判断方式**，适合简单逻辑，例如“是否管理员”。


#### 5.4.1.1 定义 Gate

```php
use Illuminate\Support\Facades\Gate;

Gate::define('is-admin', function ($user) {
    // 判断用户是否是管理员
    return $user->role === 'admin';
});
```


#### 5.4.1.2 代码说明

```php
$user->role === 'admin'
```

含义：

* 从数据库读取用户角色字段
* 如果等于 admin → 返回 true
* 否则返回 false


#### 5.4.1.3 使用 Gate（基础判断）

```php
use Illuminate\Support\Facades\Gate;

if (Gate::allows('is-admin')) {
    // 当前用户是管理员
    echo "欢迎管理员";
}
```


##### ❌ 反向判断

```php
if (Gate::denies('is-admin')) {
    abort(403, '无权限访问');
}
```


##### 🚨 推荐写法（自动抛异常）

```php
Gate::authorize('is-admin');

// 如果没有权限，会自动返回 403
echo "通过权限验证";
```


##### 🎯 Blade 中使用 Gate

```php
@can('is-admin')
    <button>删除用户</button>
@endcan
```

👉 如果不是管理员，这个按钮不会渲染


##### 🧪 扩展示例：VIP 权限

```php
Gate::define('is-vip', function ($user) {
    // 判断 VIP 是否过期
    return $user->vip_expired_at > now();
});
```


##### 🧪 扩展示例：多角色权限

```php
// 定义一个名为 "access-dashboard" 的权限规则
// 这个规则用来判断：当前用户能不能访问管理后台
Gate::define('access-dashboard', function ($user) {
    // 检查用户的角色（role）
    // 只有 role 是 "admin" 或 "editor" 的用户才允许访问
    return in_array($user->role, ['admin', 'editor']);
    // 如果是 admin 或 editor → 返回 true → 允许访问
    // 其他角色 → 返回 false → 禁止访问
});
```


#### 5.4.1.4 在 Controller 中使用

```php
namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Gate;
use App\Models\User;

class UserController extends Controller
{
    /**
     * 示例：删除用户（仅限管理员）
     */
    public function destroy($id)
    {
        // 🚨 【最常用】使用 authorize
        // 如果权限验证失败：直接抛出异常，给前端返回 403，且下方的代码永远不会执行。
        // 如果权限验证通过：继续往下运行。
        Gate::authorize('is-admin');

        User::destroy($id);
        
        return response()->json(['message' => '用户已成功删除']);
    }

    /**
     * 示例：获取内部敏感数据
     */
    public function getSensitiveData()
    {
        // ❌ 【反向判断】使用 denies
        if (Gate::denies('is-admin')) {
            // 手动拦截并返回自定义信息
            return response()->json(['message' => '只有管理员能看这个！'], 403);
        }

        return response()->json(['data' => '这是高度机密数据']);
    }

    /**
     * 示例：根据权限返回不同的内容
     */
    public function showProfile()
    {
        // ✅ 【正向判断】使用 allows
        if (Gate::allows('is-admin')) {
            // 管理员看到完整版
            return ["status" => "Admin Full Access", "info" => "所有系统日志..."];
        }

        // 普通用户看到简洁版
        return ["status" => "Regular User", "info" => "基本资料..."];
    }
}
```

#### 5.4.1.5 Gate 常用方法总结

| 方法 | 逻辑含义 | 失败后果 | 适用场景 |
| :--- | :--- | :--- | :--- |
| **`Gate::authorize()`** | **必须**具备该权限 | **立即中断**程序，抛出 403 异常，不执行后续代码。 | **最推荐**。用于保护删除、修改等关键操作，简单粗暴。 |
| **`Gate::allows()`** | **是否允许**访问？ | 返回 `false`，程序**继续执行**。 | 用于 `if` 判断，根据权限显示不同的数据结果。 |
| **`Gate::denies()`** | **是否拒绝**访问？ | 返回 `true` (如果没权限)，程序**继续执行**。 | 用于 `if` 判断，通常用于手动处理拒绝后的逻辑。 |


#### 5.4.1.6 核心笔记提醒

1. **自动识别用户**：在 Controller 里调用这些方法时，你**不需要**手动传入当前登录的用户。Laravel 框架会自动从当前的登录状态中抓取 `$user` 传给 Gate。
2. **位置建议**：`authorize` 建议写在函数的第一行。就像进大门要先刷脸一样，刷脸没过直接赶走，没必要浪费资源跑下面的业务逻辑。
3. **HTTP 状态码**：当 Gate 失败时，默认返回的是 **403 Forbidden**（禁止访问），这能准确告诉前端：我知道你是谁，但你没权力做这件事。


### 5.4.2 Policy 示例

Policy 是 Laravel 中**针对某个模型（Model）的权限控制方式**，更适合真实项目。

👉 在 JWT 架构中同样适用，因为 user 来源是 auth()->user()


🧠 **补充原理（非常重要）**

Policy 的本质是：

```text
Laravel 在你执行某个操作前，自动帮你调用一个“判断函数”
```

👉 你不用自己调用 Policy
👉 Laravel 会帮你调用


👉 例如流程是：

```text
Flutter 请求 → JWT解析 → auth()->user()
→ Controller → authorize → Policy → 返回 true/false
```


#### 5.4.2.1 创建 Policy

```bash
php artisan make:policy PostPolicy
```

🧠 作用说明：

👉 创建一个“权限规则文件”
👉 专门写：谁能做什么


#### 5.4.2.2 Policy 文件位置

```
app/Policies/PostPolicy.php
```

🧠 理解：

👉 这里就是“权限规则中心”


#### 5.4.2.3 示例：更新文章权限

```php
use App\Models\User;
use App\Models\Post;

class PostPolicy
{
    /**
     * @param User $user  -> 自动注入：当前登录的“人”
     * @param Post $post  -> 显式传入：被操作的“物”（某篇具体的文章）
     */
    public function update(User $user, Post $post)
    {
        // 🧠 核心逻辑：
        // 只有当这篇文章的 user_id 等于当前登录用户的 id 时，才准许修改
        return $user->id === $post->user_id;
    }
}
```

#### 5.4.2.4 深度拆解：这个 `$post` 是从哪来的？

很多小白会疑惑：我没在 Policy 里给它赋值，它怎么就有数据了？其实它是从你在 **Controller（控制器）** 里的调用传过来的。


**1. 在 Controller 中，你这样写：**
```php
public function update(Post $post) // 假设这是 ID 为 10 的文章
{
    // 你把 $post 丢给了 authorize 方法
    $this->authorize('update', $post); 
}
```

**2. 幕后发生了什么：**
* Laravel 看到你传了一个 `Post` 实例。
* 它会去 `PostPolicy` 里找 `update` 方法。
* 它把 **当前登录的人** 塞进第一个参数 `$user`。
* 它把你刚才传的 **那篇 ID 为 10 的文章** 塞进第二个参数 `$post`。



##### 💡 为什么必须要传这个参数？

如果不传 `$post`，Policy 就成了“瞎子”。

* **没有 $post 时**：你只能做通用判断。比如：“这个人是不是管理员？”（只能决定能不能发帖）。
* **有了 $post 时**：你可以做精准判断。比如：“这个人是不是**这篇编号为 99 的文章**的作者？”（能决定他能不能改别人的贴）。



##### 🧩 总结：参数的含义

* **第一个参数 `$user`**：由 Laravel 自动从 JWT/Session 中抓取，不需要你管。
* **第二个参数 `$post`**：是你**想要操作的那条数据库记录**。有了它，你才能写出 `$user->id === $post->user_id` 这种“作者才能改自己文章”的逻辑。



**🧠 记住这个公式：**
> **权限 = 谁（$user） + 做什么（update） + 对谁做（$post）**


#### 5.4.2.5 注册 Policy（如需手动）

👉 📍这个代码要写在：

```text
app/Providers/AuthServiceProvider.php
```


✏️ 正确写法如下（完整文件位置）

打开：

```php
app/Providers/AuthServiceProvider.php
```


然后在里面找到 `$policies`：

```php
<?php

namespace App\Providers;

use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;
use App\Models\Post;
use App\Policies\PostPolicy;

class AuthServiceProvider extends ServiceProvider
{
    /**
     * 🔐 模型与 Policy 的绑定关系
     */
    protected $policies = [
        // 👉 告诉 Laravel：
        // Post 这个模型 → 用 PostPolicy 来处理权限
        Post::class => PostPolicy::class,
    ];

    /**
     * 注册权限服务
     */
    public function boot()
    {
        $this->registerPolicies();
    }
}
```

👉 这一行代码：

```php
Post::class => PostPolicy::class,
```

翻译成人话就是：

```text
以后只要遇到 Post 相关权限问题
就去找 PostPolicy 这个文件
```


🧠 解释（很关键）：

👉 这一步是在“告诉 Laravel 绑定关系”

```text
Post 这个模型 → 用 PostPolicy 来判断权限
```


#### 5.4.2.6 控制器中使用 Policy

```php
public function update(Request $request, Post $post)
{
    // 🔥 核心一句：权限安检口
    // 💡 它的逻辑是：[ 没权限 ] → [ 抛异常 ] → [ 自动返回 403 响应 ]
    $this->authorize('update', $post);

    // -----------------------------------------------------------
    // 🛡️ 安全隔离线：
    // 只要代码能执行到这一行，说明上面的 authorize 已经通过了！
    // 如果校验失败，下面的代码（包括数据库操作）“绝对”不会被执行。
    // -----------------------------------------------------------

    // 🧠 内部运行机制：
    // 1. 自动提取：从请求中识别当前登录用户 $user（JWT 解析）。
    // 2. 自动匹配：根据 $post 类型找到对应的 PostPolicy。
    // 3. 自动调用：执行 Policy 里的 update($user, $post)。
    // 4. 自动处理：
    //    - 返回 true  → 继续执行。
    //    - 返回 false → 抛出 AuthorizationException。
    //    - 框架拦截异常 → 自动转换成 HTTP 403 状态码返回给前端。

    $post->update($request->all());

    return response()->json([
        'message' => '更新成功'
    ]);
}
```

##### 🎯 多种调用方式（按需选择）

除了上面的 `$this->authorize`，你还会经常看到以下写法，它们都是 Laravel 内置的：

* **方式 A：模型判断（最直观）**
    ```php
    // 这里的 can() 是 Laravel User 模型内置的方法
    // 语义：当前用户“能不能”对这个“帖子”进行“更新”操作
    if (auth()->user()->can('update', $post)) {
        // 校验通过
    }
    ```
* **方式 B：Gate 门面（通用型）**
    ```php
    if (Gate::allows('update', $post)) {
        // 校验通过
    }
    ```


##### 🧩 Policy 常用内置方法对照表

当你使用命令 `php artisan make:policy PostPolicy --model=Post` 时，Laravel 会预设以下方法。它们的名称与 Controller 的动作是**一一对应**的：

| Policy 方法名 | 对应场景 | 逻辑参考 (Return) |
| :--- | :--- | :--- |
| **viewAny** | 文章列表页 | `return true;` (通常所有人都能看) |
| **view** | 文章详情页 | `return true;` |
| **create** | 发布新文章 | `return $user->role !== 'banned';` (没被封号就能发) |
| **update** | 修改文章 | `return $user->id === $post->user_id;` (**核心：只能改自己的**) |
| **delete** | 删除文章 | `return $user->role === 'admin' \|\| $user->id === $post->user_id;` |
---

> **💡 小白避坑：**  像 `create` 和 `viewAny` 这种不需要指定某篇特定文章的操作，调用时要传入**类名**：
> `$this->authorize('create', Post::class);`


##### ❌ 对比：传统手动判断方式（不推荐）

```php
// 这种写法虽然能用，但不够“Laravel”
if ($post->user_id !== auth()->id()) {
    abort(403, '你没有权限');
}
```

🧠 **为什么不用这种？**
1.  **逻辑分散**：权限逻辑写死在 Controller 里，以后改规则要到处找。
2.  **重复劳动**：每个方法都要写一遍 `if`。
3.  **不够优雅**：无法复用给 Blade 模板或 API 自动过滤。


**核心总结（一句话记住）**

> **Policy** 是权限说明书，**$this->authorize** 是拿着说明书去对号入座的检查员。

🧠 **核心总结（这一段必须懂）**

```text
Policy = 一堆规则函数
Laravel = 自动帮你调用这些规则
JWT = 提供当前 user
```


#### 5.4.2.7 Blade 中使用 Policy

```php
@can('update', $post)
    <button>编辑</button>
@endcan

@can('delete', $post)
    <button>删除</button>
@endcan
```


🧠 Blade 在做什么？

```text
渲染页面时：
Laravel 自动调用 Policy 判断
true → 显示按钮
false → 不显示
```


### 5.4.3 Flutter 控制 UI

Flutter 端的权限控制主要用于：

> 👉 控制按钮显示（提升体验）
> 👉 但不能作为安全控制（必须后端验证）


#### 5.4.3.1 基础示例

```dart
if (user.role == 'admin') {
  return ElevatedButton(
    onPressed: () {
      // 调用删除接口
    },
    child: Text("删除"),
  );
}
```


#### 5.4.3.2 说明

Flutter 判断只是：

* 控制 UI 是否显示
* 提升用户体验
* 防止误操作

⚠️ 但不能防止接口被直接调用


#### 5.4.3.3 推荐写法：封装权限工具

```dart
class Permission {
  static bool isAdmin(User user) {
    return user.role == 'admin';
  }

  static bool canDeletePost(User user, Post post) {
    return user.role == 'admin' || user.id == post.userId;
  }
}
```


#### 5.4.3.4 UI 使用方式

```dart
if (Permission.canDeletePost(user, post)) {
  return IconButton(
    icon: Icon(Icons.delete),
    onPressed: () {
      // 调用删除接口
    },
  );
}
```

### 5.4.4 正确架构理解（非常重要）

| 层级                  | 作用       |
| ------------------- | -------- |
| Flutter             | 控制 UI 显示 |
| Laravel Gate/Policy | 权限核心控制   |
| API Controller      | 最终安全验证   |


#### ⚠️ 常见错误

- ❌ 只在 Flutter 控制权限
- ❌ 不在后端做权限判断
- ❌ 认为隐藏按钮就安全


#### ✅ 正确做法

- ✔ Flutter 控制显示
- ✔ Laravel 控制权限
- ✔ API 做最终拦截


## 5.5 Token 生命周期管理

### 5.5.1 过期时间配置

JWT 的安全性核心在于“过期时间”。
- **Sanctum (内置)** 修改 `config/sanctum.php`。
- **JWT-Auth (三方)** 修改 `config/jwt.php`。

```php
// config/jwt.php 示例
// 以分钟为单位
'ttl' => 60,               // Token 有效期（分钟）：建议设短一点
// 如果需要设置 7 天，可写为：60 * 24 * 7 

'refresh_ttl' => 20160,    // 刷新时长（分钟）：在此时间内可用旧 Token 换新 Token
```
> **🧠 为什么？** JWT 一旦发出，服务器无法撤回。设置较短的过期时间能将风险降到最低。


### 5.5.2 运行机制：后端会自动检查吗？

**答案是：肯定的。** 但这依赖于 Laravel 的 **Middleware（中间件）**。

* **自动化拦截**：你不需要在 Controller 里写任何判断过期的代码。只要路由被 `auth:api` 中间件包裹，Laravel 会在请求进入你的代码之前，自动解析 Header 里的 JWT 并检查 `exp`（过期时间）字段。
* **什么是“包裹”？（代码演示）**：
    在 `routes/api.php` 中，使用中间件组将需要保护的接口“套”起来：
    ```php
    // 🛡️ 这里的 middleware('auth:api') 就是安检门
    Route::middleware('auth:api')->group(function () {
        
        // 凡是在这个花括号里的路由，都被“包裹”了
        // 它们都会被自动检查 JWT 是否存在、是否过期、是否合法
        Route::put('/posts/{post}', [PostController::class, 'update']);
        Route::delete('/posts/{post}', [PostController::class, 'destroy']);
        
    });

    // 🔓 没被包裹的路由，则不会检查 Token（如文章列表）
    Route::get('/posts', [PostController::class, 'index']);
    ```

* **处理结果**：
    * **未过期**：通过安检，正常执行你的控制器代码。
    * **已过期**：**直接中断执行**。Laravel 的异常处理器会立即介入，代码运行指针根本**无法进入**控制器内部，下方的数据库操作（如 `update`）绝对不会运行。
    * **返回响应**：后端会自动向 Flutter 返回 **HTTP 401 (Unauthorized)** 状态码。


> **🧠 深度理解：**
> 你可以把中间件想象成**“关卡”**。
> 1. Flutter 发起请求 -> 2. 到达 `auth:api` 关卡 -> 3. 检查过期？
> - ❌ 检查到过期 -> 关卡处直接拦截并返回 401 -> **请求结束**。
> - ✅ 检查通过 -> 关卡抬杆放行 -> 进入控制器执行逻辑 -> **请求成功**。
> 
> 这就是为什么你的 `update` 方法里不需要写任何 `if(token_expired)` 的原因——**因为能活到那一行的请求，全都是合法的。**


**💡 小贴士：**
在开发调试时，如果发现即使 Token 错了也能访问，请务必检查 `routes/api.php`，确认该路由是否真的被放进了 `middleware('auth:api')` 的组里。


### 5.5.3 自定义失败响应

如果 Laravel 默认的返回格式不符合前端需求（例如 Flutter 需要特定的 JSON 结构，如增加 `code` 字段），可以通过修改后端的 **全局异常处理器** 来统一定制。

### 5.5.3.1 修改文件： `app/Exceptions/Handler.php`

### 5.5.3.2 实现代码：

```php
use Illuminate\Auth\AuthenticationException;
use Illuminate\Http\Request;

public function register()
{
    // 🧠 核心逻辑：拦截身份验证异常
    $this->renderable(function (AuthenticationException $e, Request $request) {
        
        // 只有 API 请求（来自 Flutter/Postman 等）才返回自定义 JSON
        if ($request->is('api/*')) {
            return response()->json([
                'code'    => 114514,
                'message' => '登录状态已过期，请重新登录',
                'data'    => null,
            ], 401);
        }
    });
}
```
### 5.5.3.3 运行机制：
* **捕获异常**：当路由上的 `auth:api` 中间件校验 Token 失败时，它会抛出一个 `AuthenticationException`。
* **重写渲染**：Handler 捕获到这个“异常信号”，按照我们定义的 JSON 格式进行包装，而不是返回系统默认的错误文本。

> **⚠️ 注意：** > 尽管自定义了 JSON 的内容，但建议 **HTTP 状态码依然保持 401**。这样 Flutter 的网络请求库（如 Dio）可以通过 `statusCode` 第一时间识别出是“权限/认证问题”。

**核心总结：**
后端就像安检员，你可以决定当安检不通过时，是冷冰冰地关门，还是送上一张前端能读懂的“温馨提示卡”。通过 `Handler.php`，你可以随心所欲地定制这张“卡片”的内容。

#### 5.5.3.4 `$this->renderable` 是从哪来的？

很多小白会好奇，这个 `renderable` 既不是在当前类定义的，也没有看到 `import`，它是怎么生效的？

**1. 家族传承（继承体系）**
你的 `app/Exceptions/Handler.php` 并不是孤立存在的，它继承自 Laravel 框架的核心类：
```php
class Handler extends ExceptionHandler // 这里的 ExceptionHandler 是框架内核提供的
```
`renderable` 方法就定义在父类 `Illuminate\Foundation\Exceptions\Handler` 中。它是 Laravel 8.x 之后引入的一项“黑科技”，专门用于简化 API 的异常处理。

**2. 核心原理：类型注入（Type Hinting）**
这是 Laravel 最优雅的设计之一。注意闭包函数中的第一个参数：
```php
$this->renderable(function (AuthenticationException $e, Request $request) { ... })
```
* **自动匹配**：Laravel 会利用 PHP 的反射机制，扫描你闭包里填写的异常类名（如 `AuthenticationException`）。
* **精准拦截**：当程序抛出异常时，Laravel 会检查该异常是否是你指定的类型。如果是，就执行你的自定义逻辑；如果不是，就跳过，交给下一个处理器。


**3. 为什么它是“现代写法”？**
在 Laravel 的早期版本中，你必须在一个巨大的 `render()` 方法里写满 `if ($e instanceof ...)`。而现在的 `renderable` 允许你像挂载“插件”一样，为不同的异常单独写处理逻辑：
* 想处理 Token 过期？挂一个 `renderable`。
* 想处理数据库找不到记录（404）？再挂一个 `renderable`。
* 彼此独立，互不干扰，代码极度整洁。

**4. 总结：它在 JWT 架构中的角色**
在 **Flutter + JWT** 的开发中，`renderable` 充当了“翻译官”的角色：
> 它把后端冷冰冰的 **PHP 异常**，翻译成了前端 Flutter 能听懂的 **JSON 暗号**（如你自定义的 `code: 114514`）。

---

**💡 提示**：如果你在代码里写完发现报错，请检查文件顶部是否正确引入了命名空间：
`use Illuminate\Auth\AuthenticationException;`

---

**💡 小贴士：** 修改 `config/jwt.php` 后，记得运行 `php artisan config:clear` 确保配置立即生效！

## 5.6 安全最佳实践

### 5.6.1 强制 HTTPS
在生产环境下，必须使用 HTTPS 来防止 JWT 在传输过程中被截获（中间人攻击）。
```env
# .env 文件
APP_URL=https://yourdomain.com
```

### 5.6.2 限流（防暴力破解）
防止脚本无限次尝试登录密码或刷新 Token。
```php
// routes/api.php
Route::post('/login', [AuthController::class, 'login'])
    ->middleware('throttle:5,1'); // 每 1 分钟最多尝试 5 次
```

### 5.6.3 生产环境不暴露内部错误
严禁将数据库报错或代码行号直接返回给前端，防止泄露服务器路径或表结构。

❌ **错误写法（泄露机密）：**
```php
return response()->json(['error' => $e->getMessage()], 500); 
// 可能会返回 "SQLSTATE[HY000]: Column not found..."
```

✅ **正确写法（模糊化处理）：**
```php
// 只给前端一个模糊的提示，具体的错误日志写进 storage/logs/laravel.log
return response()->json([
    'message' => '服务器开小差了，请稍后再试'
], 500);
```

## 5.7 总结

### 5.7.1 JWT 登录流程

```text
1. 用户登录
2. Laravel 生成 token
3. Flutter 保存 token
4. 每次请求带 token
5. 后端验证 token
```

### 5.7.2 对比 Sanctum

| 项目          | Sanctum | JWT   |
| ----------- | ------- | ----- |
| 是否存 session | 是       | 否     |
| Flutter适配   | 一般      | ⭐⭐⭐⭐⭐ |
| 性能          | 一般      | 高     |
| 推荐          | Web     | App   |


# 6. Laravel + Flutter CRUD

本章节将带你从 **0基础** 学会：

* Laravel 后端如何写 CRUD API
* Flutter 如何调用接口
* 前后端如何联动


## 6.1 什么是 CRUD？

CRUD = 四个基本操作：

| 操作     | 含义   |
| ------ | ---- |
| Create | 创建数据 |
| Read   | 读取数据 |
| Update | 更新数据 |
| Delete | 删除数据 |

我们将实现一个简单的「商品管理系统」。


## 6.2 Laravel 后端部分

### 6.2.1 创建项目

#### 6.2.1.1 默认端口
```bash
composer create-project laravel/laravel my_project
cd my_project
php artisan serve
```

访问：

```
http://127.0.0.1:8000
```

#### 6.2.1.2 自定义端口
```bash
composer create-project laravel/laravel my_project
cd my_project
php artisan serve --port=7890
```

访问：

```
http://127.0.0.1:7890
```

### 6.2.2 配置数据库

打开 `.env`

```env
DB_DATABASE=my_db
DB_USERNAME=root
DB_PASSWORD=123456
```

然后执行迁移：

```bash
php artisan migrate
```


### 6.2.3 创建模型 + 迁移

```bash
php artisan make:model Product -m
```

#### 修改迁移文件

```php
public function up()
{
    Schema::create('products', function (Blueprint $table) {
        $table->id(); // 主键
        $table->string('name'); // 商品名
        $table->decimal('price', 8, 2); // 价格
        $table->text('description')->nullable(); // 描述
        $table->timestamps(); // 创建时间 + 更新时间
    });
}
```

执行：

```bash
php artisan migrate
```


### 6.2.4 创建控制器

```bash
php artisan make:controller Api/ProductController
```


### 6.2.5 编写 CRUD API
#### 6.2.5.1 代码模板：
```php
namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use App\Models\Product;

class ProductController extends Controller
{
    // 获取所有数据 (READ)
    public function index()
    {
        return response()->json(Product::all());
    }

    // 创建数据 (CREATE)
    public function store(Request $request)
    {
        // 验证
        $request->validate([
            'name' => 'required',
            'price' => 'required|numeric'
        ]);

        // 创建
        $product = Product::create($request->all());

        return response()->json($product);
    }

    // 查看单个
    public function show($id)
    {
        return Product::findOrFail($id);
    }

    // 更新 (UPDATE)
    public function update(Request $request, $id)
    {
        $product = Product::findOrFail($id);
        $product->update($request->all());

        return response()->json($product);
    }

    // 删除 (DELETE)
    public function destroy($id)
    {
        Product::destroy($id);

        return response()->json(['message' => 'deleted']);
    }
}
```

#### 6.2.5.2 详细讲解：
```php
namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use App\Models\Product;

class ProductController extends Controller
{
    // ========================
    // 1️⃣ 查询所有数据（READ）
    // ========================
    public function index()
    {
        // Product::all()
        // 👉 查询数据库 products 表里的“所有数据”
        
        // response()->json()
        // 👉 返回 JSON 格式给前端（Flutter）
        return response()->json(Product::all());
    }


    // ========================
    // 2️⃣ 创建数据（CREATE）
    // ========================
    public function store(Request $request)
    {
        // $request = 前端传过来的数据（JSON）

        // ✅ Laravel 内置：数据验证（非常强！）
        $request->validate([
            'name' => 'required',        // 必填
            'price' => 'required|numeric' // 必填 + 必须是数字
        ]);

        // ✅ Laravel ORM（Eloquent）
        // create() = 插入数据库
        // $request->all() = 获取所有字段
        $product = Product::create($request->all());

        // 返回创建后的数据
        return response()->json($product);
    }


    // ========================
    // 3️⃣ 查询单个数据（READ 单个）
    // ========================
    public function show($id)
    {
        // findOrFail()
        // 👉 根据 id 查找
        // 👉 找不到会自动返回 404（超方便！）
        return Product::findOrFail($id);
    }


    // ========================
    // 4️⃣ 更新数据（UPDATE）
    // ========================
    public function update(Request $request, $id)
    {
        // 假设数据库里这条数据 ID 为 5，原始数据是：{"name": "旧手机", "price": 1000}
        $product = Product::findOrFail($id);

        /**
         * 💡 情况 A：前端只改价格 (例如在 Postman 发送了 {"price": 888})
         * $request->all() 拿到的就是 ['price' => 888]
         * update() 后：name 还是 "旧手机"，但 price 变成了 888
         */

        /**
         * 💡 情况 B：前端修改全部 (例如在 Flutter 发送了 {"name": "新手机", "price": 2000})
         * $request->all() 拿到的就是 ['name' => '新手机', 'price' => 2000]
         * update() 后：两个字段都会被更新
         */

        /**
         * 💡 情况 C：前端传了干扰字段 (例如 {"name": "手机", "hack": "123"})
         * 如果你在 Model 的 $fillable 里没写 'hack'
         * update() 会自动过滤掉 'hack'，只更新 'name'，非常安全！
         */

        // 执行更新
        $product->update($request->all());

        // 返回更新后的最新数据给前端
        return response()->json($product);
    }


    // ========================
    // 5️⃣ 删除数据（DELETE）
    // ========================
    public function destroy($id)
    {
        // destroy()
        // 👉 根据 id 删除数据
        Product::destroy($id);

        return response()->json([
            'message' => 'deleted'
        ]);
    }
}
```

#### 6.2.5.3 Laravel CRUD 核心函数总结

| 操作类型 | 控制器方法 | Eloquent 内置函数 | 对应 HTTP 请求方法 | 功能说明 |
| :--- | :--- | :--- | :--- | :--- |
| **Read (全查)** | `index()` | `Product::all()` | `GET` | 获取表中的所有记录，返回集合。 |
| **Create (新增)** | `store()` | `Product::create()` | `POST` | 将请求数据存入数据库（需配置 $fillable）。 |
| **Read (单查)** | `show()` | `Product::findOrFail($id)` | `GET` | 查找指定 ID，找不到则直接抛出 404 错误。 |
| **Update (更新)** | `update()` | `$model->update()` | `PUT / PATCH` | 找到实例后，根据新数据更新字段。 |
| **Delete (删除)** | `destroy()` | `Product::destroy($id)` | `DELETE` | 根据主键 ID 直接删除记录。 |


#### 6.2.5.4 补充：批量删除
- Controller 写法：
```php
public function batchDelete(Request $request)
{
    $ids = $request->input('ids');

    if (!$ids || !is_array($ids)) {
        return response()->json([
            'message' => 'Invalid IDs'
        ], 400);
    }

    // 批量删除
    $deleted = Product::whereIn('id', $ids)->delete();

    return response()->json([
        'message' => 'Deleted successfully',
        'deleted_count' => $deleted
    ]);
}
```
- 路由配置：
```php
Route::delete('/products/batch-delete', [ProductController::class, 'batchDelete']);
```
- 前端请求：Flutter 用 Dio 可以这样传👇
```json
DELETE /api/products/batch-delete

{
  "ids": [1, 2, 3, 4]
}
```


### 6.2.6 添加 HTTP 状态码
在基于 Laravel 的 CRUD API 开发过程中，合理设置 HTTP 状态码是接口设计的重要组成部分。状态码不仅用于描述请求处理结果，还能帮助前端（如 Flutter + Dio）快速判断请求是否成功，从而进行相应的业务处理。


#### 6.2.6.1 状态码的作用

HTTP 状态码用于表示服务器对请求的处理结果，主要作用包括：

* 标识请求是否成功（如 200、201）
* 标识客户端错误（如 400、404）
* 标识服务器错误（如 500）
* 提供统一的前后端交互语义


#### 6.2.6.2 Laravel 中设置状态码的方法

Laravel 提供了统一的响应构造方式，可通过 `response()` 辅助函数设置状态码：

```php
return response()->json([
    'message' => 'success'
], 200);
```

其中：

* 第一个参数为返回数据
* 第二个参数为 HTTP 状态码


#### 6.2.6.3 常用状态码规范

在本项目中，推荐使用如下状态码约定：

| 状态码 | 含义                    | 使用场景         |
| --- | --------------------- | ------------ |
| 200 | OK                    | 请求成功（查询、删除等） |
| 201 | Created               | 创建成功         |
| 204 | No Content            | 删除成功且无返回数据   |
| 400 | Bad Request           | 参数错误         |
| 401 | Unauthorized          | 未登录或认证失败     |
| 403 | Forbidden             | 无权限          |
| 404 | Not Found             | 资源不存在        |
| 500 | Internal Server Error | 服务器异常        |


#### 6.2.6.4 批量删除接口示例

##### （1）返回删除结果

```php
return response()->json([
    'deleted_count' => $deleted
], 200);
```

##### （2）无返回内容（推荐 RESTful 风格）

```php
return response()->noContent(); // 状态码 204
```

---

#### 6.2.6.5 错误响应示例

```php
return response()->json([
    'message' => 'Invalid IDs'
], 400);
```


#### 6.2.6.6 前端（Flutter）对状态码的处理

在 Flutter 中使用 Dio 发起请求时，状态码会自动包含在响应对象中：

```dart
Response response = await dio.delete('/api/products');

print(response.statusCode);
```

需要注意：

* 当状态码为 **2xx** 时，Dio 默认认为请求成功
* 当状态码为 **非 2xx** 时，Dio 会抛出异常（DioException）

示例：

```dart
try {
  final response = await dio.delete('/api/products');

  if (response.statusCode == 200) {
    // 处理成功逻辑
  }

} on DioException catch (e) {
  final statusCode = e.response?.statusCode;
  // 处理错误逻辑
}
```

#### 6.2.6.7 推荐的统一响应结构

为了提高接口可维护性，建议在返回数据中增加业务状态码字段：

```php
return response()->json([
    'code' => 0,
    'message' => 'success',
    'data' => null
], 200);
```

说明：

* HTTP 状态码：用于表示请求层级结果
* `code` 字段：用于表示业务逻辑结果
* `message`：提示信息
* `data`：返回数据


### 6.2.7 设置模型可填充字段和隐藏字段

打开 `app/Models/Product.php`

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    // ✅ 【白名单】允许批量赋值的字段
    // 只有在这里注册了，create() 和 update() 才能生效
    protected $fillable = [
        'name',
        'price',
        'description'
    ];

    // ✅ 【黑名单】转 JSON 时自动隐藏的字段
    /*
      只要是在这里定义的字段，无论你在控制器里怎么写，都不会暴露给前端。
      常见的返回方式包括：
      1. return Product::all();                 // 返回多个对象的集合
      2. return Product::findOrFail($id);       // 返回单个对象
      3. return response()->json($products);    // 显式转为 JSON 响应
      
      原理：Laravel 在将 Eloquent 对象序列化（转字符串）时，
      底层会通过 toArray() 自动剔除此列表中的字段。
    */
    protected $hidden = [
        'created_at',   
        'updated_at',   
        'internal_code' // 假设你有内部备注或敏感逻辑字段
    ];
}
```

### 6.2.8 配置路由

- 打开 `routes/api.php`
- 模板代码：
```php
use App\Http\Controllers\Api\ProductController;

Route::get('/products', [ProductController::class, 'index']);
Route::post('/products', [ProductController::class, 'store']);
Route::get('/products/{id}', [ProductController::class, 'show']);
Route::put('/products/{id}', [ProductController::class, 'update']);
Route::delete('/products/{id}', [ProductController::class, 'destroy']);
```
- 详细讲解：

```php
use App\Http\Controllers\Api\ProductController; 
// ↑ 引入控制器（必须写完整命名空间）

// ========================
// 基础 CRUD 路由写法说明
// ========================

// Route::请求方式('URL路径', [控制器类::class, '方法名']);

// 1️⃣ 获取所有商品（READ）
Route::get('/products', [ProductController::class, 'index']);
// GET 请求
// 访问：http://127.0.0.1:8000/api/products
// 调用：ProductController 里的 index() 方法


// 2️⃣ 创建商品（CREATE）
Route::post('/products', [ProductController::class, 'store']);
// POST 请求
// 用于提交数据（新增）
// 数据从 body 传过去（JSON）


// 3️⃣ 获取单个商品（READ 单个）
Route::get('/products/{id}', [ProductController::class, 'show']);
// {id} = 动态参数（变量）
// 例如：/products/1
// Laravel 会自动把 1 传给 show($id)


// 4️⃣ 更新商品（UPDATE）
Route::put('/products/{id}', [ProductController::class, 'update']);
// PUT 请求（也可以用 PATCH）
// 表示修改某一条数据
// 同样通过 {id} 指定是哪一条


// 5️⃣ 删除商品（DELETE）
Route::delete('/products/{id}', [ProductController::class, 'destroy']);
// DELETE 请求
// 删除指定 id 的数据
```

## 6.3 使用 Bruno 测试 API

### 6.3.1 创建

```
POST /api/products
```

Body:

```json
{
  "name": "iPhone",
  "price": 9999
}
```


## 6.4 Flutter 前端部分

### 6.4.1 添加依赖

```yaml
dependencies:
  dio: ^5.0.0
```


### 6.4.2 创建 API 类

```dart
import 'package:dio/dio.dart';

class ApiService {
  final Dio dio = Dio(BaseOptions(
    baseUrl: 'http://127.0.0.1:8000/api'
  ));

  // 获取列表
  Future getProducts() async {
    return await dio.get('/products');
  }

  // 添加
  Future addProduct(Map data) async {
    return await dio.post('/products', data: data);
  }

  // 更新
  Future updateProduct(int id, Map data) async {
    return await dio.put('/products/$id', data: data);
  }

  // 删除
  Future deleteProduct(int id) async {
    return await dio.delete('/products/$id');
  }
}
```


### 6.4.3 显示列表

```dart
class ProductPage extends StatefulWidget {
  @override
  _ProductPageState createState() => _ProductPageState();
}

class _ProductPageState extends State<ProductPage> {
  List products = [];
  final api = ApiService();

  @override
  void initState() {
    super.initState();
    loadData();
  }

  void loadData() async {
    var res = await api.getProducts();
    setState(() {
      products = res.data;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('商品列表')),
      body: ListView.builder(
        itemCount: products.length,
        itemBuilder: (_, i) {
          var item = products[i];
          return ListTile(
            title: Text(item['name']),
            subtitle: Text(item['price'].toString()),
          );
        },
      ),
    );
  }
}
```


### 6.4.4 添加数据

```dart
api.addProduct({
  "name": "MacBook",
  "price": 15000
});
```


### 6.4.5 删除数据

```dart
api.deleteProduct(1);
```



## 6.5 常见坑（非常重要）

### 6.5.1 跨域问题

安装：

```bash
composer require fruitcake/laravel-cors
```



### 6.5.2 访问地址问题

手机不能用：

```
127.0.0.1
```

必须改为：

```
电脑IP
```


### 6.5.3 端口问题

确保：

```
8000 已开放
```

# 7. Dcat Admin
明白了！既然你要**快点入门 Dcat Admin**，而且前端是 **Flutter**，那我们的策略很明确：

1.  **后端管理 (Web)**：用 Dcat Admin 快速搭建，用于你（管理员）在电脑上审数据、配置 App 内容。
2.  **前端接口 (App)**：用 Laravel 写 API（JWT 认证），供 Flutter 调用。

以下是 Dcat Admin 的**光速入门指南**：

## 7.1 第一步：安装 Dcat Admin

假设你已经有一个安装好的 Laravel 项目：

```bash
# 1. 引入 composer 包
composer require dcat/laravel-admin

# 2. 发布资源并运行安装 (它会自动建立管理员表和权限表)
php artisan admin:publish
php artisan admin:install
```

👉 安装完成后，启动服务器 `php artisan serve`，然后访问：`http://localhost:8000/admin`
* **默认账号：** `admin`
* **默认密码：** `admin`


## 7.2 第二步：一键生成 CRUD (核心大招)

你不需要手写代码！Dcat Admin 有一个 **“代码生成器”**。

1.  登录后台，找到 **工具 -> 代码生成器**。
2.  填写你的表名（比如 `posts`），勾选“创建迁移文件”、“创建模型”、“创建控制器”、“创建翻译文件”。
3.  点击 **提交**。
4.  **运行迁移**（如果没点自动运行的话）：`php artisan migrate`。
5.  **添加菜单**：在后台 **系统 -> 菜单** 里，把新生成的路由路径（比如 `posts`）加进去。

**现在，你的 Post 管理后台（增删改查）就已经全部跑通了！**


## 7.3 第三步：理解 Dcat 的核心代码结构

生成的控制器在 `app/Admin/Controllers/PostController.php`。你会看到三个核心方法：

* **`grid()`**：控制**列表页**显示哪些列、哪些搜索框。
* **`form()`**：控制**新增/编辑页**有哪些表单项（文本、图片、日期等）。
* **`detail()`**：控制**详情页**显示。

**常用组件示例：**
```php
protected function form()
{
    return Form::make(new Post(), function (Form $form) {
        $form->display('id');
        $form->text('title', '标题')->required(); // 文本框
        $form->image('cover', '封面图')->uniqueName(); // 图片上传
        $form->editor('content', '正文'); // 富文本编辑器
        $form->switch('is_public', '是否公开'); // 开关
    });
}
```


## 7.4 第四步：同步给 Flutter 提供接口

这就是你现在的双轨并行：

1.  **管理数据**：你通过 `app/Admin/Controllers/PostController.php` 录入新数据。
2.  **展示数据**：你在 `app/Http/Controllers/Api/PostController.php`（之前用 `-ma` 生成的）里写逻辑，把数据传给 Flutter。

**代码逻辑共享：**
因为它们都共用同一个模型 `app/Models/Post.php`。你在后台改了数据，Flutter 那边调 API 看到的就是最新的。



## 7.5 Flutter 开发者的特别提醒

1.  **文件上传路径**：
    Dcat Admin 默认把图片上传到 `storage/app/public/`。
    * 记得运行 `php artisan storage:link`。
    * 在给 Flutter 返回 JSON 数据时，图片路径记得加上 `url()` 函数，变成完整的 `http://.../xxx.jpg`，否则 Flutter 显示不出来。

2.  **接口测试**：
    既然你现在有后台了，你可以先在 Dcat Admin 里手动录入几条测试数据，然后再去调你的 API。这样你写 Flutter 的时候，就有真实的 JSON 数据可以解析了。

**现在的进度：**
你已经安装好 Dcat 并且能看到登录页面了吗？如果有报错（比如数据库连接），记得检查 `.env` 文件。