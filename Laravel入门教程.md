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

