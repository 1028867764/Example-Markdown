# 0. 前言
Laravel 是一个流行的开源 PHP Web 开发框架，由 Taylor Otwell 创建，主打“优雅语法 + 高开发效率”。

## 0.1 🧩 一句话理解

Laravel = **帮你快速开发网站/接口的 PHP 框架**，把常见功能都帮你封装好了。


## 0.2 ⚙️ 核心特点

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


## 0.3 🚀 适合做什么

* 后端 API（比如给 Flutter 用 👍）
* 管理系统（CMS、后台）
* 电商网站
* 博客系统


## 0.4 🧠 学习建议（结合你当前情况）

你现在：

* ✅ 会 Flutter
* ✅ 正在做 Django + DRF
* ✅ 想尝试 Laravel

👉 推荐路线：

1. 先用 Laravel 做 **简单 API**
2. 用 Flutter 调接口（你已经会）
3. 对比 Django 和 Laravel 的差异


## 0.5 📊 简单对比

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

#### 安装步骤：

1. 下载 **Laravel Herd**
2. 安装完成后，它会自动帮你：

   * 安装 PHP
   * 配置环境变量
   * 启动本地服务器



#### 创建 Laravel 项目

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



#### 启动项目

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



#### 初始化 Sail

```bash
curl -s "https://laravel.build/my_project" | bash
cd my_project
./vendor/bin/sail up
```



#### 运行 Artisan

```bash
./vendor/bin/sail artisan list
```

👉 注意：Docker 里必须用 `sail artisan`



### 1.1.3 Composer 是什么？

👉 PHP 世界的“npm”



#### 常用命令：

```bash
composer install     # 安装依赖
composer update      # 更新依赖
composer require xxx # 安装新包
```



## 1.2 目录结构详解（重点！！！🔥）

Laravel 项目结构非常清晰，必须掌握。



### 1.2.1 📁 app/ —— 核心代码区

👉 你写的“业务逻辑”基本都在这里



#### 示例结构：

```
app/
 ├── Models/
 ├── Http/
 │    ├── Controllers/
 │    └── Middleware/
```



#### ✅ 示例：创建一个控制器

```bash
php artisan make:controller UserController
```

生成：

```
app/Http/Controllers/UserController.php
```



#### 控制器代码示例（带注释）

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



#### 关键文件：

```
routes/web.php   # 浏览器访问
routes/api.php   # API 接口
```



#### 示例：定义路由

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



#### 路由解释：

```php
Route::get('/users', ...)  
// GET 请求
// URL: /users

Route::get('/users/{id}', ...)
// {id} 是动态参数
```



### 1.2.3 📁 resources/ —— 视图与前端资源

👉 用来写页面（Blade 模板）



#### 示例结构：

```
resources/
 ├── views/
 │    ├── welcome.blade.php
```



#### Blade 示例：

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



#### 控制器返回视图：

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



#### 1️⃣ 创建控制器

```bash
php artisan make:controller PostController
```



#### 2️⃣ 创建模型

```bash
php artisan make:model Post
```



#### 3️⃣ 创建模型 + 迁移 + 控制器（超常用）

```bash
php artisan make:model Post -mcr
```

👉 等价于：

* `-m` → migration（数据库表）
* `-c` → controller
* `-r` → resource controller



#### 生成结果：

```
app/Models/Post.php
database/migrations/xxxx_create_posts_table.php
app/Http/Controllers/PostController.php
```



#### 4️⃣ 启动服务器

```bash
php artisan serve
```



#### 5️⃣ 数据库迁移

```bash
php artisan migrate
```



#### 示例：迁移文件

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



#### 6️⃣ 清缓存（开发必备）

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

### 2.1.1 什么是路由？

👉 路由的作用：**决定 URL 请求由谁处理**

例如：

```
用户访问：http://your-app.com/user/1
↓
Laravel 路由接收
↓
交给某个控制器处理
↓
返回数据/页面
```



### 2.1.2 `web.php` 与 `api.php` 的区别

📁 路径：

```
routes/web.php   // 用于网页（返回 HTML）
routes/api.php   // 用于 API（返回 JSON）
```

#### 1️⃣ web.php 示例（返回页面）

```php
// routes/web.php

use Illuminate\Support\Facades\Route;

// 访问 http://localhost/hello
Route::get('/hello', function () {
    return view('hello'); // 返回 resources/views/hello.blade.php 页面
});
```



#### 2️⃣ api.php 示例（返回 JSON）

```php
// routes/api.php

use Illuminate\Support\Facades\Route;

// 访问 http://localhost/api/user
Route::get('/user', function () {
    return [
        'name' => 'Tom',
        'age' => 18
    ]; // 自动返回 JSON
});
```



### 2.1.3 路由参数

#### 1️⃣ 必选参数

```php
Route::get('/user/{id}', function ($id) {
    return "用户ID是：" . $id;
});
```

访问：

```
/user/10
输出：用户ID是：10
```



#### 2️⃣ 可选参数

```php
Route::get('/user/{name?}', function ($name = 'Guest') {
    return "用户名：" . $name;
});
```

访问：

```
/user        → 用户名：Guest
/user/Tom    → 用户名：Tom
```



#### 3️⃣ 正则约束（限制参数格式）

```php
Route::get('/user/{id}', function ($id) {
    return "ID：" . $id;
})->where('id', '[0-9]+'); // 只能是数字
```



### 2.1.4 控制器（Controller）

👉 控制器 = 专门写业务逻辑的地方



#### 1️⃣ 创建控制器

```bash
php artisan make:controller UserController
```

生成文件：

```
app/Http/Controllers/UserController.php
```



#### 2️⃣ 控制器代码示例

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    // 显示用户列表
    public function index()
    {
        return "用户列表";
    }

    // 显示单个用户
    public function show($id)
    {
        return "用户ID：" . $id;
    }
}
```



#### 3️⃣ 路由绑定控制器

```php
use App\Http\Controllers\UserController;

// 访问 /users
Route::get('/users', [UserController::class, 'index']);

// 访问 /user/1
Route::get('/user/{id}', [UserController::class, 'show']);
```



## 2.2 数据库与模型 (Eloquent ORM)

Laravel 最强大的地方之一：👉 **ORM（对象关系映射）**



### 2.2.1 Migrations（数据库迁移）

👉 用代码管理数据库结构（类似 Git）



#### 1️⃣ 创建迁移文件

```bash
php artisan make:migration create_users_table
```



#### 2️⃣ 迁移文件示例

```php
// database/migrations/xxxx_create_users_table.php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    // 创建表
    public function up(): void
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id(); // 主键 ID（自增）
            $table->string('name'); // 字符串字段
            $table->string('email')->unique(); // 唯一字段
            $table->timestamps(); // created_at 和 updated_at
        });
    }

    // 回滚（删除表）
    public function down(): void
    {
        Schema::dropIfExists('users');
    }
};
```



#### 3️⃣ 执行迁移

```bash
php artisan migrate
```



### 2.2.2 Eloquent ORM（模型）

👉 一张表 = 一个 Model 类



#### 1️⃣ 创建模型

```bash
php artisan make:model User
```



#### 2️⃣ 模型示例

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    // 允许批量赋值的字段
    protected $fillable = ['name', 'email'];
}
```



#### 3️⃣ 增删改查（CRUD）

##### ✅ 创建数据

```php
User::create([
    'name' => 'Tom',
    'email' => 'tom@example.com'
]);
```



##### ✅ 查询数据

```php
// 查询所有
$users = User::all();

// 查询单条
$user = User::find(1);

// 条件查询
$user = User::where('name', 'Tom')->first();
```



##### ✅ 更新数据

```php
$user = User::find(1);
$user->name = 'Jack';
$user->save();
```



##### ✅ 删除数据

```php
$user = User::find(1);
$user->delete();
```



### 2.2.3 模型关联（关系）



#### 1️⃣ 一对多（hasMany）

👉 一个用户有多个文章

```php
// User.php
public function posts()
{
    return $this->hasMany(Post::class);
}
```



#### 2️⃣ 反向 belongsTo

```php
// Post.php
public function user()
{
    return $this->belongsTo(User::class);
}
```



 使用：

```php
$user = User::find(1);

// 获取该用户所有文章
$posts = $user->posts;
```



#### 3️⃣ 多对多（belongsToMany）

```php
// User.php
public function roles()
{
    return $this->belongsToMany(Role::class);
}
```



## 2.3 Blade 模板引擎

👉 Blade = Laravel 的前端模板引擎



### 2.3.1 基础语法

#### 1️⃣ 输出变量

```blade
{{ $name }}
```

👉 自动防 XSS（安全）



#### 2️⃣ 条件判断

```blade
@if($age > 18)
    成年人
@else
    未成年
@endif
```



#### 3️⃣ 循环

```blade
@foreach($users as $user)
    <p>{{ $user->name }}</p>
@endforeach
```



### 2.3.2 模板继承



#### 1️⃣ 父模板

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



#### 2️⃣ 子模板

```html
@extends('layouts.app')

@section('title', '首页')

@section('content')
    <h2>欢迎来到首页</h2>
@endsection
```



### 2.3.3 组件（Component）

👉 更现代的写法（推荐）



#### 1️⃣ 创建组件

```bash
php artisan make:component Alert
```



#### 2️⃣ 使用组件

```html
<x-alert type="success" message="操作成功！" />
```



#### 3️⃣ 组件模板

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

在 Laravel 中，**永远不要相信用户输入**，验证是后端必须做的事情（前端验证只是辅助）。


### 3.1.1 验证方式

#### ✅ 方式1：控制器内验证（简单项目用）

```php
use Illuminate\Http\Request;

public function store(Request $request)
{
    $validated = $request->validate([
        'name' => 'required|min:3|max:50',
        'email' => 'required|email',
    ]);

    // 验证通过后才会执行这里
    return "验证成功";
}
```

👉 特点：

* 简单直接
* 不适合复杂项目（代码会变乱）



#### ✅ 方式2：Validator 类（更灵活）

```php
use Illuminate\Support\Facades\Validator;

public function store(Request $request)
{
    $validator = Validator::make($request->all(), [
        'name' => 'required|min:3',
    ]);

    if ($validator->fails()) {
        return redirect()->back()
            ->withErrors($validator) // 错误信息
            ->withInput(); // 保留输入
    }

    return "验证成功";
}
```



#### ✅ 方式3：Form Request（推荐 ⭐）

👉 企业级项目标准写法

```bash
php artisan make:request StoreUserRequest
```

生成文件：

```
app/Http/Requests/StoreUserRequest.php
```



### 3.1.2 Form Request 结构

```php
class StoreUserRequest extends FormRequest
{
    // 是否允许访问（权限控制）
    public function authorize(): bool
    {
        return true; // false 会直接 403
    }

    // 验证规则
    public function rules(): array
    {
        return [
            'name' => 'required|min:3',
            'email' => 'required|email|unique:users,email',
            'password' => 'required|confirmed|min:6',
        ];
    }

    // 自定义错误信息
    public function messages(): array
    {
        return [
            'name.required' => '用户名不能为空',
            'email.email' => '邮箱格式不正确',
        ];
    }
}
```



#### 控制器中使用：

```php
public function store(StoreUserRequest $request)
{
    // 自动验证通过才会进入这里
    $data = $request->validated();

    return $data;
}
```



### 3.1.3 常用验证规则

#### 📌 必填

```php
'name' => 'required'
'age' => 'nullable'
```



#### 📌 格式

```php
'email' => 'email'
'website' => 'url'
'birthday' => 'date'
```



#### 📌 长度

```php
'username' => 'min:3|max:20'
```



#### 📌 唯一性

```php
'email' => 'unique:users,email'
```

更新时：

```php
'email' => 'unique:users,email,' . $user->id
```



#### 📌 字段对比

```php
'password' => 'confirmed' // 需要 password_confirmation 字段
'new_password' => 'different:old_password'
```



### 3.1.4 自定义验证规则

#### 方法1：闭包

```php
'name' => [
    'required',
    function ($attribute, $value, $fail) {
        if ($value === 'admin') {
            $fail('不能使用 admin');
        }
    }
]
```



#### 方法2：Rule 类（推荐）

```bash
php artisan make:rule CheckName
```

```php
class CheckName implements Rule
{
    public function passes($attribute, $value)
    {
        return $value !== 'admin';
    }

    public function message()
    {
        return '不能使用 admin';
    }
}
```

使用：

```php
'name' => ['required', new CheckName]
```



### 3.1.5 验证处理

#### ❌ 验证失败行为

* Web：自动重定向
* API：自动返回 JSON

```json
{
  "errors": {
    "email": ["邮箱格式错误"]
  }
}
```



#### Blade 显示错误

```blade
@if ($errors->any())
    <ul>
        @foreach ($errors->all() as $error)
            <li>{{ $error }}</li>
        @endforeach
    </ul>
@endif
```



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



## 3.4 用户认证（Authentication）



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



## 3.5 权限控制（Authorization）



### 3.5.1 权限方式

| 方式     | 使用场景     |
|  | -- |
| Gate   | 简单判断     |
| Policy | 复杂权限（推荐） |



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

| 功能  | 推荐           |
|  |  |
| 验证  | Form Request |
| 权限  | Policy       |
| 认证  | Breeze       |
| API | Sanctum      |



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


# 4. FilamentPHP（现代后台加速器）

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
```

访问：

```text
http://127.0.0.1:8000/admin
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



#### 表单示例（带详细注释）

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



#### 常用组件大全（新手必会）

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



#### 🔍 搜索 & 筛选示例

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



#### 添加按钮

```php
use Filament\Tables\Actions\Action;

Action::make('download')
    ->label('下载')
    ->action(function ($record) {
        // $record = 当前行数据
        dd($record);
    })
```



#### 带确认弹窗

```php
Action::make('delete')
    ->requiresConfirmation()
    ->action(fn ($record) => $record->delete())
```



#### 通知提示

```php
use Filament\Notifications\Notification;

Notification::make()
    ->title('操作成功')
    ->success()
    ->send();
```



## 4.4 实战：完整 CRUD 流程



### 一步到位流程：

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


## 5.1 认证机制选择 (Authentication Strategies)

### 5.1.1 为什么移动端不用 Session？

传统 Web（比如 PHP + Blade）：

```text
浏览器 → 登录 → 服务器返回 Cookie → 浏览器自动带 Cookie
```

👉 但 Flutter：

* 没有浏览器环境
* 不自动管理 Cookie
* 跨平台复杂

👉 所以用 **Token（令牌）**


### 5.1.2 Sanctum vs Passport vs JWT

| 方案        | 适合场景      | 难度   |
| --------- | --------- | ---- |
| Sanctum ✅ | App / SPA | ⭐    |
| Passport  | OAuth2    | ⭐⭐⭐⭐ |
| JWT       | 自定义认证     | ⭐⭐⭐  |

👉 **结论：你现在就用 Sanctum**


### 5.1.3 安装 Sanctum

```bash
composer require laravel/sanctum
```

发布配置：

```bash
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
```

迁移数据库：

```bash
php artisan migrate
```


### 5.1.4 配置 User 模型

```php
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens; // ✅ 必须加这个

    // 允许批量赋值字段
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    // 隐藏字段（不会返回给前端）
    protected $hidden = [
        'password',
        'remember_token',
    ];
}
```


## 5.2 Laravel 后端核心


### 5.2.1 创建 AuthController

```bash
php artisan make:controller Api/AuthController
```


### 5.2.2 注册接口

```php
public function register(Request $request)
{
    // ✅ 数据验证
    $request->validate([
        'name' => 'required|string|max:255',
        'email' => 'required|email|unique:users',
        'password' => 'required|min:6'
    ]);

    // ✅ 创建用户
    $user = User::create([
        'name' => $request->name,
        'email' => $request->email,
        // 🔐 密码必须加密
        'password' => bcrypt($request->password),
    ]);

    // ✅ 创建 Token
    $token = $user->createToken('app-token')->plainTextToken;

    return response()->json([
        'user' => $user,
        'token' => $token
    ]);
}
```


### 5.2.3 登录接口

```php
public function login(Request $request)
{
    // ✅ 验证输入
    $request->validate([
        'email' => 'required|email',
        'password' => 'required'
    ]);

    // 🔍 查找用户
    $user = User::where('email', $request->email)->first();

    // ❌ 用户不存在 或 密码错误
    if (!$user || !Hash::check($request->password, $user->password)) {
        return response()->json([
            'message' => 'Invalid credentials'
        ], 401);
    }

    // 🧹 删除旧 Token（可选）
    $user->tokens()->delete();

    // 🎫 创建新 Token
    $token = $user->createToken('app-token')->plainTextToken;

    return response()->json([
        'user' => $user,
        'token' => $token
    ]);
}
```


### 5.2.4 登出接口

```php
public function logout(Request $request)
{
    // ❌ 删除当前 Token
    $request->user()->currentAccessToken()->delete();

    return response()->json([
        'message' => 'Logged out'
    ]);
}
```


### 5.2.5 保护路由（重点）

```php
Route::middleware('auth:sanctum')->group(function () {

    Route::get('/user', function (Request $request) {
        return $request->user();
    });

    Route::post('/logout', [AuthController::class, 'logout']);

});
```

👉 没 Token 访问会返回：

```json
401 Unauthorized
```


### 5.2.6 Token 权限（Abilities）

```php
$token = $user->createToken('admin-token', ['create', 'delete']);
```

检查权限：

```php
if ($request->user()->tokenCan('delete')) {
    // 可以删除
}
```


## 5.3 Flutter 前端（核心重点🔥）


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
          String? token = await storage.getToken();

          if (token != null) {
            options.headers['Authorization'] = 'Bearer $token';
          }

          return handler.next(options);
        },

        // ❗ 响应拦截器
        onError: (error, handler) async {
          if (error.response?.statusCode == 401) {
            // 🔥 Token 失效
            await storage.deleteToken();

            // 👉 可以跳转登录页
          }

          return handler.next(error);
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


### 5.3.6 Riverpod 状态（推荐🔥）

```dart
final authProvider = StateProvider<bool>((ref) => false);
```

登录成功：

```dart
ref.read(authProvider.notifier).state = true;
```


## 5.4 权限系统（Authorization）


### 5.4.1 Gate 示例

```php
Gate::define('is-admin', function ($user) {
    return $user->role === 'admin';
});
```

使用：

```php
if (Gate::allows('is-admin')) {
    // 是管理员
}
```


### 5.4.2 Policy 示例

```bash
php artisan make:policy PostPolicy
```

```php
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
```

控制器中：

```php
$this->authorize('update', $post);
```


### 5.4.3 Flutter 控制 UI

```dart
if (user.role == 'admin') {
  return ElevatedButton(
    onPressed: () {},
    child: Text("删除"),
  );
}
```


## 5.5 安全最佳实践（非常重要🔥）

### 5.5.1 强制 HTTPS

```env
APP_URL=https://yourdomain.com
```


### 5.5.2 限流（防爆破）

```php
Route::post('/login', [AuthController::class, 'login'])
    ->middleware('throttle:5,1'); // 1分钟最多5次
```


### 5.5.3 Token 过期

```php
// config/sanctum.php
'expiration' => 60, // 分钟
```


### 5.5.4 不暴露错误

❌ 错误写法：

```php
return $e->getMessage();
```

✅ 正确：

```php
return response()->json([
    'message' => 'Server Error'
], 500);
```

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

```bash
composer create-project laravel/laravel my_project
cd my_project
php artisan serve
```

访问：

```
http://127.0.0.1:8000
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
        // 先查到这条数据
        $product = Product::findOrFail($id);

        // update()
        // 👉 更新数据库
        // 👉 传什么字段就更新什么
        $product->update($request->all());

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

#### 6.2.5.3 补充：批量删除
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


### 6.2.6 添加状态码
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


### 6.2.7 设置模型可填充字段

打开 `Product.php`

```php
protected $fillable = [
    'name',
    'price',
    'description'
];
```

⚠️ 不写这个会报错（Laravel 安全机制）


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