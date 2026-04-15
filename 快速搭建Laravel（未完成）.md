# 0. 前言 

## 0.1 在开始之前
我们假设你已经安装了 Laravel 11（推荐最新稳定版）。如果还没安装，先执行：

```bash
composer create-project laravel/laravel your-project-name
cd your-project-name
```

下面我先给你一个**简要知识点提纲**（M / C / JR 结构），然后逐个部分详细讲解：**是否需要命令行**、**文件路径**、**代码怎么写**（带完整示例）。

## 0.2 树状目录结构

```
your-project-name/           ← 项目根目录
├── .env                     ← 数据库配置（最先修改）
├── app/
│   ├── Models/
│   │   ├── User.php         ← 用户模型（必须实现 JWTSubject）
│   │   └── Article.php      ← 示例：你的业务模型
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Controller.php      ← 所有控制器的基类
│   │   │   ├── AuthController.php  ← 登录、注册控制器
│   │   │   └── ArticleController.php ← 业务控制器（CRUD）
│   │   ├── Requests/
│   │   │   ├── StoreArticleRequest.php   ← 创建验证
│   │   │   └── UpdateArticleRequest.php  ← 更新验证
│   │   └── Resources/
│   │       ├── ArticleResource.php       ← 单条资源格式
│   │       └── ArticleCollection.php     ← 列表资源格式
│   ├── Policies/
│   │   └── ArticlePolicy.php       ← 授权策略（谁能修改/删除）
│   └── Providers/
│       └── AuthServiceProvider.php ← 注册 Policy
├── bootstrap/
│   └── app.php            ← 自定义 JWT 401 返回的 Json
├── config/
│   ├── auth.php           ← 配置 api guard 为 jwt
│   └── jwt.php            ← JWT 配置（过期时间、黑名单等）
├── database/
│   └── migrations/
│       ├── xxxx_xx_xx_create_users_table.php
│       └── xxxx_xx_xx_create_articles_table.php   ← 迁移文件
├── routes/
│   └── api.php             ← API 路由定义
├── composer.json
└── ...（其他文件夹如 public、storage 等一般不用动）
```

## 0.3 知识点简要提纲

**M（Model 相关）**  
- `.env`：数据库连接配置（最基础）。  
- `app\Models\Model.php`（或具体如 `User.php`）：Eloquent 模型，定义表结构、关系、fillable 等。  
- `database/migrations/xxxx_create_xxxx_table.php`：迁移文件，用来创建/修改数据库表。

**C（Controller 相关）**  
- `app\Http\Controllers\XxxxController.php`：业务逻辑。  
- `app\Http\Requests\XxxxRequest.php`：FormRequest 类，负责验证输入数据，是 Controller 的入口。  
- `app\Http\Resources\XxxxResource.php`：API Resource 类，控制返回 JSON 的格式（避免暴露敏感字段），是 Controller 的出口。  
- `app\Policies\XxxxPolicy.php`：授权策略，控制用户是否有权限操作资源。  
- `app\Providers\XxxxServiceProvider.php`：注册 Policies。

**JR（JWT & Router）**  
- `config\auth.php`：配置认证守卫（guard），把 `api` 守卫改成 JWT。  
- `config\jwt.php`：JWT 包的配置文件（token 过期时间、黑名单等）。  
- `routes\api.php`：定义 API 路由（推荐用 `apiResource` 或分组）。 
- `bootstrap\app.php`：Laravel 11 新结构，JWT 无效/过期时，自定义 JSON 返回（不再用旧的 `Handler.php`）。

下面开始**实际操作**，按顺序来，一步一步做。

# 1. M（Model 相关）

## 1.1 配置数据库连接

### 1.1.1 以 SQLite 为例（推荐本地开发时使用）

用编辑器打开根目录下的 `.env` 文件。

你会看到类似下面这样（Laravel 11 默认内容）：

```env
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:xxxxxxxxxxxxxxxxxxxxxxxxxxxx
APP_DEBUG=true
APP_URL=http://localhost

# ==================== 数据库配置 ====================
DB_CONNECTION=sqlite          ← 默认就是 sqlite
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=laravel
# DB_USERNAME=root
# DB_PASSWORD=
```

**说明**：
- `DB_CONNECTION=sqlite` → 已经设置好了。
- 后面几行（MySQL 配置）被 `#` 注释掉了，不用管它们。
- **DB_DATABASE** 可以不写，Laravel 会默认使用 `database/database.sqlite` 这个文件。

### 1.1.2 以 MySQL 为例（推荐实际生产时使用）

用编辑器打开根目录下的 `.env` 文件修改。

```env
APP_NAME=LaravelAPI
APP_ENV=local
APP_KEY=base64:xxxxxxxxxxxxxxx   # 运行 php artisan key:generate 生成
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name   # 先在 MySQL 中创建好这个数据库
DB_USERNAME=root                 # 你的数据库用户名
DB_PASSWORD=                     # 你的数据库密码（本地通常为空）
```

保存后，测试连接是否成功：

```bash
php artisan migrate:status   # 如果没报错，就连上了
```

## 1.2 创建模型和迁移
 `app\Models\` 和 `database\migrations\`

### 1.2.1 命令行工具

命令行（推荐一次性创建模型 + 迁移 + factory）：

```bash
# 示例：创建一个文章模型（Article）
php artisan make:model Article -m -f

# 如果想创建带软删除的：
# php artisan make:model Article -m -f --soft-deletes
```

执行后会生成两个文件：

- `app/Models/Article.php`
- `database/migrations/xxxx_xx_xx_create_articles_table.php`


### 1.2.2 修改迁移文件

当你执行 `php artisan make:model Article -m` 后，Laravel 会在 `database/migrations/` 文件夹下自动生成一个类似下面名称的迁移文件：

`xxxx_xx_xx_xxxxxx_create_articles_table.php`

#### 1.2.2.1 打开并完善迁移文件

**文件路径**：  
`database/migrations/xxxx_xx_xx_xxxxxx_create_articles_table.php`

**完整推荐写法 + 详细注释**：

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * 运行迁移：创建 articles 表
     * up() 方法会在执行 php artisan migrate 时被调用
     */
    public function up(): void
    {
        Schema::create('articles', function (Blueprint $table) {

            // ================ 基础字段 ================
            $table->id();                    // 主键，自增 ID (bigint unsigned)
            // $table->bigIncrements('id');  // 旧写法，等同于上面的 id()

            $table->string('title', 255);    // 标题，字符串类型，默认长度 255
            $table->text('content');         // 文章内容，长文本

            // ================ 关联字段 ================
            $table->foreignId('user_id')           // 等同于 $table->unsignedBigInteger('user_id')
                  ->constrained()                  // 自动关联 users 表（表名 = 字段去掉 _id）
                  ->onDelete('cascade');           // 用户被删除时，关联的文章也一起删除

            // ================ 时间字段 ================
            $table->timestamps();            // 自动创建并管理 created_at 和 updated_at 两个 timestamp 字段

            // ================ 可选常用字段 ================
            // $table->softDeletes();        // 软删除：添加 deleted_at 字段，删除时不会真正删掉数据
            // $table->boolean('is_published')->default(false);   // 布尔类型，默认值 false（是否发布）
            // $table->integer('views')->default(0);              // 浏览量，默认 0
            // $table->decimal('price', 10, 2)->nullable();       // 价格，保留两位小数，可为空
            // $table->json('tags')->nullable();                  // JSON 字段，适合存数组或对象
            // $table->date('published_at')->nullable();          // 发布日期，可为空
            // $table->timestamp('published_at')->nullable();     // 带时区的精确时间戳

            // ================ 索引（提升查询速度） ================
            // $table->index('user_id');                    // 单字段索引
            // $table->index(['title', 'user_id']);         // 联合索引
            // $table->unique('title');                     // 唯一索引（标题不能重复）

        });
    }

    /**
     * 回滚（撤销）迁移：删除 articles 表
     * down() 方法会在执行 php artisan migrate:rollback 时被调用
     */
    public function down(): void
    {
        Schema::dropIfExists('articles');
    }
};
```


### 1.2.2.2 迁移文件中常用写法详细说明

下面我把迁移文件中**最常用**的字段类型和方法，全部列出来并注释说明：

- `$table->id();`  
  创建主键 `id` 字段（推荐写法）

- `$table->string('title', 255);`  
  字符串字段，可指定最大长度

- `$table->text('content');`  
  长文本（适合文章内容）

- `$table->foreignId('user_id')->constrained()->onDelete('cascade');`  
  外键关联，最简洁写法

- `$table->timestamps();`  
  自动添加并管理 `created_at` 和 `updated_at`

- `$table->softDeletes();`  
  软删除，添加 `deleted_at` 字段

- `$table->boolean('is_active')->default(false);`  
  布尔字段 + 默认值

- `$table->integer('views')->default(0);`  
  整数字段 + 默认值

- `$table->decimal('price', 10, 2);`  
  精确小数（总位数10，小数位2）

- `$table->json('meta');`  
  JSON 字段（Laravel 推荐存数组/对象）

- `$table->nullable();`  
  允许该字段为空（可链式调用）

- `$table->default('默认值');`  
  设置默认值

- `$table->index('字段名');`、`unique()`、`fullText()` 等  
  添加索引，提升查询性能


#### 1.2.2.3 迁移文件常用方法总结表

| 方法写法                                      | 含义说明                           | 常用场景                  | 是否推荐 |
|---------------------------------------------|------------------------------------|---------------------------|----------|
| `$table->id()`                              | 主键自增ID                         | 所有表                    | 强烈推荐 |
| `$table->string('title')`                   | 字符串（默认255）                  | 标题、名称                | 常用     |
| `$table->text('content')`                   | 长文本                             | 文章、描述                | 常用     |
| `$table->foreignId('user_id')`              | 外键（简洁写法）                   | 关联其他表                | 强烈推荐 |
| `$table->timestamps()`                      | 创建时间 + 更新时间                | 几乎所有表                | 强烈推荐 |
| `$table->softDeletes()`                     | 软删除字段                         | 需要恢复删除的数据        | 推荐     |
| `$table->boolean('is_published')`           | 布尔值                    | 开关、状态                | 常用     |
| `$table->integer('views')`                  | 整数                               | 数量、次数                | 常用     |
| `$table->decimal('price', 10, 2)`           | 精确小数                           | 金额、价格                | 常用     |
| `$table->json('tags')`                      | JSON 格式数据                      | 配置、标签、扩展字段      | 推荐     |
| `$table->date()` / `$table->timestamp()`    | 日期 / 时间戳                      | 日期相关                  | 常用     |
| `->nullable()`                              | 允许为空                           | 非必填字段                | 常用     |
| `->default(值)`                             | 设置默认值                         | 有默认值的字段            | 常用     |
| `->index()`                 | 添加索引                | 经常搜索的字段  | 推荐     |
| `->unique()`                  | 唯一约束                | 不能重复的字段  | 推荐     |
| `->onDelete('cascade')`                     | 级联删除                           | 外键删除时                | 推荐     |


**小贴士**：

1. 迁移文件一旦 `migrate` 成功后，**不要随意修改**已运行过的迁移文件（除非使用 `migrate:refresh` 重置）。
2. 如果想修改表结构，应该新建一个迁移文件：`php artisan make:migration update_articles_table --table=articles`
3. 养成好习惯：**每次修改迁移文件前，先写清楚注释**，以后自己或别人看代码会非常清晰。


### 1.2.3 修改模型文件（Model）

模型文件是 Laravel Eloquent ORM 的核心，用于操作数据库表。

当你执行 `php artisan make:model Article -m` 后，会自动生成：

**文件路径**：  
`app/Models/Article.php`

#### 1.2.3.1 完整推荐写法 + 详细注释

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
// use Illuminate\Database\Eloquent\SoftDeletes;   // 如果需要软删除，取消注释

class Article extends Model
{
    use HasFactory;
    // use SoftDeletes;     // 软删除功能：删除数据时不会真正删除，而是标记 deleted_at 时间

    /**
     * 可以被批量赋值的字段（Mass Assignment）
     * 非常重要！不写的话，create() / fill() / update() 会报错
     */
    protected $fillable = [
        'title', 
        'content', 
        'user_id',
        // 'is_published',
        // 'views'
    ];

    /**
     * 隐藏不应该出现在返回前端的 JSON 中的字段（API 返回时自动隐藏）
     * 比如密码、token 等敏感信息
     */
    protected $hidden = [
        // 'password', 
        // 'remember_token'
    ];

    /**
     * 应该被转为日期格式的字段
     * Laravel 会自动把这些字段转为 Carbon 对象
     */
    protected $dates = [
        // 'published_at'
    ];

    /**
     * 默认每次查询都自动加载的关联关系（避免 N+1 问题）
     */
    protected $with = [
        // 'user'   // 如果你希望每次查询 Article 时都自动带上 user 信息
    ];

    // ====================== 关联关系 ======================

    /**
     * 一篇文章属于一个用户（多对一）
     */
    public function user()
    {
        return $this->belongsTo(User::class);
        // 等同于 return $this->belongsTo(User::class, 'user_id', 'id');
    }

    /**
     * 示例：如果有评论功能，可以这样写一对多
     */
    // public function comments()
    // {
    //     return $this->hasMany(Comment::class);
    // }

    /**
     * 示例：作用域（Scope）—— 常用查询简化
     * 使用方法：Article::published()->get();
     */
    public function scopePublished($query)
    {
        return $query->where('is_published', true);
    }

    /**
     * 示例：访问器（Accessor）—— 自定义属性
     * 使用方法：$article->full_title
     */
    // public function getFullTitleAttribute()
    // {
    //     return $this->title . ' - ' . $this->created_at->format('Y-m-d');
    // }

    /**
     * 示例：修改器（Mutator）—— 保存前自动处理数据
     */
    // public function setTitleAttribute($value)
    // {
    //     $this->attributes['title'] = strtoupper($value);   // 强制转大写
    // }
}
```

运行迁移创建表：

```bash
php artisan migrate
```

#### 1.2.3.2 模型文件常用写法详细说明

- `use HasFactory;`  
  允许使用工厂模式快速生成测试数据（`Article::factory()->create()`）

- `protected $fillable = [];`  
  **最重要**：白名单，允许哪些字段被批量赋值（`create()`、`update()`、`fill()`）

- `protected $guarded = [];`  
  黑名单写法（与 $fillable 二选一，通常推荐用 $fillable）

- `protected $hidden = [];`  
  API 返回 JSON 时自动隐藏的字段（安全考虑）

- `protected $with = [];`  
  全局自动加载关联关系，减少 N+1 查询问题

- `belongsTo()`、`hasMany()`、`hasOne()`、`belongsToMany()`  
  定义模型之间的关联关系

- `scopeXXX()`  
  自定义查询作用域，让查询更优雅

- `getXXXAttribute()`  
  访问器：自定义读取时的属性

- `setXXXAttribute()`  
  修改器：保存数据前自动处理


#### 1.2.3.3 Laravel 模型常用属性与方法总结表

| 写法 / 属性                        | 含义说明                                   | 常用场景                        | 推荐程度 |
|-----------------------------------|--------------------------------------------|---------------------------------|----------|
| `use HasFactory`                  | 启用模型工厂                               | 测试数据生成                    | 强烈推荐 |
| `protected $fillable = []`        | 允许批量赋值的字段（白名单）               | create()、update()              | 必须掌握 |
| `protected $guarded = []`         | 禁止批量赋值的字段（黑名单）               | 极少使用                        | 了解即可 |
| `protected $hidden = []`          | JSON 输出时隐藏的字段                      | 密码、token、敏感信息           | 常用     |
| `protected $with = []`            | 每次查询自动加载的关联                     | 减少 N+1 查询问题               | 推荐     |
| `protected $dates = []`           | 自动转为 Carbon 日期对象                   | 日期字段                        | 推荐     |
| `$table->softDeletes()` + `use SoftDeletes` | 软删除功能                         | 需要恢复删除的数据              | 推荐     |
| `belongsTo(User::class)`          | 多对一关联                                 | 文章属于用户                    | 常用     |
| `hasMany(Comment::class)`         | 一对多关联                                 | 一篇文章有多条评论              | 常用     |
| `scopePublished($query)`          | 查询作用域                                 | 常用过滤条件简化                | 推荐     |
| `getFullTitleAttribute()`         | 访问器（自定义属性）                       | 格式化输出                      | 推荐     |
| `setTitleAttribute($value)`       | 修改器（数据保存前处理）                   | 数据清洗、格式转换              | 推荐     |
| `public function user()`          | 定义关联关系方法                           | 模型间关系                      | 必须掌握 |


**小贴士**：

1. **$fillable 是最容易踩坑的地方**！如果你使用 `Article::create($request->all())`，却没有在 `$fillable` 中声明字段，就会报 `MassAssignmentException` 错误。
2. 关联关系方法名建议使用**单数或复数正确形式**（belongsTo 用单数，hasMany 用复数）。
3. 模型文件写好后，**先运行一次迁移**，再继续写控制器。
4. **User 模型**（Laravel 自带）通常要修改 `app/Models/User.php`，添加 JWT 支持（后面 JR 部分会说）。

# 2. C（Controller 相关）

## 2.1 Eloquent ORM

`app\Http\Controllers\Controller.php` 通常不用改，它已经继承了 `Illuminate\Routing\Controller`。

接下来，我们来聊聊控制器中的业务逻辑怎样写。

### 2.1.1 创建 ArticleController（控制器）

控制器是连接 Flutter 前端与数据库的核心，负责接收请求、处理业务逻辑、返回 JSON 数据。

**命令行创建控制器**（推荐使用 `--api` 和 `--model` 参数）：

```bash
php artisan make:controller ArticleController --api --model=Article
```

**文件路径**：  
`app/Http/Controllers/ArticleController.php`

### 2.1.2 完整推荐写法 + 详细注释

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use App\Models\Article;
use App\Http\Requests\StoreArticleRequest;
use App\Http\Requests\UpdateArticleRequest;

class ArticleController extends Controller
{
    // ========================
    // 1️⃣ 查询所有文章（READ - 列表）
    // ========================
    public function index()
    {
        // Article::all()           👉 查询 articles 表里的“所有数据”
        // with('user')             👉 同时加载关联的用户信息，避免 N+1 查询问题
        // latest()                 👉 按创建时间倒序排列（最新的在前面）
        // paginate(15)             👉 分页，每页显示15条（非常推荐 API 使用）
        
        $articles = Article::with('user')->latest()->paginate(15);

        // response()->json()       👉 返回 JSON 格式给前端（Flutter）
        return response()->json($articles);
    }

    // ========================
    // 2️⃣ 创建文章（CREATE）
    // ========================
    public function store(Request $request)
    {
        // $request = 前端（Flutter）传过来的 JSON 数据
        
        // ✅ Laravel 内置验证（简单写法）
        $request->validate([
            'title'   => 'required|string|max:255',
            'content' => 'required|string|min:10',
        ]);

        // ✅ Eloquent ORM
        // create() = 插入新数据到数据库
        // $request->all() = 获取前端传来的所有字段
        // $fillable 会自动过滤不安全的字段
        $article = Article::create($request->all());

        // 返回创建成功的数据 + HTTP 状态码 201（Created）
        return response()->json([
            'message' => '文章创建成功',
            'data' => $article
        ], 201);
    }

    // ========================
    // 3️⃣ 查询单个文章（READ - 单个）
    // ========================
    public function show($id)
    {
        // findOrFail($id)
        // 👉 根据 id 查找文章
        // 👉 如果找不到，自动返回 404 错误（超方便！不需要自己判断）
        return Article::findOrFail($id);
    }

    // ========================
    // 4️⃣ 更新文章（UPDATE）
    // ========================
    public function update(Request $request, $id)
    {
        // 先找到要更新的文章
        $article = Article::findOrFail($id);

        /**
         * 💡 情况 A：前端只修改标题
         * 前端发送 {"title": "新标题"} → 只有 title 被更新
         *
         * 💡 情况 B：前端修改标题和内容
         * 前端发送 {"title": "新标题", "content": "新内容"} → 两个字段都被更新
         *
         * 💡 情况 C：前端传了多余字段（如 {"title": "xx", "hack": "123"}）
         * 如果 Model 的 $fillable 里没有 'hack'，update() 会自动过滤掉，保护数据安全！
         */

        $article->update($request->all());

        return response()->json([
            'message' => '文章更新成功',
            'data' => $article
        ]);
    }

    // ========================
    // 5️⃣ 删除单个文章（DELETE）
    // ========================
    public function destroy($id)
    {
        // destroy($id)
        // 👉 根据 id 删除数据（支持软删除）
        Article::destroy($id);

        return response()->json([
            'message' => '文章删除成功'
        ]);
    }

    // ========================
    // 6️⃣ 批量删除文章（Batch Delete）← 新增功能
    // ========================
    /**
     * Flutter 前端调用方式：
     * POST   /api/articles/batch-delete
     * Body:  { "ids": [1, 3, 5, 7] }
     */
    public function batchDelete(Request $request)
    {
        // 验证前端传来的 ids
        $request->validate([
            'ids'   => 'required|array|min:1',
            'ids.*' => 'integer|exists:articles,id',   // 确保每个ID都真实存在
        ]);

        // 执行批量删除
        $deletedCount = Article::whereIn('id', $request->ids)->delete();

        return response()->json([
            'success' => true,
            'message' => "成功批量删除 {$deletedCount} 篇文章",
            'deleted_count' => $deletedCount
        ]);
    }
}
```


### 2.1.3 ArticleController 方法总结表

以下表格不仅列出了每个方法，还**特别标注了对应的数据库操作方法（Eloquent ORM）**，方便小白清晰了解控制器方法与数据库操作之间的对应关系。

| 方法名称          | HTTP 请求方式 | 功能说明                     | 数据库操作方法                          | 验证方式                    |
|-------------------|---------------|------------------------------|-----------------------------------------|-----------------------------|
| `index()`         | GET           | 获取文章列表（支持分页）     | `Article::with('user')->latest()->paginate(15)` | 无                          |
| `store()`         | POST          | 创建新文章                   | `Article::create($request->all())`      | `$request->validate()`      |
| `show($id)`       | GET           | 获取单个文章                 | `Article::findOrFail($id)`              | 无                          |
| `update($id)`        | PUT / PATCH   | 更新单个文章                     | `$article->update($request->all())`     | `$request->validate()`      |
| `destroy($id)`    | DELETE        | 删除单个文章                 | `Article::destroy($id)`                 | 无                          |
| `batchDelete()`   | POST          | **批量删除多篇文章**         | `Article::whereIn('id', $ids)->delete()` | `$request->validate()`      |


**小贴士**：

1. 本示例使用的是**最基础的写法**（直接在控制器中验证），方便你快速理解 Eloquent ORM。
2. 在实际项目中，**强烈建议**把验证逻辑移到 `StoreArticleRequest` 和 `UpdateArticleRequest` 中（接下来会学）。
3. `batchDelete()` 方法非常实用，Flutter 端只需要传一个 `ids` 数组即可实现批量删除。
4. `findOrFail()` 和 `destroy()` 如果找不到数据会自动抛出 404，省去了大量 `if` 判断。
5. `whereIn('id', $ids)` 是实现批量操作最常用、最高效的方式。



## 2.2 创建 Form Request（表单请求验证）

`Form Request` 是 Laravel 中专门用来**验证用户提交数据**的类。

它的优点：代码干净、验证规则集中管理、错误信息友好、可以复用。

**文件位置**：  
`app/Http/Requests/`

### 2.2.1 命令行创建 Form Request

```bash
# 创建用于“新增文章”的验证类
php artisan make:request StoreArticleRequest

# 创建用于“修改文章”的验证类
php artisan make:request UpdateArticleRequest
```

### 2.2.2 完整推荐写法 + 详细注释

**文件路径**：  
`app/Http/Requests/StoreArticleRequest.php`

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreArticleRequest extends FormRequest
{
    /** 必填方法：authorize()
     * 判断当前用户是否有权限提交这个请求
     * 返回 true 则允许验证，继续执行控制器
     * 返回 false 则直接抛出 403 禁止访问
     */
    public function authorize(): bool
    {
        // 场景示例：如果是更新文章，可以判断当前用户是不是文章作者
        // return $this->user()->id === $this->article->author_id;
        
        return true; // 小白阶段建议先设为 true，否则会直接报 403 错误
    }

    /** 核心方法：rules()
     * 作用：定义具体的验证规则（数据长什么样才合格）。
     * 返回值：返回一个关联数组。
     * 这是 Form Request 中最核心的部分
     */
    public function rules(): array
    {
        return [
            'title'      => 'required|string|max:255|unique:articles,title',   // 标题必填、字符串、最长255、不能重复
            'content'    => 'required|string|min:10',                         // 内容必填、最少10个字符
            'user_id'    => 'required|integer|exists:users,id',               // 必须是已存在的用户ID
            // 'is_published' => 'boolean',      // 必须是布尔值
            // 'tags'       => 'array',        // 必须是数组
            // 'tags.*'     => 'string|max:50',   // 数组中的每一项都要验证
            // 'published_at' => 'nullable|date|after:today',                 // 可为空，必须是日期，且在今天之后
            // 'image'      => 'nullable|image|mimes:jpeg,png,jpg|max:2048',  // 图片验证（最大2MB）
        ];
    }

    /** 
     * 自定义验证错误消息（让错误提示更友好，可选）
     * 如果不写，会使用 Laravel 默认的英文提示
     */
    public function messages(): array
    {
        return [
            'title.required'    => '文章标题不能为空',
            'title.max'         => '标题长度不能超过255个字符',
            'title.unique'      => '该标题已经存在，请换一个标题',
            'content.required'  => '文章内容不能为空',
            'content.min'       => '文章内容至少需要10个字符',
            'user_id.exists'    => '所选用户不存在',
            // 'image.max'      => '图片大小不能超过2MB',
        ];
    }

    /**
     * 自定义字段名称（让错误提示更友好，可选）
     * 例如：把 'user_id' 显示为 '作者'
     */
    public function attributes(): array
    {
        return [
            'title'   => '文章标题',
            'content' => '文章内容',
            'user_id' => '作者',
        ];
    }

    /**
     * 在验证通过后还可以做额外处理（可选）
     */
    // protected function prepareForValidation()
    // {
    //     $this->merge([
    //         'user_id' => auth()->id(),   // 自动把当前登录用户ID填入
    //     ]);
    // }
}
```


### 2.2.3 Form Request 文件常用写法详细说明

- `authorize()`：**必需** —— 权限检查（登录、角色、Policy 等）
- `rules()`：**核心** —— 定义所有验证规则
- `messages()`：自定义中文错误提示
- `attributes()`：把字段名翻译成中文名称，让错误信息更自然
- `prepareForValidation()`：在验证前对数据进行预处理（如自动填充用户ID）


### 2.2.4 Laravel Form Request 常用验证方法总结表

**验证方法：**

| 方法                    | 含义说明                                      | 常用场景                        | 推荐程度 |
|-------------------------------|-----------------------------------------------|---------------------------------|----------|
| `authorize()`                 | 判断是否有权限提交请求                        | 登录验证、权限控制              | 必须掌握 |
| `rules()`                     | 定义验证规则（最重要）                        | 所有表单验证                    | 必须掌握 |
| `messages()`                  | 自定义错误提示消息                            | 中文友好提示                    | 强烈推荐 |
| `attributes()`                | 自定义字段名称（中文显示）                    | 让错误提示更自然                | 可选   |
| `prepareForValidation()`      | 验证前预处理数据                              | 自动填充用户ID、格式化数据      | 可选     |


### 2.2.5 Laravel Form Request 常用验证规则总结表

**验证规则：**

Laravel 提供了上百种验证规则，作为小白，你只需要先掌握下面这 20% 最常用的，就能应付 80% 的开发场景。

#### 2.5.5.1 基础存在性检查
| 规则 | 说明 | 示例 |
| :--- | :--- | :--- |
| `required` | **必填**。字段不能为空且必须存在。 | `'name' => 'required'` |
| `nullable` | **可为空**。如果不传或传 null，则通过验证。 | `'bio' => 'nullable'` |
| `sometimes` | **存在时验证**。只有当字段出现在请求中时才验证。 | `'password' => 'sometimes\|min:8'` |

#### 2.5.5.2 数据类型检查
| 规则 | 说明 | 示例 |
| :--- | :--- | :--- |
| `string` | 必须是字符串。 | `'title' => 'string'` |
| `numeric` | 必须是**数字**（支持浮点数和整数）。 | `'price' => 'numeric'` |
| `integer` | 必须是**整数**。 | `'age' => 'integer'` |
| `boolean` | 必须是布尔值（true/false）。 | `'is_public' => 'boolean'` |
| `array` | 必须是数组。 | `'tags' => 'array'` |

#### 2.5.5.3 长度与大小限制
| 规则 | 说明 | 示例 |
| :--- | :--- | :--- |
| `max:value` | 最大值。字符串算字符数，数字算大小，文件算大小 (以KB计)。 | `'title' => 'max:255'` |
| `min:value` | 最小值。字符串算字符数，数字算大小，文件算大小 (以KB计)。 | `'password' => 'min:8'` |
| `between:min,max` | 范围限制。（同上） | `'score' => 'between:1,100'` |

#### 2.5.5.4 格式校验
| 规则 | 说明 | 示例 |
| :--- | :--- | :--- |
| `email` | 必须符合邮箱格式。 | `'email' => 'email'` |
| `url` | 必须是合法的 URL 地址。 | `'website' => 'url'` |
| `date` | 必须是合法的日期字符串。 | `'birthday' => 'date'` |
| `ip` | 必须是合法的 IP 地址。 | `'last_ip' => 'ip'` |
| `regex:pattern` | **正则匹配**（终极方案）。 | `'phone' => 'regex:/^1[3-9]\d{9}$/'` |

#### 2.5.5.5 数据库相关（最常用）
| 规则 | 说明 | 示例 |
| :--- | :--- | :--- |
| `unique:table,column` | **唯一性**。在指定表的某列中不能重复。 | `'email' => 'unique:users,email'` |
| `exists:table,column` | **存在性**。提交的值必须在表里已存在（常用于外键）。 | `'category_id' => 'exists:categories,id'` |

👉 注意：`column` 什么时候可以省略?

当你写 `'title' => 'unique:articles'` 时，Laravel 会默认认为：

**你要校验的数据库字段名，和你请求中的键名（Key）是一模一样的。**

* **你的请求键名**：`title`
* **Laravel 默认查找的列名**：`articles` 表中的 `title` 列。

所以，如果两者一致，你完全可以省掉列名。

#### 2.5.5.6 其他逻辑
| 规则 | 说明 | 示例 |
| :--- | :--- | :--- |
| `confirmed` | **二次确认**。会自动检查 `字段名_confirmation` 字段。 | `'password' => 'confirmed'` |
| `in:a,b,c` | **枚举限制**。值必须在给定的选项中。 | `'type' => 'in:news,video,image'` |

#### 2.5.5.7 规则组合小技巧

在 `rules()` 方法中，你可以使用 `|`（管道符）或者 **数组** 来组合多个规则。

**写法 A（字符串形式）：**
```php
'email' => 'required|email|unique:users|max:50',
```

**写法 B（数组形式 - 推荐）：**
当规则中带有正则表达式或变量时，数组写法更安全、更不容易出错。
```php
'phone' => [
    'required',
    'regex:/^1[3-9]\d{9}$/',
    'unique:users,phone'
],
```

#### 2.5.5.8 课后小思考

> **Q：如果我想验证用户上传的头像图片，要求必须是图片格式，且大小不能超过 2MB，该怎么组合规则？**
>
> **A：** 你可以查阅文档使用 `image` 和 `max` 规则：
> `'avatar' => 'required|image|max:2048',`（注意：`max` 在处理文件时，单位是 **KB**）。


### 2.2.6 在控制器中使用：
```php
// 由原来的 Request 类改成了 FormRequest 类
   public function store(StoreArticleRequest $request)  
{
    // 验证已经自动通过，这里可以直接使用 $request->validated()
    // 逻辑非常清爽！
    // 验证逻辑在进入这个方法之前就已经由 Laravel 自动完成了
    $data = $request->validated(); 

/*
   ❌ rules() 里面没有的字段会被丢弃

例如：
    public function rules(): array
    {
        return [
            'title' => 'required|string|max:255|unique:articles',
            'content'    => 'required|string|min:10', 
        ];
    }
而 request 的内容是：
    {
    "title": "我的第一篇文章",
    "content": "简而言之，这是一篇速通 Laravel 的教程，有助于小白在最短时间内理解框架结构",
    "is_admin": true,
    "money": 9999
    }

那么：
    $data = $request->validated(); 
    此时 $data 的结果仅为：{"title": "...", "content": "..."}
    ❌ {"is_admin": true, "money": 9999} 会被彻底丢弃，不会进入数据库！
*/
    
    Article::create($data);

    return response()->json(['message' => '发布成功']);
}
```

### 2.2.7 小贴士：
1. **StoreXXXRequest** 用于新增，**UpdateXXXRequest** 用于修改（更新时通常把 `unique` 规则去掉或加上 `ignore`）。
2. 错误信息会自动以 JSON 格式返回给前端，非常适合 API 开发。
3. 建议每个需要验证的接口都使用独立的 Form Request，不要把验证规则直接写在控制器里。




## 2.3 创建 API Resource（数据转换层）

当我们从数据库查询出数据后，通常不会直接把模型数据返回给 Flutter 前端。  
**API Resource**（资源类）的作用就是对数据进行“加工、过滤、格式化”，让返回给前端的 JSON 更加干净、专业和安全。

### 2.3.1 为什么要使用 API Resource？

- **隐藏敏感字段**：不返回用户密码、邮箱、手机号等隐私信息。
- **统一返回格式**：所有接口的时间格式、字段名称保持一致，方便 Flutter 解析。
- **字段重命名**：数据库字段叫 `created_at`，前端可能希望看到 `create_time`。
- **优雅处理关联数据**：避免出现 N+1 查询问题，同时控制关联数据是否返回。
- **代码更清晰**：控制器中不需要写一堆 `$article->only([...])` 或手动数组转换。

### 2.3.2 创建 ArticleResource

**命令行生成 Resource：**

```bash
# 创建单个资源（推荐）
php artisan make:resource ArticleResource

# 创建集合资源（列表专用，可选）
php artisan make:resource ArticleCollection
```

**文件路径**：  
`app/Http/Resources/ArticleResource.php`

### 2.3.3 完整推荐写法 + 详细注释

```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class ArticleResource extends JsonResource
{
    /**
     * 将 Article 模型转换为数组 / JSON
     * $this 代表当前正在处理的 Article 模型实例
     */
    public function toArray(Request $request): array
    {
        return [
            // 基础字段
            'id'         => $this->id,
            'title'      => $this->title,
            'content'    => $this->content,

            // 关联字段 - 只在关联被加载时才返回（推荐写法，避免 N+1 查询）
            'user' => $this->whenLoaded('user', function () {
                // user 存在时，只返回 username（不返回 id）
                return [
                    'username' => $this->user->name,
                ];
            }, [
                // user 不存在时（例如用户已被删除）
                'username' => '用户不存在'
            ]),

            // 时间格式化（统一返回格式）
            'created_at' => $this->created_at ? $this->created_at->toDateTimeString() : null,
            'updated_at' => $this->updated_at ? $this->updated_at->toDateTimeString() : null,

            // 示例：自定义字段（可选）
            // 'status'     => $this->is_published ? '已发布' : '草稿',

            // 条件返回（只有当某个条件成立时才返回该字段）
            // 'published_at' => $this->when($this->is_published, fn() => $this->published_at),
        ];
    }
}
```

### 2.3.4 在 Controller 中使用 API Resource

修改 `ArticleController.php`，推荐使用 Resource 包裹数据返回：

```php
// ========================
// 查询文章列表
// ========================
public function index()
{
    $articles = Article::with('user')->latest()->paginate(15);
    
    // 使用 collection() 处理列表（支持分页）
    return ArticleResource::collection($articles);
}

// ========================
// 查询单个文章
// ========================
public function show(Article $article)
{
    // 加载关联关系
    $article->load('user');
    
    // 返回单个 Resource
    return new ArticleResource($article);
}

// ========================
// 创建文章后返回
// ========================
public function store(StoreArticleRequest $request)
{
    $article = Article::create($request->validated());
    
    return response()->json([
        'success' => true,
        'message' => '文章创建成功',
        'data'    => new ArticleResource($article)
    ], 201);
}
```

### 2.3.5 API Resource 常用写法总结表

| 写法                                      | 含义说明                                   | 常用场景                     | 推荐程度 |
|-------------------------------------------|--------------------------------------------|------------------------------|----------|
| `$this->id` / `$this->title`              | 直接返回模型字段                           | 基础字段                     | 必须     |
| `$this->whenLoaded('user')`               | 只有关联被加载时才返回                     | 关联数据（推荐）             | 强烈推荐 |
| `fn() => $this->user->name`               | 闭包方式返回关联的指定字段                 | 只返回部分关联信息           | 推荐     |
| `$this->created_at->toDateTimeString()`   | 格式化时间                                 | 时间字段统一格式             | 常用     |
| `$this->when($condition, fn() => ...)`    | 满足条件时才返回该字段                     | 条件返回                     | 推荐     |
| `new UserResource($this->user)`           | 嵌套其他 Resource                          | 复杂关联数据                 | 常用     |
| `ArticleResource::collection($articles)`  | 处理列表或分页数据                         | 列表接口                     | 必须     |


**小贴士：**

1. **优先使用 `whenLoaded()`**，可以有效防止 N+1 查询问题。
2. Resource 只负责「数据展示格式」，验证逻辑仍然放在 Form Request 中。
3. 如果列表需要额外包装数据（例如加上 `{'success': true, 'data': [...]}`），可以在控制器中再包一层 `response()->json()`。
4. `ArticleCollection` 类一般不需要额外修改，直接使用 `ArticleResource::collection()` 即可。


## 2.4 `API Resource` 是必需的吗？

**`API Resource` 不是必需的，但 AI 强烈建议使用。**

## 2.5 创建 Policy（授权）

`app\Policies\`

命令行：

```bash
php artisan make:policy ArticlePolicy --model=Article
```

**注册 Policy**（在 `app/Providers/AuthServiceProvider.php` 的 `boot()` 方法中）：

```php
<?php

namespace App\Providers;

use App\Models\Article;
use App\Policies\ArticlePolicy;
use Illuminate\Support\Facades\Gate;
use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;

class AuthServiceProvider extends ServiceProvider
{
    protected $policies = [
        Article::class => ArticlePolicy::class,
    ];

    public function boot(): void
    {
        $this->registerPolicies();
    }
}
```

**示例 Policy**：

```php
public function update(User $user, Article $article): bool
{
    return $user->id === $article->user_id; // 只有作者能修改
}
```

在路由或中间件中使用 `->middleware('can:update,article')`。


# 3. JR（JWT & Router）

**强烈推荐使用 `php-open-source-saver/jwt-auth`**（tymon/jwt-auth 的活跃 fork，兼容 Laravel 11）。

## 3.1 安装 JWT 包（命令行）

```bash
composer require php-open-source-saver/jwt-auth
```

发布配置文件：

```bash
php artisan vendor:publish --provider="PHPOpenSourceSaver\JWTAuth\Providers\LaravelServiceProvider"
php artisan jwt:secret   # 这会在 .env 中生成 JWT_SECRET
```

## 3.2 配置 `config/auth.php`

找到 `'guards'` 部分，修改或添加：

```php
'guards' => [
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],

    'api' => [
        'driver' => 'jwt',        // 改成 jwt
        'provider' => 'users',
    ],
],
```

默认 guard 可以改成 `'defaults' => ['guard' => 'api']`（可选）。

## 3.3 更新 User 模型 `app/Models/User.php`

添加 JWT 需要的接口和 trait：

```php
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use PHPOpenSourceSaver\JWTAuth\Contracts\JWTSubject;   // 关键

class User extends Authenticatable implements JWTSubject
{
    use HasFactory;

    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    public function getJWTCustomClaims()
    {
        return [];
    }

    // 其他代码...
}
```

## 3.4 路由 `routes/api.php`

先确保安装了 API 路由（Laravel 11 默认可能需要）：

```bash
php artisan install:api
```

然后编辑 `routes/api.php`：

```php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\AuthController;   // 你后面会创建
use App\Http\Controllers\ArticleController;

Route::post('/register', [AuthController::class, 'register']);
Route::post('/login', [AuthController::class, 'login']);

Route::middleware('auth:api')->group(function () {
    Route::post('/logout', [AuthController::class, 'logout']);
    Route::get('/me', [AuthController::class, 'me']);

    Route::apiResource('articles', ArticleController::class);
});
```

## 3.5 自定义 JWT 无效/过期返回

这是 Laravel 11 的新方式（不再用 Exception Handler）。

在 `bootstrap/app.php` 中修改 `withExceptions` 部分：

```php
<?php

use Illuminate\Foundation\Application;
use Illuminate\Foundation\Configuration\Exceptions;
use Illuminate\Foundation\Configuration\Middleware;
use Illuminate\Auth\AuthenticationException;

return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware) {
        //
    })
    ->withExceptions(function (Exceptions $exceptions) {
        $exceptions->render(function (AuthenticationException $e, $request) {
            if ($request->is('api/*') || $request->wantsJson()) {
                // 判断是否 JWT guard
                if (in_array('api', $e->guards()) || str_contains($e->getMessage(), 'JWT')) {
                    return response()->json([
                        'success' => false,
                        'message' => 'Token 无效或已过期，请重新登录',
                        'error' => 'unauthenticated',
                    ], 401);
                }
            }
            return null; // 其他异常走默认处理
        });
    })->create();
```

这样 Flutter 收到 401 时就能友好提示用户重新登录。

