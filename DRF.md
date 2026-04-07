## 前言
根据笔者的个人实战DRF项目所涉及到的知识做一个简要总结，温故而知新。

这是去年写的入门教程👉👉 [Django REST Framework](https://blog.csdn.net/2403_88863963/article/details/155992843?fromshare=blogdetail&sharetype=blogdetail&sharerId=155992843&sharerefer=PC&sharesource=2403_88863963&sharefrom=from_link)

## `request`

在 Django REST Framework (DRF) 中，`request` 对象是每个视图（View）的核心。虽然它看起来和原生 Django 的 `HttpRequest` 很像，但实际上 DRF 对其进行了**封装和增强**，使其更适合 API 开发。

### 1. 核心定位
DRF 的 `request` 对象是 `rest_framework.request.Request` 类的实例。它并不是替换了 Django 原生的 `HttpRequest`，而是通过**装饰器模式**将其包裹在内部。这意味着你依然可以访问原生请求的所有属性，同时享受 DRF 提供的更强大的数据解析功能。

---

### 2. 代码示例

以下是一个典型的 DRF 类视图示例，展示了如何使用 `request` 对象获取不同类型的数据：

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class UserProfileAPIView(APIView):
    def post(self, request, *args, **kwargs):
        # 1. 获取解析后的请求体数据（最常用）
        # 无论客户端发送的是 JSON、Form Data 还是其他格式，request.data 都能自动解析
        data = request.data
        username = data.get('username')

        # 2. 获取 URL 查询参数（即 GET 参数）
        # 推荐使用 .query_params 而不是原生的 .GET，语义更清晰
        is_active = request.query_params.get('active', 'true')

        # 3. 获取用户信息（由 DRF 认证系统自动填充）
        current_user = request.user
        
        # 4. 检查认证方式
        auth_info = request.auth  # 如果是 Token 认证，这里通常是 Token 对象

        # 5. 原生属性依然可用
        method = request.method
        path = request.path

        return Response({
            "message": f"Hello {username}",
            "is_active": is_active,
            "user_id": current_user.id if current_user.is_authenticated else None
        }, status=status.HTTP_200_OK)
```

---

### 3. 核心用法总结

| 属性/方法 | 描述 | 与原生 Django 的区别 |
| :--- | :--- | :--- |
| **`request.data`** | **解析后的请求体内容**。支持 JSON、表单等多种媒体类型。 | 相当于原生 `request.POST` + `request.FILES` 的加强版，且支持 `PUT`/`PATCH` 等方法。 |
| **`request.query_params`** | **URL 查询参数**。例如 `?id=1`。 | 与原生 `request.GET` 完全一致，但在 API 上下文中语义更准确。 |
| **`request.user`** | **当前经过认证的用户实例**。 | 由 DRF 的认证组件根据 Header（如 Token）自动处理，不再局限于 Session。 |
| **`request.auth`** | **认证凭据**。通常是 Token 对象或 None。 | 原生 Django 请求中没有这个专门的属性。 |
| **`request.parsers`** | 当前请求可用的解析器列表。 | DRF 特有，用于处理不同格式的输入。 |
| **`request._request`** | **访问原生 Django HttpRequest**。 | 如果某些插件强制要求原生请求对象，可以通过此属性获取。 |

### 💡 小贴士：
* **不要再用 `request.POST`**：在编写 API 时，请统一使用 `request.data`。因为 `request.POST` 只能处理表单数据，而 `request.data` 可以处理 JSON（现代前端最常用的格式）。
* **自动处理**：只要你的视图继承自 `APIView` 或使用了 `@api_view` 装饰器，传入的 `request` 就已经是 DRF 的增强版对象了。


## `admin`

既然你已经引入了 `from django.contrib import admin`，这意味着你准备进入 Django 的“杀手级特性”——**Admin 管理后台**。

它是 Django 自动为你生成的内置后台，让你无需写任何 HTML 就能直接对数据库模型（Models）进行增删改查。

---

### 1. 基础用法：注册模型
要让你的模型在后台显示，最简单的方法是使用 `admin.site.register()`。

假设你有一个模型叫 `Article`：

```python
# admin.py
from django.contrib import admin
from .models import Article

# 最简单的注册方式
admin.site.register(Article)
```

---

### 2. 进阶用法：使用装饰器自定义展示
如果你想控制后台列表页显示哪些列、搜索框、或者过滤器，建议使用 `ModelAdmin` 类并配合 **`@admin.register`** 装饰器。

```python
# admin.py
from django.contrib import admin
from .models import Article

@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    # 1. 列表页显示的字段
    list_display = ('title', 'author', 'created_time', 'is_published')
    
    # 2. 右侧侧边栏过滤器（按时间或分类过滤）
    list_filter = ('created_time', 'author')
    
    # 3. 搜索框（支持模糊查询标题和作者名）
    search_fields = ('title', 'author__username')
    
    # 4. 设置点击哪些字段可以进入编辑页
    list_display_links = ('title',)
    
    # 5. 每页显示多少条数据
    list_per_page = 20
```

---

### 3. 常用配置项详解

通过继承 `admin.ModelAdmin`，你可以深度定制后台体验：

| 属性 | 作用 |
| :--- | :--- |
| **`list_display`** | 定义列表页展示的列，可以是模型字段，也可以是自定义函数。 |
| **`list_editable`** | 允许在列表页直接编辑并保存字段（必须在 `list_display` 中）。 |
| **`ordering`** | 默认排序规则，例如 `['-created_time']` 表示按时间倒序。 |
| **`readonly_fields`** | 设置某些字段为只读，不可在后台修改。 |
| **`fieldsets`** | 在详情页对字段进行分组，增加可读性。 |

---

### 4. 一个完整的复杂示例
如果你想让后台变得更专业，比如添加关联模型的内联编辑（Inlines）：

```python
from django.contrib import admin
from .models import Article, Comment

# 定义评论的内联显示（在文章详情页直接管理评论）
class CommentInline(admin.TabularInline):
    model = Comment
    extra = 1  # 默认显示1个空白输入框

@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    list_display = ('title', 'status', 'view_count')
    inlines = [CommentInline] # 将评论内联到文章编辑页
    actions = ['make_published'] # 自定义动作

    # 自定义一个后台动作（批量修改状态）
    @admin.action(description='将选中的文章标记为已发布')
    def make_published(self, request, queryset):
        queryset.update(status='p')
```

---

### 💡 小贴士
1. **不要忘记 `createsuperuser`**：在使用 `admin` 之前，你需要通过命令行 `python manage.py createsuperuser` 创建一个管理员账号。
2. **访问地址**：启动服务器后，默认访问路径是 `http://127.0.0.1:8000/admin/`。
3. **安全建议**：在生产环境中，建议修改 `urls.py` 里的 `admin/` 路径为更隐蔽的名称，防止撞库攻击。

## `authenticate`

既然你引入了 `from django.contrib.auth import authenticate`，你现在正站在 Django 用户认证系统的核心关口。

`authenticate()` 的主要职责是：**验证凭据（用户名和密码）是否正确**。

---



### 1. 基础用法
它是最常用的身份验证方法。如果凭据有效，它会返回一个 `User` 对象；如果无效（密码错、用户不存在等），它会返回 `None`。

```python
from django.contrib.auth import authenticate

# 基本语法
user = authenticate(username='jack', password='password123')

if user is not None:
    # 身份验证成功，且该用户是活跃状态 (is_active=True)
    print(f"欢迎回来，{user.username}!")
else:
    # 验证失败
    print("用户名或密码错误")
```

---

### 2. 结合登录逻辑 (Login)
在实际开发中，`authenticate` 通常与 `login` 函数配合使用。`authenticate` 负责**核对身份**，`login` 负责**创建会话 (Session)**。

```python
from django.contrib.auth import authenticate, login
from django.http import HttpResponse

def my_login_view(request):
    # 假设从 POST 请求中获取数据
    user_name = request.POST.get('username')
    pass_word = request.POST.get('password')

    # 第一步：核对身份
    user = authenticate(request, username=user_name, password=pass_word)

    if user is not None:
        # 第二步：建立会话
        login(request, user)
        return HttpResponse("登录成功！")
    else:
        return HttpResponse("凭据无效，请重试。", status=401)
```

---

### 3. 注意事项与核心特性

#### 校验状态
`authenticate()` 不仅仅检查密码是否正确，它还会检查用户的 `is_active` 状态。
* 如果用户存在且密码正确，但 `is_active=False`，`authenticate` 会返回 `None`（除非你在配置中特殊处理）。

#### 自动处理密码哈希
你不需要手动对密码进行加密比对。Django 会自动读取数据库中的哈希值，并使用当前的加密算法对输入进行校验。

#### 多后端支持
Django 支持多个认证后端（比如：同时支持用户名登录和邮箱登录）。`authenticate` 会依次轮询 `AUTHENTICATION_BACKENDS` 配置中的每一个后端，直到其中一个验证通过。

---

### 4. 常见坑点

| 现象 | 原因 | 解决方案 |
| :--- | :--- | :--- |
| **返回 None** | 用户名不存在 | 确认 `User.objects.filter(username=...).exists()` |
| **返回 None** | 密码输入错误 | 检查是否有多余空格或大小写 |
| **返回 None** | 用户被禁用 | 检查该用户的 `is_active` 是否为 `True` |
| **传入 request** | 虽非必须，但推荐 | 传入 `request` 对象可以方便某些自定义认证后端记录 IP 或日志 |

---

### 进阶技巧：支持邮箱登录
如果你想让用户能用邮箱登录，你需要自定义一个 Backend，然后在 `authenticate` 调用时，它会自动识别并处理：

```python
# 在 settings.py 中配置
AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend', # 默认的用户名登录
    'yourapp.backends.EmailBackend',             # 你自定义的邮箱登录
]
```

## `User`

引入 `from django.contrib.auth.models import User` 后，你就拿到了 Django 自带的**内置用户模型**。它是 Django 身份验证系统的核心，包含了用户名、密码、邮箱等基础字段。

---




### 1. 基础用法：创建用户
创建用户时，**绝对不能**直接使用 `User.objects.create()`，因为这样密码会以明文存储。必须使用模型管理器提供的 `create_user` 方法。

```python
from django.contrib.auth.models import User

# 1. 创建普通用户（自动处理密码哈希）
user = User.objects.create_user(username='lucy', email='lucy@example.com', password='safe_password_123')

# 2. 创建超级管理员
admin_user = User.objects.create_superuser(username='boss', email='admin@example.com', password='top_secret_password')
```

---

### 2. 常用字段与属性
每个 `User` 实例自带以下常用字段：

| 类别 | 字段名 | 说明 | 示例 / 返回值 |
| :--- | :--- | :--- | :--- |
| **核心标识** | `id` / `pk` | 数据库主键，自动生成的唯一标识。 | `user.id` (如 `1`) |
| | **`username`** | **用户名**。必填且唯一，通常用于登录。 | `'johndoe'` |
| | `password` | 存储哈希后的密码。**严禁**直接访问或修改。 | `pbkdf2_sha256$...` |
| **个人信息** | **`email`** | 电子邮箱。 | `'example@mail.com'` |
| | `first_name` | 名字。 | `'Jerry'` |
| | `last_name` | 姓氏。 | `'Smith'` |
| **状态标志** | **`is_active`** | **布尔值**。决定账号是否有效。建议用此字段代替物理删除。 | `True` |
| | **`is_staff`** | **布尔值**。决定用户是否可以访问 Django Admin 管理后台。 | `False` |
| | **`is_superuser`** | **布尔值**。超级用户，自动拥有所有权限。 | `True` |
| **时间记录** | **`date_joined`** | 用户账号创建的确切日期和时间。 | `2024-05-20 10:00:00` |
| | `last_login` | 用户最后一次成功登录的时间。 | `2026-04-03 09:30:00` |
| **权限管理** | `groups` | 该用户所属的所有 **Group**（多对多关系）。 | `user.groups.all()` |
| | `user_permissions` | 该用户特有的权限（不通过组继承的权限）。 | `user.user_permissions.all()` |

---

### 3. 核心方法示例
`User` 对象自带了一些非常实用的逻辑方法：

```python
user = User.objects.get(username='lucy')

# 1. 校验密码是否正确
if user.check_password('safe_password_123'):
    print("密码验证通过")

# 2. 修改密码（会自动哈希加密）
user.set_password('new_very_safe_password')
user.save()

# 3. 权限判断
print(user.has_perm('myapp.change_article')) # 是否有修改文章的权限
```

---

### 4. 查询与过滤
因为 `User` 是一个标准的 Django Model，你可以像操作其他模型一样操作它：

```python
# 获取所有活跃用户
active_users = User.objects.filter(is_active=True)

# 模糊查询用户名包含 "admin" 的人
admins = User.objects.filter(username__contains='admin')
```

---

### 5. 进阶：如何关联自定义数据？
由于内置 `User` 字段有限，通常有两种方式扩展：

#### 方案 A：使用 OneToOneField（最常用）
创建一个 `UserProfile` 模型来存储额外信息（如头像、手机号）。

```python
from django.db import models
from django.contrib.auth.models import User

class UserProfile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    phone = models.CharField(max_length=11)
    avatar = models.ImageField(upload_to='avatars/')
```

#### 方案 B：继承 AbstractUser（推荐新项目使用）
如果你还在项目初期，建议自定义一个用户类继承 `AbstractUser`。
> **注意**：这需要在 `settings.py` 中配置 `AUTH_USER_MODEL`。

---

### ⚠️ 避坑指南
* **密码加密**：再次强调，永远用 `set_password()` 或 `create_user()`，不要直接对 `password` 字段赋值字符串。
* **邮箱唯一性**：Django 默认不强制要求 `email` 唯一，如果你需要邮箱登录，需要自定义模型或通过表单逻辑校验。


## `models`

在 Django 开发中，`models.py` 是定义数据结构的灵魂。它不仅映射数据库表，还包含了业务逻辑和数据校验。

以下我将通过一个名为 **“星际图书馆 (StarLibrary)”** 的示例，为你详细介绍 Django Model 的核心用法、字段定义以及各种实用的“语法糖”。

---

### 1. 基础结构与字段定义

每个模型都是 `django.db.models.Model` 的子类。

```python
from django.db import models

class Book(models.Model):
    # --- 常用字段类型 ---
    # 字符串：必须指定 max_length
    title = models.CharField("书名", max_length=200)
    
    # 长文本：不需要 max_length
    summary = models.TextField("简介", blank=True)
    
    # 数字：DecimalField 适合存钱（高精度），FloatField 适合科学计算
    price = models.DecimalField("价格", max_digits=10, decimal_places=2)
    
    # 布尔值：默认值设置
    is_available = models.BooleanField("是否有货", default=True)
    
    # 时间：auto_now_add(创建时记录), auto_now(每次保存更新)
    published_at = models.DateField("出版日期")
    created_at = models.DateTimeField(auto_now_add=True)

    # --- 关联关系 ---
    # 1:N 关系 (ForeignKey)
    # on_delete 决定了当作者被删除时，书该怎么办
    author = models.ForeignKey('Author', on_delete=models.CASCADE, related_name="books")

    class Meta:
        verbose_name = "书籍"
        ordering = ['-created_at'] # 默认排序：按创建时间倒序
```

---

### 2. 字段参数的“潜规则”

Django 提供了一些非常有用的参数来控制数据库行为和后台展示：

* **`null=True`**: 数据库层面允许为空（针对 `NULL`）。
* **`blank=True`**: 表单验证层面允许为空（针对 Django Admin 或 Serializer 的校验）。
* **`choices`**: 极其好用的语法糖，用于定义“下拉菜单”式的枚举值。

```python
class Author(models.Model):
    LEVEL_CHOICES = [
        ('S', '资深'),
        ('A', '中级'),
        ('B', '萌新'),
    ]
    name = models.CharField(max_length=50)
    level = models.CharField(max_length=1, choices=LEVEL_CHOICES, default='B')
```

---

### 3. 模型层的“语法糖”与逻辑封装

Model 不仅仅是数据，它也可以包含方法。

#### 3.1 `__str__` 方法
这是最常用的语法糖，定义了对象在 Django Admin 或 `print()` 时的显示方式。
```python
def __str__(self):
    return f"<{self.title}> - {self.author.name}"
```

#### 3.2 自定义属性 (Property)
可以像访问字段一样访问一个计算出来的值。
```python
@property
def is_expensive(self):
    return self.price > 100
```

#### 3.3 重写 `save()` 或 `delete()`
在数据入库前或销毁前执行逻辑（例如：自动生成编号、清理文件、触发通知）。
```python
def save(self, *args, **kwargs):
    # 逻辑：如果价格超过1000，自动标记为不可售（仅作示例）
    if self.price > 1000:
        self.is_available = False
    super().save(*args, **kwargs) # 务必调用父类方法，否则存不进数据库
```

---

### 4. 进阶用法：Meta 配置与约束

`class Meta` 用于定义不属于字段的“元数据”。

```python
class BookInventory(models.Model):
    book = models.ForeignKey(Book, on_delete=models.CASCADE)
    store_name = models.CharField(max_length=100)
    stock_count = models.IntegerField(default=0)

    class Meta:
        # 联合唯一：同一个店里不能有两个相同的书记录
        unique_together = ('book', 'store_name')
        
        # 给模型起个好听的中文名（Admin后台显示）
        verbose_name = "库存详情"
        verbose_name_plural = "库存详情列表"
```



---

### 5. `on_delete` 策略详解

在 Django 中，`on_delete` 是 `ForeignKey`（一对多）和 `OneToOneField`（一对一）中**必须指定**的参数。它决定了当“被引用对象”（父表）被删除时，“引用对象”（子表）该何去何从。

想象一下：如果你的 **“星际图书馆”** 里有一位作者，他写了 10 本书。如果这位作者决定退出文坛并删除了他的账号，这 10 本书该怎么办？

以下是 Django 提供的几种处理方案：

| 策略名称 | 行为描述 | 适用场景 |
| :--- | :--- | :--- |
| **`models.CASCADE`** | **级联删除**。父对象删了，子对象也跟着一起消失。 | **最常用**。如：文章被删，文章下的评论也该被删。 |
| **`models.SET_NULL`** | **置空**。父对象删了，子对象保留，但关联字段设为 `NULL`。 | **需配合 `null=True`**。如：员工离职了，但他负责的设备还在，只是暂时无人认领。 |
| **`models.PROTECT`** | **保护模式**。如果还有子对象关联着，就不允许删除父对象。 | **安全性极高**。如：只要仓库里还有库存，就不允许删除该商品类目。 |
| **`models.SET_DEFAULT`** | **设为默认值**。父对象删了，子对象关联到某个默认值上。 | **需配合 `default=xxx`**。如：原作者注销，文章自动归属到“匿名用户”名下。 |
| **`models.DO_NOTHING`** | **啥也不干**。不采取任何行动，但这可能导致数据库完整性报错。 | 极少使用，除非你在数据库层面有特殊的触发器逻辑。 |

#### 代码示例：

```python
class Book(models.Model):
    title = models.CharField(max_length=100)
    
    # 场景 A：作者没了，书也别留了 (最狠心)
    author = models.ForeignKey('Author', on_delete=models.CASCADE)

    # 场景 B：作者没了，把书的作者设为空 (最佛系)
    # 记得必须设置 null=True，否则数据库会报错
    contributor = models.ForeignKey('Author', on_delete=models.SET_NULL, null=True)

    # 场景 C：只要这人还有书在架上，就不准删这人 (最严谨)
    original_creator = models.ForeignKey('Author', on_delete=models.PROTECT)
```



---

#### 💡 深度建议：
* **级联删除（CASCADE）要慎用**：在复杂的业务系统中，误删一个父级节点（如一个部门）可能导致成千上万条子数据（如员工信息、考勤记录）瞬间消失，且难以恢复。
* **推荐使用 `SET_NULL` 或软删除**：对于核心业务数据，通常建议设置 `on_delete=models.SET_NULL`，或者在模型中添加一个 `is_deleted = models.BooleanField(default=False)` 字段，通过逻辑而非物理手段来“删除”数据。
---

### 6. 快速上手：常用操作小抄

如果你想在代码中调用这些 Model：

| 操作 | 代码示例 |
| :--- | :--- |
| **创建** | `Book.objects.create(title="Python进阶", price=99.0, ...)` |
| **查询(全部)** | `Book.objects.all()` |
| **过滤** | `Book.objects.filter(price__gt=50)` (价格 > 50) |
| **获取单条** | `Book.objects.get(id=1)` (不存在会报错) |
| **模糊查询** | `Book.objects.filter(title__icontains="Python")` |
| **排序** | `Book.objects.order_by('-price')` (价格从高到低) |

---

### 总结建议
1.  **逻辑写在 Model 里**：尽量遵循 "Fat Models, Skinny Views"（胖模型、瘦视图）原则。如果你在 `views.py` 里写了大量的 `if/else` 来处理数据逻辑，考虑把它们搬进 `models.py` 的方法里。
2.  **善用 `related_name`**：在 `ForeignKey` 中定义它，可以让你通过作者直接反查书籍，如 `author.books.all()`，这比默认的 `author.book_set.all()` 语义更清晰。


## `Q`

在 Django 查询中，`from django.db.models import Q` 是一个非常强大的工具。

简单来说，Django 默认的 `.filter()` 里的参数是 **“与 (AND)”** 关系。如果你想实现 **“或 (OR)”**、**“非 (NOT)”** 或者更复杂的逻辑嵌套，就必须用到 `Q` 对象。

以下通过一个“星际探险队 (SpaceExpedition)”的例子来介绍它的用法：

---


### 1. 基础用法：实现“或 (OR)”逻辑
在普通查询中，如果你写 `filter(name='A', rank='B')`，Django 会寻找同时满足这两个条件的记录。使用 `Q` 对象配合 `|` 符号，可以实现“或”。

```python
from django.db.models import Q
from .models import Explorer

# 查询名字叫 "Captain Solo" 或者 军衔是 "Commander" 的探险员
results = Explorer.objects.filter(
    Q(name="Captain Solo") | Q(rank="Commander")
)
```

---

### 2. 实现“非 (NOT)”逻辑
使用 `~` 符号可以对 `Q` 对象取反，这比 `.exclude()` 在处理复杂组合时更灵活。

```python
# 查询所有 军衔不是 "Trainee" (实习生) 的探险员
results = Explorer.objects.filter(~Q(rank="Trainee"))
```

---

### 3. 复杂逻辑嵌套
你可以把 `Q` 对象像数学公式一样嵌套起来。

```python
# 查询场景：
# (级别大于 5 且 状态为活跃) 
# 或者 
# (属于 "Alpha" 舰队)
results = Explorer.objects.filter(
    (Q(level__gt=5) & Q(is_active=True)) | Q(fleet="Alpha")
)
```

---

### 4. 动态构建查询条件 (语法糖)
这是 `Q` 对象最实用的高级技巧。如果你有一个关键词列表，想查询标题包含其中**任意一个**关键词的记录，可以用循环动态构造。

```python
keywords = ["Mars", "Jupiter", "Saturn"]
query = Q() # 创建一个空的 Q 对象

for word in keywords:
    # 动态累加 "或" 条件
    query |= Q(destination__icontains=word)

# 最终执行查询
results = Explorer.objects.filter(query)
```

---

### 5. 注意事项：混合使用顺序
如果你同时使用 `Q` 对象和普通的关键字参数，**`Q` 对象必须放在前面**。

```python
# ✅ 正确写法
Explorer.objects.filter(
    Q(rank="Captain"),
    is_active=True
)

# ❌ 错误写法 (会抛出语法错误)
# Explorer.objects.filter(is_active=True, Q(rank="Captain"))
```

### 总结
| 符号 | 逻辑 | 说明 |
| :--- | :--- | :--- |
| `&` | **AND** | 与逻辑（默认也是 AND，但用于连接 Q 对象） |
| `\|` | **OR** | 或逻辑 |
| `~` | **NOT** | 非逻辑（取反） |



`Q` 对象本质上是将查询条件**封装成了一个 Python 对象**，使其可以进行位运算和动态传递，极大地提升了 Django ORM 的灵活性。

## `DjangoFilterBackend`

`DjangoFilterBackend` 是 Django REST Framework (DRF) 中最常用的“语法糖”之一。它的核心作用是：**让你不需要手动解析 URL 参数，就能自动实现数据库过滤。**

如果不使用它，你需要从 `request.query_params` 手动提取字段并写 `filter()`；使用它后，你只需要在代码中声明一下即可。

以下通过一个**“星际贸易系统 (StarTrade)”**的例子来介绍用法：

---

### 1. 基础用法：全自动过滤
假设你有一个商品模型 `Product`，你想通过 URL 快速筛选类别或价格。

#### 模型与序列化器 (参考)
```python
# models.py
class Product(models.Model):
    name = models.CharField(max_length=100)
    category = models.CharField(max_length=50) # 比如：燃料, 飞船, 机器人
    is_in_stock = models.BooleanField(default=True)
```

#### 视图配置 (关键部分)
在 ViewSet 中添加 `filter_backends` 和 `filterset_fields`：

```python
from rest_framework import viewsets
from django_filters.rest_framework import DjangoFilterBackend
from .models import Product
from .serializers import ProductSerializer

class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    
    # 1. 声明使用的过滤器后端
    filter_backends = [DjangoFilterBackend]
    
    # 2. 声明哪些字段允许被过滤 (精确匹配)
    filterset_fields = ['category', 'is_in_stock']
```

#### 如何在 API 中调用？
一旦配置完成，你无需修改代码，直接在 URL 后面加参数即可：
* **筛选类别：** `GET /api/products/?category=燃料`
* **组合筛选：** `GET /api/products/?category=机器人&is_in_stock=true`

---

### 2. 进阶用法：范围与模糊查询
基础用法只能做“等于”判断。如果你想实现“价格大于等于 (>=)”或者“名字包含 (contains)”，需要配合 `FilterSet` 类使用。



#### 定义过滤器类
```python
from django_filters import rest_framework as filters
from .models import Product

class ProductFilter(filters.FilterSet):
    # 定义过滤逻辑
    min_price = filters.NumberFilter(field_name="price", lookup_expr='gte') # 大于等于
    max_price = filters.NumberFilter(field_name="price", lookup_expr='lte') # 小于等于
    name_keyword = filters.CharFilter(field_name="name", lookup_expr='icontains') # 包含(不区分大小写)

    class Meta:
        model = Product
        fields = ['category', 'min_price', 'max_price', 'name_keyword']
```

#### 在视图中使用自定义类
```python
class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = [DjangoFilterBackend]
    
    # 使用 filterset_class 替换掉之前的 filterset_fields
    filterset_class = ProductFilter
```

#### 此时的 API 调用：
* **价格范围查询：** `GET /api/products/?min_price=100&max_price=500`
* **搜索名字：** `GET /api/products/?name_keyword=阿波罗`

---

### 3. 全局配置 (小贴士)
如果你希望全站所有的 `ModelViewSet` 默认都支持这个过滤功能，可以在 `settings.py` 中全局配置，这样就不用在每个 View 里写 `filter_backends` 了：

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': [
        'django_filters.rest_framework.DjangoFilterBackend',
    ],
}
```



### 总结：为什么要用它？
1.  **代码解耦**：查询逻辑从 View 中剥离，视图只负责逻辑分发。
2.  **自动文档**：如果你使用了 Swagger/OpenAPI，这些过滤字段会自动出现在文档的参数列表中。
3.  **安全性**：它会自动处理数据类型转换（比如把 URL 里的字符串 `"true"` 转成布尔值 `True`），防止 SQL 注入风险。
---

## `FBV` 和 `CBV`

### 1. 函数视图（Function-Based Views）

函数视图是使用普通 Python 函数编写 DRF 接口的方式，必须配合 `@api_view` 装饰器使用。

#### 基础写法示例（views.py）：

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from .models import Book
from .serializers import BookSerializer

@api_view(['GET', 'POST'])          # 必须加这一行
# 还可以是其它方法，例如 @api_view(['DELETE'])
def book_list(request):
    """获取书籍列表 或 创建新书籍"""
    if request.method == 'GET':
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)
    
    elif request.method == 'POST':
        serializer = BookSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

#### 如果不加 `@api_view` 会怎样？

如果去掉 `@api_view` 装饰器，直接这样写：

```python
def book_list(request):   # 没有 @api_view
    ...
```

**后果**：
- DRF 无法正确识别该视图为 REST 接口
- 所有请求都会返回 **405 Method Not Allowed**
- 失去 DRF 的核心功能：内容协商、Request 对象增强、统一的异常处理、权限认证支持等
- 实际使用中几乎无法正常工作，退化成普通 Django 视图

**结论**：函数视图**必须**加上 `@api_view(['GET', 'POST', ...])`，否则基本不可用。

#### 装饰器的正确书写顺序（强烈推荐）

在函数视图中，所有策略装饰器 **必须放在 `@api_view` 的下方**，推荐顺序如下：

```python
from rest_framework.decorators import (
    api_view,
    authentication_classes,
    permission_classes,
    parser_classes,
    throttle_classes,      # 可选
    renderer_classes       # 可选
)

@api_view(['GET', 'POST'])                    # 第一行：必须在最上面
@authentication_classes([...])                # 认证（推荐放在较上方）
@permission_classes([...])                    # 权限
@parser_classes([...])                        # 数据解析器
@throttle_classes([...])                      # 限流（可选）
@renderer_classes([...])                      # 渲染器（可选）
def my_view(request):
    ...
```

**推荐的实际书写顺序**（最常用）：

```python
@api_view(['GET', 'POST'])
@authentication_classes([TokenAuthentication, SessionAuthentication])
@permission_classes([IsAuthenticated])
@parser_classes([JSONParser, MultiPartParser])
def book_list(request):
    ...
```

#### 为什么要有这个顺序？

- **`@api_view`** 必须放在**最上面**（最外层）：  
  它把普通函数转换为 DRF 的视图函数，并负责后续装饰器的识别。如果其他装饰器写在它上面，可能会失效或报错。

- 其他策略装饰器（`@authentication_classes`、`@permission_classes` 等）**必须写在 `@api_view` 的下方**。

- **内部执行顺序**（DRF 实际运行时）与装饰器书写顺序不完全相同：
  1. **认证（Authentication）**：最早执行，识别用户身份（填充 `request.user` 和 `request.auth`）。
  2. **权限检查（Permission）**：认证之后立即执行，根据 `request.user` 判断是否允许访问。
  3. **解析器（Parser）**：当访问 `request.data` 时才触发，用于解析请求体（JSON、文件等）。
  4. 限流、渲染器等在适当时机执行。

虽然执行顺序是固定的，但**书写时推荐按 “认证 → 权限 → 解析器”** 的逻辑顺序排列，代码更清晰易读。

#### 函数视图的路由配置（urls.py）：

```python
from django.urls import path
from . import views

urlpatterns = [
    path('books/', views.book_list, name='book-list'),   # 直接写函数名，不加 as_view()
]
```

---

### 2. 类视图（Class-Based Views） —— APIView（推荐方式）

DRF 提供的基于类的视图，最基础的类是 `APIView`。

#### 示例代码（views.py）：

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Book
from .serializers import BookSerializer

class BookListAPIView(APIView):
    """书籍列表视图"""
    
    def get(self, request):
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)
    
    def post(self, request):
        serializer = BookSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

#### 类视图的路由配置（urls.py）：

```python
from django.urls import path
from .views import BookListAPIView

urlpatterns = [
    path('books/', BookListAPIView.as_view(), name='book-list'),  # 必须加 .as_view()
]
```

---

### 函数视图 vs 类视图（APIView）对比

| 对比维度           | 函数视图 (`@api_view`)                  | 类视图 (`APIView`)                        |
|--------------------|-----------------------------------------|-------------------------------------------|
| 代码风格           | 函数式                                  | 面向对象                                  |
| 是否需要装饰器     | 必须加 `@api_view`                      | 不需要                                    |
| 路由写法           | `views.book_list`                       | `views.BookListAPIView.as_view()`         |
| HTTP 方法处理      | 用 `if request.method == 'GET'`         | 用 `def get(self, request)`               |
| 代码复用性         | 较差                                    | 优秀（支持继承、Mixin）                   |
| 可读性与维护性     | 简单接口时较好                          | 中大型项目更清晰                          |
| 扩展性             | 一般                                    | 非常好                                    |
| 适合场景           | 简单接口、学习、快速原型                | **实际项目主流推荐**                      |
| DRF 功能集成       | 需要装饰器开启                          | 天然支持                                  |

---

### 开发建议

- **小型项目或初学**：可以使用函数视图（必须加 `@api_view`）。
- **中大型项目**：**强烈推荐使用类视图（APIView）**，结构更好，后期维护和扩展更方便。
- DRF 还提供了更高级的类视图（如 `ModelViewSet` 等），可以进一步减少重复代码。

---


## `urlpatterns`

### 1. 什么是 `urlpatterns`？

`urlpatterns` 是 Django 项目中**最核心的 URL 配置列表**。  
它告诉 Django：当用户访问某个地址时，应该交给哪个视图（view）来处理。

简单来说：  
**URL → 视图函数 / 类视图** 的映射关系，都写在 `urlpatterns` 里面。

### 2. 基本结构

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    # path(访问的路径, 对应的视图, 可选的名称)
    path('admin/', admin.site.urls),
    path('api/', include('students.urls')),
]
```

### 3. 常见写法说明

| 写法                              | 含义说明                                      | 使用场景                  |
|-----------------------------------|-----------------------------------------------|---------------------------|
| `path('admin/', admin.site.urls)` | 把 `/admin/` 开头的地址交给 Django 后台管理   | 管理后台                  |
| `path('api/', include('app.urls'))` | 把 `/api/` 下的所有地址交给另一个 urls.py 文件 | API 接口、模块化拆分      |
| `path('', include(router.urls))`  | 把 DRF ViewSet 生成的所有路由加进来          | REST API（ViewSet）       |
| `path('login/', LoginView.as_view())` | 把 `/login/` 交给某个类视图处理             | 自定义 API 接口           |

### 4. 实际例子（通用版）

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),                    # 管理后台
    
    path('api/', include('students.urls')),             # 把所有 API 接口交给 students 应用处理
    
    # 如果有其他普通页面，也可以在这里添加
    # path('about/', views.about, name='about'),
]

# 仅在开发环境提供静态文件
if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

### 5. 重要特点

- `urlpatterns` 必须是一个 **Python 列表**（list）。
- 列表中的每一项通常用 `path()` 或 `re_path()` 创建。
- 路径匹配是**从上到下**顺序进行的，**先匹配到的就生效**。
- 支持使用 `include()` 把不同应用的 URL 拆分到各自的 `urls.py` 文件中（推荐做法，项目大了不会乱）。
- DRF 的 `router.urls` 也需要加入这个列表中，否则 ViewSet 生成的接口会无法访问。

### 6. 小建议

1. 把 API 相关的路由尽量放在 `api/` 前缀下，便于区分。
2. 项目变大后，不要把所有路径都堆在主 `urls.py` 里，应该用 `include()` 拆分到各个 App 的 `urls.py`。
3. 养成给重要路径加上 `name=` 的习惯，方便以后反向解析 URL。

---

## `path`, `include`

在 Django 开发中，`path` 和 `include` 是构建路由系统（URLConf）的两大基石。简单来说：**`path` 用于定义具体的路线，而 `include` 用于分发任务。**

为了保护你的项目机密，我们以一个**“银河航线系统 (GalaxyRoutes)”**为例进行讲解。

---

### 1. `path`：定义单条路由
`path` 函数负责将一个具体的 URL 路径映射到一个特定的视图（View）函数上。

**语法结构：**
`path(route, view, kwargs=None, name=None)`

* **route**: 字符串，URL 模式。
* **view**: 函数视图或加了 `as_view()` 后的类视图。
* **name**: 为该路由命名（非常重要，用于在模板或代码中反向解析 URL）。

#### 代码示例
假设我们有一个处理飞船降落的视图：

```python
from django.urls import path
from . import views
from .views import ShipListView    # 继承自 APIView 的类视图

urlpatterns = [
    path('land/', views.land_ship, name='ship-landing'),
    path('ships/', ShipListView.as_view(), name='ship-list'),
]

```

---

### 2. `include`：路由分发（解耦必备）
当项目变大时，如果把成百上千个 URL 全写在一个文件里会非常混乱。`include` 的作用是**“路由切分”**。它允许你将相关的 URL 集合放在各自的应用（App）文件夹内。



#### 代码示例

- 第一步：在项目主路由中引入
```python
# main_project/urls.py
from django.urls import path, include

urlpatterns = [
    # 只要 URL 是以 'fleet/' 开头的，都交给 star_fleet 应用（App）处理
    path('fleet/', include('star_fleet.urls')),
]
```

- 第二步：在应用（App）内部创建 `urls.py`
假设应用名为 `star_fleet`：

```python
# star_fleet/urls.py
from django.urls import path
from . import views
from .views import FleetDetailView # 继承自 APIView 的类

urlpatterns = [
    path('status/', views.fleet_status, name='status'),
    path('orders/', views.fleet_orders, name='orders'),
    path('detail/', FleetDetailView.as_view(), name='fleet-detail'),
]
```

**效果：**
* 访问 `fleet/status/` 会触发 `fleet_status` 函数视图。
* 访问 `fleet/orders/` 会触发 `fleet_orders` 函数视图。
* 访问 `fleet/detail/` 会触发 `FleetDetailView` 类视图。

---

### 总结
* **`path`** 是**执行者**：告诉 Django “看到这个路径，去运行那个函数”。
* **`include`** 是**指挥官**：告诉 Django “看到这一类路径，去那个 App 的配置文件里找答案”。

## `APIView`

在 Django REST Framework (DRF) 中，`APIView` 是所有视图的**基石**。

如果说 `ModelViewSet` 是“全自动洗衣机”（省事但灵活度受限），那么 `APIView` 就是“手工洗板”（你需要自己处理逻辑，但拥有**绝对的控制权**）。它继承自 Django 的 `View` 类，但专门为 API 场景做了增强。

以下通过一个**“深空资源调度系统 (DeepSpaceDispatch)”**为例进行介绍：

---

### 1. `APIView` 的核心功能
与普通的 Django View 相比，`APIView` 自动处理了以下繁琐工作：
* **内容协商**：自动根据客户端请求（如 Header 里的 Accept）决定返回 JSON 还是 XML。
* **请求解析**：将原始请求封装成 DRF 的 `Request` 对象，你可以直接用 `request.data` 获取 POST 数据，不用再分 `request.POST` 和 `json.loads`。
* **权限与认证**：可以为每个视图单独配置谁能访问、如何识别身份。

---

### 2. 基础用法示例
这是一个处理“能源晶体（Crystals）”查询和创建的自定义接口：

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from .models import Crystal
from .serializers import CrystalSerializer

class SimpleAPI(APIView):
    """最简单的完整API - 5个方法全了"""
    
    # 1. 查全部
    def get(self, request):
        items = Crystal.objects.all()
        return Response(CrystalSerializer(items, many=True).data)
    
    # 2. 查单个
    def get_one(self, request, id):
        item = Crystal.objects.get(id=id)
        return Response(CrystalSerializer(item).data)
    
    # 3. 增
    def post(self, request):
        s = CrystalSerializer(data=request.data)
        if s.is_valid(): 
            s.save()
            return Response(s.data)
        return Response(s.errors)
    
    # 4. 改
    def put(self, request, id):
        item = Crystal.objects.get(id=id)
        s = CrystalSerializer(item, data=request.data)
        if s.is_valid(): 
            s.save()
            return Response(s.data)
        return Response(s.errors)
    
    # 5. 删
    def delete(self, request, id):
        Crystal.objects.get(id=id).delete()
        return Response({"msg": "已删除"})

```
- **对应关系总结** ：
```
GET     /crystals/        → get()        # 查所有
GET     /crystals/1/      → get_one()    # 查单个  
POST    /crystals/        → post()       # 增
PUT     /crystals/1/      → put()        # 改
DELETE  /crystals/1/      → delete()     # 删
```
---

### 3. 如何配置路由？
由于 `APIView` 是**类视图(CBV)**，在 `urls.py` 中需要调用 `.as_view()`：

```python
from django.urls import path
from .views import CrystalManagerView

urlpatterns = [
    path('crystals/', CrystalManagerView.as_view(), name='crystal-manage'),
]
```

---

### 4. 为什么要用 `APIView` 而不是 `ViewSet`？
虽然 `ViewSet` 代码更少，但在以下场景下，`APIView` 是更好的选择：

* **非数据库操作**：比如你要写一个接口去调用第三方天气 API，或者执行一段复杂的计算逻辑，并没有对应的数据库 Model。
* **极度复杂的逻辑**：当你的接口逻辑涉及 4-5 个不同的模型，或者需要根据复杂的 Header 信息做分支处理时，`ViewSet` 的默认行为反而会成为阻碍。
* **单一功能接口**：比如“发送短信验证码”、“文件上传解析”，这些接口通常只需要一个 `POST` 方法。

---

### 5. 关键语法糖：`Response` 与 `status`
在 `APIView` 中，你几乎总是配合这两者使用：
* **`Response(data)`**：你只需传一个 Python 字典或列表，它会自动帮你序列化成 JSON。
* **`status`**：提供语义化的 HTTP 状态码（如 `status.HTTP_204_NO_CONTENT`），比死记硬背数字 `204` 更有读性。



### 6. 总结
`APIView` 是 DRF 的“灵魂”。掌握了它，你就掌握了控制 API 行为的最高权限。通常建议：**简单的 CRUD 用 ViewSet，复杂的自定义逻辑用 APIView。**


## `ModelViewSet`

在 Django REST Framework (DRF) 中，`ModelViewSet` 是开发效率的“终极武器”。它将常见的数据库操作（增删改查）封装成了一个高度自动化的类。

>冷知识： `APIView` 其实是 `ModelViewSet` 的 “祖先” ，`ModelViewSet` 根据子类继承父类的关系逐步向上追溯，最终可以追溯到 `APIView` ；换句话说， `APIView` 是 `ModelViewSet` 的间接父类。

如果你有一个模型（Model），并且想通过最少的代码提供标准的 RESTful 接口，`ModelViewSet` 是最佳选择。为了确保安全，我们以一个 **“银河航线管理系统 (GalaxyAirways)”** 为例进行详细介绍。

---

### 1. `ModelViewSet` 的核心逻辑：五合一
通常，一个完整的资源管理需要编写多个视图。而 `ModelViewSet` 默认集成了以下五种行为：

| HTTP 方法 | URL 范例 | 对应 Action | 业务逻辑 |
| :--- | :--- | :--- | :--- |
| **GET** | `/flights/` | `list` | 列出所有航线 |
| **POST** | `/flights/` | `create` | 创建新航线 |
| **GET** | `/flights/{id}/` | `retrieve` | 获取某条航线的详情 |
| **PUT / PATCH** | `/flights/{id}/` | `update` | 修改航线信息 |
| **DELETE** | `/flights/{id}/` | `destroy` | 删除某条航线 |



---

### 2. 基础用法：极简配置
你只需要提供 `queryset` 和 `serializer_class`（序列化器）。

```python
from rest_framework.viewsets import ModelViewSet
from .models import Flight
from .serializers import FlightSerializer

class FlightViewSet(ModelViewSet):
    # 1. 定义 queryset ：告诉 DRF 去哪里（数据源）查数据
    queryset = Flight.objects.all().order_by('-departure_time')
    
    # 2. 定义序列化器：告诉 DRF 如何转换数据
    serializer_class = FlightSerializer
```

#### `queryset` 可以用在哪里？

>**答案**：它主要用于ModelViewSet，并不是所有视图都能直接使用。

支持范围一览表：

| 视图类型                              | 是否支持 `queryset` | 使用方式 | 备注 |
|--------------------------------------|-----------------------------|----------|------|
| **函数视图**（`@api_view`）          | **不支持**                  | 手动查询 | 需要自己写查询逻辑 |
| **APIView**（基础类视图）            | **不支持**                  | 手动查询   | 需要手动写 `Model.objects.all()` |
| **ModelViewSet** 等 | **支持** | 类属性 `queryset = Model.objects.all()`  | 最常用 |

#### `serializer_class` 可以用在哪里？

>**答案**：它主要用于ModelViewSet，并不是所有视图都能直接使用。

支持范围一览表：

| 视图类型                              | 是否支持 `serializer_class` | 使用方式 | 备注 |
|--------------------------------------|-----------------------------|----------|------|
| **函数视图**（`@api_view`）          | **不支持**                  | 手动创建 | 需要自己写 `Serializer(...)` |
| **APIView**（基础类视图）            | **不支持**                  | 手动创建 | 需要手动实例化 serializer |
| **ModelViewSet** 等 | **支持** | 类属性 `serializer_class = YourSerializer`  | 最常用 |

---

---

### 3. 配合路由使用
使用 `ModelViewSet` 后，你不需要在 `urls.py` 里手动写每一个 `path`，配合 `DefaultRouter` 即可自动生成列表、详情、创建、更新、删除等标准路由。

```python
from rest_framework.routers import DefaultRouter
from .views import FlightViewSet

router = DefaultRouter() # 实例化一个 DefaultRouter
# 将ModelViewSet注册进去
router.register(r'flights', FlightViewSet) 
# 自动生成上述 5 种路由：
# GET    /flights/              → list()      # 获取所有航班
# POST   /flights/              → create()    # 创建新航班
# GET    /flights/{id}/         → retrieve()  # 获取单个航班
# PUT    /flights/{id}/         → update()    # 更新航班
# DELETE /flights/{id}/         → destroy()   # 删除航班

urlpatterns = router.urls
```

---

**路由的基础写法（项目中只有 ViewSet 路由时）：**

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import YourViewSet

router = DefaultRouter()
router.register(r'your-resource', YourViewSet, basename='your-resource')

urlpatterns = [
    path('', include(router.urls)),     # 推荐写法
]
```

**当项目中还有其他自定义路由时（最常见的情况）：**

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import YourViewSet

router = DefaultRouter()
router.register(r'your-resource', YourViewSet, basename='your-resource')

urlpatterns = [
    # 其他自定义路由写在这里
    path('some-page/', views.some_view, name='some-page'),
    path('auth/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    
    # ... 更多自定义接口 ...

    # ViewSet 生成的所有路由必须加入
    path('', include(router.urls)),        # 推荐使用 include
    # 或者使用列表解包写法：
    # *router.urls,
]
```

#### 重要说明：

- `router.urls` 是一个普通的 Python 列表，里面存放 DRF 自动生成的多条 URL 规则。
- **不能直接写成** `urlpatterns = router.urls`，否则项目中其他自定义路由会全部丢失。
- 推荐优先使用 `path('', include(router.urls))`，优点包括：
  - 代码结构更清晰易读
  - 方便后续给所有 ViewSet 接口统一添加前缀（如 `path('api/v1/', include(router.urls))`）
  - 更符合官方文档和大多数实际项目的习惯

- 注册 ViewSet 时，**强烈建议加上 `basename`** 参数，可避免潜在的警告或错误。

---

## `DefaultRouter`

在 Django REST Framework (DRF) 中，`DefaultRouter` 是实现 **“自动化路由”** 的终极武器。

如果你使用 `ModelViewSet`（正如你之前代码中展示的那样），手动为 `list`、`create`、`retrieve` 等操作一个一个写 `path` 会非常低效。`DefaultRouter` 的作用就是：**扫描你的 ViewSet，自动生成全套符合 RESTful 规范的 URL。**

以下通过一个**“星际港口管理系统 (StarPort)”**为例进行介绍：

---

### 1. 基础用法：一键生成 CRUD 路由

假设你已经定义好了 `PortViewSet`。

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import PortViewSet, ShipViewSet

# 1. 实例化路由对象
router = DefaultRouter()

# 2. 注册 ViewSet
# 参数1: 路径前缀 (URL prefix)
# 参数2: 视图集类 (ViewSet class)
router.register(r'ports', PortViewSet, basename='port')
router.register(r'ships', ShipViewSet, basename='ship')

# 3. 将生成的路由包含进 urlpatterns
urlpatterns = [
    path('api/', include(router.urls)),
]
```

### 2. 它为你自动生成了什么？
仅仅通过上面的 `router.register`，`DefaultRouter` 就会自动创建以下路由：

| HTTP 方法 | URL 路径 | 对应 ViewSet 动作 | 说明 |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/ports/` | `list` | 获取列表 |
| `POST` | `/api/ports/` | `create` | 创建新港口 |
| `GET` | `/api/ports/{pk}/` | `retrieve` | 获取特定详情 |
| `PUT` | `/api/ports/{pk}/` | `update` | 完整更新 |
| `PATCH` | `/api/ports/{pk}/` | `partial_update` | 部分更新 |
| `DELETE` | `/api/ports/{pk}/` | `destroy` | 删除 |


---

### 3. 为什么要用 `basename`？
在 `register` 时，`basename` 参数用于生成 **URL Name**。
* 如果不写，它默认使用 `queryset` 里的模型名（小写）。
* 建议手动写上，这样你在代码里反向解析 URL 时非常清晰。



## `action`：ModelViewSet 的扩展功能

`ModelViewSet` 默认只提供 CRUD（增删改查）操作。如果你想在同一个 ViewSet 里增加像“批量审批”、“一键锁定”等自定义功能，**就必须用** `@action`。



### 1. 初识 `action`
假设我们在 `ShipViewSet` 中增加一个“紧急避险 (evade)”的指令。

```python
from rest_framework import viewsets
from rest_framework.decorators import action
from rest_framework.response import Response
from .models import SpaceShip
from .serializers import ShipSerializer

class ShipViewSet(viewsets.ModelViewSet):
    queryset = SpaceShip.objects.all()
    serializer_class = ShipSerializer

    # 情况1：detail=True → id夹在中间
    # detail=True 表示该操作针对单条数据（需要 ID），URL 为 /ships/{id}/emergency-evade/
    @action(detail=True, methods=['post'], url_path='emergency-evade')
    def evade(self, request, pk=None):
        ship = self.get_object()  # 等价于手动写：ship = SpaceShip.objects.get(pk=pk)
        # 执行避险逻辑...
        return Response({"status": f"Ship {ship.id} is performing evasive maneuvers!"})

    # 情况2：detail=False → id不在URL中
    # detail=False 表示该操作针对整个列表，URL 为 /ships/summary/
    @action(detail=False, methods=['get'])
    def summary(self, request):
        count = self.queryset.count()
        return Response({"total_ships": count})
```

### 2. `action` 参数讲解：
| 参数 | 说明 |
| :--- | :--- |
| **`detail`** | **核心参数**。`True` 针对特定对象（路径含 ID）；`False` 针对集合（路径不含 ID）。 |
| **`methods`** | 允许的方法，如 `['post']` 或 `['get', 'delete']`。 |
| **`url_path`** | 自定义 URL。如果不写，URL默认使用函数名（如 `evade`）。 |
| **`permission_classes`** | 可以为这个特定的 action 单独设置权限，覆盖类级别的配置。 |

---


### 3. self.action 详解

#### 3.1 DRF 内置的标准 Actions

`ModelViewSet` 默认就自带了 6 个标准 `CRUD action`，它们对应着 **RESTful API** 的基本操作：
```python
from rest_framework import viewsets
from .models import Book
from .serializers import BookSerializer

class BookViewSet(viewsets.ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    
    # 注意：你不需要自己写下面这些方法！
    # DRF 的 ModelViewSet 已经内置实现了：
    # 1. list()     - 获取所有书籍
    # 2. create()   - 创建新书
    # 3. retrieve() - 获取单本书详情
    # 4. update()   - 更新整本书
    # 5. partial_update() - 部分更新书
    # 6. destroy()  - 删除书
    
    def get_queryset(self):
        """在任何地方都可以用 self.action 判断当前是哪个内置 action"""
        if self.action == 'list':
            print("正在执行列表查询...")
            return Book.objects.filter(is_published=True)
        elif self.action == 'retrieve':
            print("正在获取单个对象...")
            return Book.objects.all()  # 详情页不过滤
        return super().get_queryset()
```

这些内置 `action` 的路由是自动生成的：
```
GET     /books/              → list (列表)
POST    /books/              → create (创建)
GET     /books/{id}/         → retrieve (详情)
PUT     /books/{id}/         → update (全量更新)
PATCH   /books/{id}/         → partial_update (部分更新)
DELETE  /books/{id}/         → destroy (删除)
```

#### 3.2 自定义 Action 的路由规则

当内置的 6 个 action 不够用时，比如需要"批量操作"、"状态变更"等业务特定功能，就可以用 `@action` 装饰器添加自定义 action：

```python
# 📌 detail=True 表示针对单个对象
# URL 模式：/resource/{id}/action-name/
@action(detail=True, methods=['post'])
def activate(self, request, pk=None):
    """激活用户"""
    # 对应路由：POST /users/{id}/activate/
    pass

# 📌 detail=False 表示针对整个列表
# URL 模式：/resource/action-name/
@action(detail=False, methods=['get'])
def search(self, request):
    """搜索用户"""
    # 对应路由：GET /users/search/
    pass
```

#### 3.3 完整的路由对照表

基于上面的 BookViewSet，看看完整的路由结构：

```
# 📋 内置的标准路由（自动生成）：
GET     /books/                    → list
POST    /books/                    → create
GET     /books/{id}/               → retrieve
PUT     /books/{id}/               → update
PATCH   /books/{id}/               → partial_update
DELETE  /books/{id}/               → destroy

# 📋 自定义的业务路由：
POST    /users/{id}/activate/        → activate (自定义)
GET     /users/search/       → search (自定义)
```

#### 3.4 实际应用示例
```python
class ExampleViewSet(ModelViewSet):

    def do_something(self):
        if self.action == 'search':
            return Response("这是一个搜索动作。")
        return Response("搞事情!")
```

#### 3.5 总结

1. DRF 先提供 6 个内置的 **CRUD action**，对应 RESTful 标准操作
2. 当业务需要额外功能时，用 `@action` 装饰器添加自定义 `action`
3. 路由自动生成规则：
   • `detail=True` → `/{resource}/{id}/{action}/`

   • `detail=False` → `/{resource}/{action}/`

4. 可以用 `url_path` 自定义 `URL` 路径
5. 无论是内置还是自定义 `action`，都可以通过 `self.action` 识别当前动作
6. 可以在各种方法中使用 `self.action`：
   • `get_queryset()` - 根据 action 返回不同的查询集

   • `get_serializer_class()` - 根据 action 使用不同的序列化器

   • `get_permissions()` - 根据 action 设置不同的权限

   • 任何业务逻辑中
7. `self.action` 永远等于函数名，无论是否设置 `url_path`，换句话说，在代码中判断 `action` 时，永远用函数名
8. `url_path` 只影响 URL 路径，不影响 `action` 名称


## `Response`

在 Django REST Framework (DRF) 中，`Response` 是一个**内容协商（Content Negotiation）**响应类。它不同于 Django 原生的 `HttpResponse`，因为它不需要你手动将数据渲染成特定的格式（如 JSON 或 HTML）。

`Response` 会根据客户端的请求头（如 `Accept: application/json`）自动决定返回哪种格式的数据。

---

### 1. 基础语法
`Response(data, status=None, template_name=None, headers=None, content_type=None)`

* **`data`**: 序列化后的数据（通常是 Python 字典、列表或字符串）。
* **`status`**: HTTP 状态码（建议配合 `rest_framework.status` 使用）。
* **`headers`**: 响应头字典。

---

### 2. 基础用法：返回 JSON 数据
假设我们正在开发一个**“深海勘测系统 (DeepSeaSurvey)”**，我们需要返回潜水器的状态。

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class SubmarineStatusView(APIView):
    def get(self, request):
        # 准备数据（通常来自序列化器，这里用普通字典演示）
        data = {
            "device_name": "Deep-Blue-01",
            "current_depth": 3500,
            "status": "Scanning",
            "battery": "85%"
        }
        
        # 自动将字典转换为客户端要求的格式（默认 JSON）
        # status.HTTP_200_OK 增加了代码的可读性
        return Response(data, status=status.HTTP_200_OK)
```

---

### 3. 结合状态码：处理错误
当操作失败时，我们会返回特定的错误信息和状态码。

```python
    def post(self, request):
        pressure = request.data.get("pressure")
        
        if not pressure:
            # 返回错误信息和 400 状态码
            return Response(
                {"error": "必须提供压力参数"}, 
                status=status.HTTP_400_BAD_REQUEST
            )
            
        return Response({"message": "压力数据已记录"}, status=status.HTTP_201_CREATED)
```

---

### 4. 自定义响应头 (Headers)
有时候你需要返回一些额外的元数据，比如分页信息或自定义标记。

```python
    def get(self, request):
        data = {"result": "Discovery of rare coral"}
        custom_headers = {
            "X-Survey-ID": "EXP-2026-X1",
            "X-Server-Node": "Node-Pacific-04"
        }
        
        return Response(data, headers=custom_headers)
```

---

### 5. 核心优势：为什么不用 `JsonResponse`？



1.  **自动渲染**：如果你在浏览器中直接访问该 API，`Response` 会检测到并返回一个漂亮的 **Browsable API (可交互网页)** 界面，方便调试；如果是代码请求，则返回纯 JSON。
2.  **解耦**：视图函数不需要关心数据最后是变成 JSON 还是 XML，逻辑更加纯粹。
3.  **状态码语义化**：配合 `from rest_framework import status`，可以让你的代码更像“行业标准”，而不是到处写 `200`、`404` 之类的硬编码数字。

---

### 避坑指南
* **Data 必须是可序列化的**：你不能直接把一个 Django Model 对象（比如 `Submarine.objects.get(id=1)`）扔进 `Response`。你必须先通过 **Serializer** 将其转换为 Python 字典，或者手动构造字典。
* **只能在 DRF 视图中使用**：`Response` 类必须在继承了 `APIView` 、`ModelViewSet` 的类视图中或使用了 `@api_view` 装饰器的函数视图中使用，否则它无法正常处理内容。


## `permission_classes`

在 Django REST Framework (DRF) 中，权限控制是保护 API 的核心。你提到的这三个组件构成了权限系统的基础：`AllowAny` 是预设方案，`BasePermission` 是自定义工具，而 `SAFE_METHODS` 是逻辑判断的标尺。

为了保持机密性，我们以一个 **“星际遗产博物馆 (RelicMuseum)”** 系统为例进行讲解。

---


### 1. DRF 内置权限组件概览

| 权限 | 作用 |
| :--- | :--- |
| **`AllowAny`** | **全开放**。无论是否登录，任何人都可以访问。 |
| **`BasePermission`** | **自定义基类**。当你需要编写特殊的权限逻辑（如“只有馆长能修改”）时需要继承它。 |
| **`SAFE_METHODS`** | **只读方法元组**。包含 `('GET', 'HEAD', 'OPTIONS')`。用于区分“查看”和“修改”操作。 |
| **`IsAuthenticated`** | **仅限登录用户**。必须通过身份验证（Token 或 Session）。 | 用户个人中心、收藏夹、发布评论。 |
| **`IsAdminUser`** | **仅限管理员**。要求用户的 `is_staff` 字段为 `True`。 | 系统后台配置、敏感日志查看。 |
| **`IsAuthenticatedOrReadOnly`** | **登录可写，游客只读**。未登录只能 GET，登录后可 POST/PUT。 | 博客评论区、百科条目（允许游客浏览但不能编辑）。 |
| **`DjangoModelPermissions`** | **模型权限绑定**。与 Django Admin 的权限系统联动。 | 复杂的企业内部管理系统，权限细化到增删改查。 |

---

### 2. 基础用法：`AllowAny` 或多种权限
通常用于登录、注册或公开展示的页面。

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import AllowAny

class MuseumAnnouncementView(APIView):
    # 所有人（包括游客）都能看到博物馆公告
    permission_classes = [AllowAny]

    def get(self, request):
        return Response({"message": "欢迎来到星际博物馆，今日免费开放！"})
```


你可以在 `permission_classes` 中放入多个权限类，DRF 会按顺序校验，**全部通过**才允许访问。

```python
from rest_framework import viewsets
from rest_framework.permissions import IsAuthenticated
from .permissions import IsOwnerOrReadOnly

class ArticleViewSet(viewsets.ModelViewSet):
    queryset = Article.objects.all()
    
    # 组合技：必须先登录 (IsAuthenticated)，且必须是作者 (IsOwnerOrReadOnly)
    permission_classes = [IsAuthenticated, IsOwnerOrReadOnly]
```

---

### 3. 进阶用法：`BasePermission` 与 `SAFE_METHODS`
这是开发中最常见的场景：**准许所有人查看（只读），但只有作者或管理员可以修改（写入）。**



#### 自定义权限类
我们需要继承 `BasePermission` 并重写 `has_permission` 方法或 `has_object_permission` 方法。

```python
from rest_framework.permissions import BasePermission, SAFE_METHODS

class MyPermission(BasePermission):
    """
    已认证普通用户只可读，管理员可以增删改，未认证用户无权限。
    """

    def has_permission(self, request, view):
        # 未认证用户直接拒绝
        if not request.user or not request.user.is_authenticated:
            return False

        # 安全方法(GET/HEAD/OPTIONS) → 已认证用户可以读
        if request.method in SAFE_METHODS:
            return True

        # 非安全方法 → 只有管理员可以操作
        return request.user.is_staff

class IsCuratorOrReadOnly(BasePermission):
    """
    自定义权限：如果是只读请求则通过；如果是修改请求，则必须是‘馆长’身份。
    """
    def has_object_permission(self, request, view, obj):
        # 1. 检查是否为“安全方法” (GET, HEAD, OPTIONS)
        if request.method in SAFE_METHODS:
            return True

        # 2. 如果是 POST/PUT/DELETE，检查用户是否为馆长 (假设模型有 is_curator 字段)
        return bool(request.user and request.user.is_authenticated and request.user.is_staff)
```

#### 在视图中应用
```python
from rest_framework import viewsets
from .models import Relic
from .serializers import RelicSerializer

class RelicViewSet(viewsets.ModelViewSet):
    queryset = Relic.objects.all()
    serializer_class = RelicSerializer
    
    # 使用刚才自定义的权限
    permission_classes = [IsCuratorOrReadOnly] # 也可以是 MyPermission
```

---

### 4. 核心逻辑详解

#### 为什么使用 `SAFE_METHODS`？
在 HTTP 协议中，`GET` 被认为是“安全”的，因为它不应该改变服务器的数据状态。通过 `request.method in SAFE_METHODS`，你可以快速放行所有的查询请求，而把权限校验集中在破坏性的 `POST`、`PUT`、`DELETE` 上。

#### `has_permission` vs `has_object_permission`
* **`has_permission`**: 进入视图前的第一道关卡（比如：是否登录？）。
* **`has_object_permission`**: 针对具体某条数据的检查（比如：这条遗产文物是不是你捐赠的？）。

---

### 5. 组合多个权限
DRF 支持在 `permission_classes` 列表中放入多个类，系统会按顺序检查，**全部通过**才允许访问（区别于 `authentication_classes` 的 **“第一个成功即生效”**）。

```python
from rest_framework.permissions import IsAuthenticated

class SecretArchiveView(APIView):
    # 必须既是登录用户，又是馆长身份
    permission_classes = [IsAuthenticated, IsCuratorOrReadOnly]
```

### 6. `permission_classes` 可以用在哪里？

| 视图类型                          | 使用方式 |
|----------------------------------|----------|
| **函数视图**（`@api_view`）  | `@permission_classes` 装饰器 |
| **APIView**（基础类视图）  | 类属性 `permission_classes = [...]`  |
| **ModelViewSet** 等 | 类属性 `permission_classes = [...]` |




### 总结建议
1.  **最小权限原则**：除非明确需要公开，否则默认应使用 `IsAuthenticated` 或更严格的权限。
2.  **善用 `SAFE_METHODS`**：它可以让你的权限逻辑非常优雅，一行代码解决“读写分离”的权限需求。



## `RefreshToken`

在 Django REST Framework (DRF) 中，`RefreshToken` 是 `django-rest-framework-simplejwt` 库的核心组件。它主要用于**手动控制令牌（Token）的生成、刷新和销毁**，而不是仅仅依赖库自带的默认视图。

---

### 1. `RefreshToken` 的核心逻辑

JWT 认证通常包含两个令牌：
* **Access Token (访问令牌)**: 寿命短（如 5 分钟），用于每次请求的身份验证。
* **Refresh Token (刷新令牌)**: 寿命长（如 7 天），专门用于在 Access Token 过期后换取新的 Access Token。



---

### 2. 代码用法示例：手动发放 Token

当你自定义登录逻辑（例如你代码中通过 `authenticate` 验证后），需要手动给用户发放“通行证”。

```python
from django.contrib.auth.models import User
from rest_framework_simplejwt.tokens import RefreshToken

def get_tokens_for_user(user):
    # 1. 为指定用户实例化一个 RefreshToken 对象
    refresh = RefreshToken.for_user(user)

    # 2. 返回包含 refresh 和 access 字符串的字典
    # str() 转换是必须的，因为对象本身是类实例，前端需要的是字符串
    return {
        'refresh': str(refresh),
        'access': str(refresh.access_token),
    }
```
---

### 3. 客户端怎样用 Token

1. **用户登录成功**后，服务器返回：
   ```json
   {
     "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTc0Mzk5OTk5OSwiaWF0IjoxNzQzOTEzNTk5LCJqdGkiOiJhYmNkMTIzNCIsInVzZXJfaWQiOjF9.abc123def456...",
     "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzQzOTEzODk5LCJpYXQiOjE3NDM5MTM1OTksImp0aSI6IjU2N2U4ZjkiLCJ1c2VyX2lkIjoxfQ.xyz789..."
   }
   ```

2. **客户端保存**：
   - 把 `access` 存到内存（或持久化保存，根据实际安全需求）
   - 把 `refresh` 持久化保存（localStorage / cookie 等）

3. **后续每次请求后端接口时**（在请求头里写入）：
   ```http
   Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...   ← ⚠️这是 Access Token，而不是 Refresh Token
   ```

4. **当 Access Token 过期**（返回 401）时：
   - 方法A ：客户端自动调用刷新接口（可选）
     ```http
     POST /api/token/refresh/
     Content-Type: application/json

     {
       "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."   ← ⚠️只带 Refresh Token
     }
     ```
   - 方法B：客户端需要重新登录（自己写逻辑）

---

### 4. 在 `settings.py` 中的关键配置

使用 `RefreshToken` 时，你必须在 `settings.py` 中定义它的行为（有效期、加密方式等）。如果不配置，它将使用默认的短时效设置。

```python
from datetime import timedelta

SIMPLE_JWT = {
    # 1. 访问令牌有效期（建议设置较短，如 30 分钟到 2 小时）
    
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=60),  # 如果觉得老是刷新令牌很麻烦，且安全性要求不高的话，时间可以设置得长一些，比如我就设置的7天
    
    # 2. 刷新令牌有效期（建议设置较长，如 7 天到 30 天）
    'REFRESH_TOKEN_LIFETIME': timedelta(days=7),
    
    # 3. 是否在刷新后更换新的刷新令牌（防止令牌被盗长期使用）
    'ROTATE_REFRESH_TOKENS': True,
    
    # 4. 是否将旧的刷新令牌加入黑名单（需要安装 blacklist app）
    'BLACKLIST_AFTER_ROTATION': True,

    # 5. 签名算法
    'ALGORITHM': 'HS256',
    'SIGNING_KEY': '你的密钥', # 默认使用 Django 的 SECRET_KEY
    
    # 6. 认证头部关键词，默认是 'Bearer'
    'AUTH_HEADER_TYPES': ('Bearer',),
}
```

---

### 5. 进阶：在令牌中注入自定义信息 (Payload)

如果你希望前端解密 Token 后能直接看到用户的角色或等级，可以手动添加声明（Claims）。

```python
def get_custom_tokens(user):
    refresh = RefreshToken.for_user(user)
    
    # 注入自定义载荷（注意：不要放敏感密码！）
    refresh['user_role'] = 'Senior_Explorer'
    refresh['is_vip'] = True
    
    return {
        'refresh': str(refresh),
        'access': str(refresh.access_token),
    }
```

---

### 6. 你的代码上下文分析

在你提供的代码中，你将 `access_token` 存入了 `DeviceToken` 模型：
```python
device_token.token = access_token
device_token.save()
```
**这种做法的妙处在于**：配合你的自定义认证类（`MyJWTAuthentication`），你可以在用户每次请求时，去数据库校验当前的 Token 是否与 `MyToken` 表中存储的一致。如果不一致（说明该平台有新登录），则判定旧 Token 失效，从而实现**单设备登录限制**。

### 总结
* **`Access Token`** → 日常“通行证”（每次请求都要带）
* **`Refresh Token`** → “令牌生产机”（只用来换新 Access Token）。
* **`settings.py`** 是“质检标准”，负责规定令牌能用多久、怎么加密。
* **手动操作** 赋予了你控制权，让你能实现类似“强制下线”或“多平台独立登录”的复杂业务逻辑。

## `JWTAuthentication`

在 Django REST Framework (DRF) 中，`JWTAuthentication` 是处理 **JSON Web Token** 的核心后端。它的主要任务是：**检查请求头中的 Token 是否合法，如果合法，则自动将对应的用户对象绑定到 `request.user` 上。**

为了保护你的项目机密，我们以一个**“星际贸易联盟 (TradeFederation)”**系统为例。

---

### 1. `JWTAuthentication` 的工作原理

当客户端向服务端发送请求时，它通常会在 HTTP Header 中携带如下信息：
`Authorization: Bearer <你的Token字符串>`

>⚠️请注意⚠️ 这里的 Token 字符串是 Access Token ，而不是 Refresh Token！

`JWTAuthentication` 会执行以下操作：
1.  截取 `Bearer` 后面的字符串。
2.  使用 `settings.py` 中定义的密钥进行解密。
3.  验证 Token 是否过期或被篡改。
4.  从 Token 的载荷（Payload）中获取 `user_id`，并从数据库中查询出该用户。



---

### 2. 代码用法示例

#### 场景 A：在单个视图中启用
如果你只想让某个特定的接口（例如“查看账户余额”）支持 JWT 认证：

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
from rest_framework_simplejwt.authentication import JWTAuthentication

class AccountBalanceView(APIView):
    # 1. 声明认证后端
    authentication_classes = [JWTAuthentication]
    
    # 2. 声明权限：必须是已认证用户
    permission_classes = [IsAuthenticated]

    def get(self, request):
        # 此时 request.user 已经是被 JWT 识别出来的用户对象了
        content = {'message': f'你好 {request.user.username}, 你的余额为 5000 星币'}
        return Response(content)
```

#### 场景 B：在 ViewSet 中启用
这和你代码中的逻辑类似，通常配合权限类一起使用。

```python
from rest_framework.viewsets import ModelViewSet

class StarshipViewSet(ModelViewSet):
    queryset = Starship.objects.all()
    serializer_class = StarshipSerializer
    
    # 只有携带有效 JWT Token 的人才能访问飞船列表
    authentication_classes = [JWTAuthentication]
    permission_classes = [IsAuthenticated]
```

---

### 3. 全局配置（推荐做法，可选）

如果你希望项目中的绝大多数接口都默认使用 JWT，你可以在 `settings.py` 中进行全局声明。这样你就不需要在每个 View 里手动写 `authentication_classes` 了。

```python
# settings.py

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}
```

---

### 4. 自定义认证类型

在你的代码中，你并没有直接在 ViewSet 里使用原生的 `JWTAuthentication`，而是使用了：
`authentication_classes = [MyJWTAuthentication]`

```python
"""
最简单的自定义JWT认证示例
原理：继承并重写父类的authenticate方法
"""

from rest_framework_simplejwt.authentication import JWTAuthentication
from rest_framework.exceptions import AuthenticationFailed

# 最简单的自定义认证
class MyJWTAuthentication(JWTAuthentication):
    """
    重写authenticate方法，添加额外检查
    """
    def authenticate(self, request):
        # 1. 先让父类验证JWT token
        result = super().authenticate(request)  # 返回 (user, token) 或 None
        
        if result is None:
            return None  # 没有token，让权限类处理
            
        # 2. 获取验证后的用户
        user, token = result
        
        # 3. 添加你的额外检查（这里只是示例）
        # 例如，检查是否已设置邮箱
        if not user.email:  
            raise AuthenticationFailed('请先设置邮箱') # 认证不通过，登录失败
        
        # 4. 返回认证结果
        return (user, token)


# 使用示例
from rest_framework.views import APIView
from rest_framework.permissions import IsAuthenticated
from .authentication import MyJWTAuthentication

class MyView(APIView):
    # 关键：用你的自定义认证替换默认的
    authentication_classes = [MyJWTAuthentication]
    permission_classes = [IsAuthenticated]  # 需要认证
    
    def get(self, request):
        return {"message": "认证通过"}


```
#### `authentication_classes` 可以用在哪里？

| 视图类型                        |  使用方式 |
|--------------------------------|----------|
| **函数视图**（`@api_view`）    | `@authentication_classes` 装饰器 |
| **APIView**（类视图）   |  类属性 `authentication_classes = [...]` |
| **ModelViewSet** 等 | 类属性 `authentication_classes = [...]` |


---

#### 实用小贴士

1. **认证 ≠ 权限**  
   `authentication_classes` 只负责“识别用户是谁”（填充 `request.user` 和 `request.auth`）。  
   要控制“谁能访问”，还需要配合 `permission_classes`。

2. 如果你不想使用任何认证，可以这样关闭：
   ```python
   authentication_classes = []   # 空列表表示禁用认证
   ```

3. 多个认证类会按顺序尝试，第一个认证成功的就会生效，后面的认证类不会再继续尝试。

---

### 5. 常见问题排查
* **401 Unauthorized**：通常是忘了带 `Authorization` 请求头，或者关键词写错（比如写成了 `Token` 而不是 `Bearer`）。
* **403 Forbidden**：认证成功了，但是 `permission_classes`（权限类）校验没通过。
* **User 找不到**：Token 是真的，但 Token 里的用户在数据库中被删除了。

## `JSONParser`, `MultiPartParser`

在 Django REST Framework (DRF) 中，**Parsers (解析器)** 的作用是：当客户端发送请求时，解析器决定了如何处理请求体（Request Body）中的数据，并将其转换为 `request.data` 供你使用。

### 1. DRF 默认的解析器

DRF 默认配置（不写任何 parser 设置时）是：

```python
# setting.py
DEFAULT_PARSER_CLASSES = [
    'rest_framework.parsers.JSONParser',
    'rest_framework.parsers.FormParser',        # 处理普通 form 表单
    'rest_framework.parsers.MultiPartParser',   # 处理文件上传（multipart/form-data）
]
```

这意味着你的 API **同时接受 JSON、form-urlencoded 和文件上传** 三种格式。

>**如果是前后端分离的项目**：**有必要**在 `settings.py` 中显式设置 `DEFAULT_PARSER_CLASSES`，只保留 `JSONParser`（需要上传文件时再加上 `MultiPartParser`）。

>移除 `FormParser` 可以防止客户端用 `application/x-www-form-urlencoded` 发送数据，减少某些攻击向量（尤其是和 Mass Assignment 结合时）。

---

### 2. 两种解析器的核心区别

| 解析器 | 数据类型 (Content-Type) | 适用场景 |
| :--- | :--- | :--- |
| **`JSONParser`** | `application/json` | **标准 API 请求**。处理纯文本/JSON 数据。 |
| **`MultiPartParser`** | `multipart/form-data` | **文件上传**。支持表单字段和二进制文件。 |

---

### 3. 代码用法示例：混合数据处理

在实际开发中，我们通常将这两者结合使用，这样你的接口既能接收普通的 JSON，也能接收用户通过表单上传的照片。

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.parsers import MultiPartParser, JSONParser
from rest_framework import status

class LogUploadView(APIView):
    # 1. 声明该视图支持的解析器
    # 顺序不重要，DRF 会根据请求头的 Content-Type 自动匹配
    parser_classes = [MultiPartParser, JSONParser]

    def post(self, request, format=None):
        # 如果是 JSON 请求，这里拿到的是字典
        # 如果是 MultiPart 请求，这里能拿到表单字段 + 文件对象
        title = request.data.get('title')
        
        # 获取上传的文件 (针对 MultiPartParser)
        photo = request.FILES.get('photo') 
        
        if photo:
            # 执行保存文件或上传到云存储的逻辑
            return Response({
                "message": f"日志 '{title}' 及照片已接收",
                "file_name": photo.name
            }, status=status.HTTP_201_CREATED)
            
        return Response({"message": "仅接收到文字数据"}, status=status.HTTP_200_OK)
```

---

### 4. 数据解析器的用法

如果你使用的是 `ModelViewSet`，配置方式是一样的：

```python
from rest_framework.viewsets import ModelViewSet
from rest_framework.parsers import MultiPartParser, JSONParser

class ExpeditionViewSet(ModelViewSet):
    queryset = Expedition.objects.all()
    serializer_class = ExpeditionSerializer
    
    # 允许该接口处理 JSON、文件上传
    parser_classes = [JSONParser, MultiPartParser]
```

#### `parser_classes` 可以用在哪里？

| 视图类型                  | 使用方式 |
|---------------------------|----------|
| **函数视图**（`@api_view`） |使用 `@parser_classes` 装饰器 |
| **APIView**（类视图）     | 类属性 `parser_classes = [...]` |
| **ModelViewSet** 等 | 类属性 `parser_classes = [...]` |


---

### 总结
* **只传文字？** 客户端用 `JSONParser` 效率最高。
* **要传图片/文件？** 必须用 `MultiPartParser`。
* **你的代码逻辑**：通过在 `ViewSet` 中同时引入这两者，你构建了一个极其健壮的接口，能够“通吃”几乎所有主流的客户端请求格式。

## `NotFound`, `APIException`

在 Django REST Framework (DRF) 中，**Exceptions（异常）** 是处理错误的优雅方式。当程序运行遇到问题（如找不到数据、权限不足或逻辑错误）时，抛出异常比手动返回一个 `Response` 更符合框架的设计哲学，因为 DRF 会捕获这些异常并自动将其转换为标准化的 JSON 响应。

为了保护机密，我们以一个**“星际航行导航系统 (GalaxyNav)”**为例进行介绍。

---

### 1. `NotFound`：专用于“资源不存在”
`NotFound` 是 DRF 预设的异常类，对应 HTTP 404 状态码。当你无法根据用户提供的 ID 找到对应的行星或坐标时，使用它。

#### 代码示例
```python
from rest_framework.views import APIView
from rest_framework.exceptions import NotFound
from .models import Planet

class PlanetDetailView(APIView):
    def get(self, request, planet_id):
        try:
            planet = Planet.objects.get(id=planet_id)
        except Planet.DoesNotExist:
            # 抛出 NotFound 异常
            # DRF 会自动返回 {"detail": "未找到该星体。"}，状态码 404
            raise NotFound(detail="未找到该星体，可能已被黑洞吞噬。")
        
        return Response({"name": planet.name})
```

---

### 2. `APIException`：自定义异常的基类
如果你遇到了特定的业务逻辑错误（例如：燃料不足、飞船处于锁定状态），而 DRF 预设的错误类（如 `ValidationError`）都不合适，你可以通过继承 `APIException` 来创建自己的异常。



#### 代码示例：自定义业务异常
```python
from rest_framework.exceptions import APIException
from rest_framework import status

class FuelExhaustedException(APIException):
    # 默认状态码（例如 400 错误请求）
    status_code = status.HTTP_400_BAD_REQUEST
    # 默认错误详情
    default_detail = '燃料已耗尽，无法执行跃迁指令。'
    # 默认错误代码（方便前端做逻辑判断）
    default_code = 'fuel_error'

# 在视图中使用
class WarpDriveView(APIView):
    def post(self, request):
        fuel_level = request.data.get("fuel")
        if fuel_level < 10:
            # 抛出自定义异常
            raise FuelExhaustedException()
        return Response({"message": "跃迁成功！"})
```

---

### 3. 为什么要用 `raise` 异常而不是 `return Response`？

1.  **代码更整洁**：你可以在 Service 层或模型层抛出异常，而不需要层层传递 `if/else` 逻辑到 View 层。
2.  **全局统一格式**：DRF 的异常处理器会确保所有错误返回的 JSON 结构是一致的（通常包含一个 `detail` 键）。
3.  **自动终止**：执行 `raise` 后，当前函数会立即停止，不会执行后面的逻辑，有效防止由于数据缺失导致的程序崩溃。

---

### 4. 你的代码上下文分析

在你的代码中，`NotFound` 的用法通常出现在需要精确匹配对象的场景：
```python
# 逻辑示意（非泄密版本）
try:
    obj = MyModel.objects.get(pk=pk)
except MyModel.DoesNotExist:
    raise NotFound()
```
这比返回一个自定义的 `{"error": "not found"}` 响应要专业得多，因为它能让 DRF 的中间件识别这是一个 404 错误。

### 5. 高级小贴士：动态修改详情
即使是预设的异常，你也可以在抛出时动态传入信息：

```python
if user_input == "forbidden_zone":
    # 临时覆盖默认的 status_code 或 detail
    exc = APIException("禁飞区，请立即掉头！")
    exc.status_code = 403 
    raise exc
```

你是否遇到过需要针对特定错误代码（如错误码 10001）返回给前端的场景？如果有，我们可以深入聊聊如何自定义异常的 `data` 结构。

## `status`

在 Django REST Framework (DRF) 中，`from rest_framework import status` 是一个非常实用的模块。它预定义了一系列 **HTTP 状态码常量**。

虽然你可以直接在代码里写数字（如 `200`, `404`），但使用 `status` 常量的专业度更高，且具备极强的**可读性**和**规范性**。

---

### 1. 为什么推荐使用 `status` 而不是数字？

* **可读性**：`HTTP_400_BAD_REQUEST` 比 `400` 更能一眼看出代表“参数错误”。
* **语义化**：遵循 RESTful 规范，让你的代码像一份自解释的文档。
* **避免失误**：手动输入数字容易写错（例如把 `201` 写成 `210`），而使用常量会有 IDE 的代码补全提醒。

---

### 2. 常用状态码分类与示例

为了保护你的代码隐私，我们以一个**“银河航线管理系统 (GalaxyAirways)”**为例：

#### 2.1 成功类 (2xx)
用于表示请求已成功处理。

```python
from rest_framework import status
from rest_framework.response import Response

# 200 OK: 通用成功
return Response({"message": "航线数据查询成功"}, status=status.HTTP_200_OK)

# 201 CREATED: 成功创建了资源（如新增了一条航线）
return Response({"message": "新航线已备案"}, status=status.HTTP_201_CREATED)

# 204 NO CONTENT: 成功删除，且不需要返回任何内容
return Response(status=status.HTTP_204_NO_CONTENT)
```

#### 2.2 客户端错误类 (4xx)
用于表示前端传来的请求有问题。

```python
# 400 BAD REQUEST: 参数校验失败
return Response({"error": "航向坐标格式不正确"}, status=status.HTTP_400_BAD_REQUEST)

# 401 UNAUTHORIZED: 未登录或 Token 无效
return Response({"error": "请先出示飞行执照"}, status=status.HTTP_401_UNAUTHORIZED)

# 403 FORBIDDEN: 已登录但权限不足
return Response({"error": "你没有权限取消该航班"}, status=status.HTTP_403_FORBIDDEN)

# 404 NOT_FOUND: 资源不存在
return Response({"error": "未找到指定的星门"}, status=status.HTTP_404_NOT_FOUND)
```

#### 2.3 服务器错误类 (5xx)
用于表示后端程序崩溃或第三方服务（如 COS 上传）异常。

```python
# 500 INTERNAL SERVER ERROR: 后端逻辑抛出未捕获的异常
return Response({"error": "跳跃引擎发生未知错误"}, status=status.HTTP_500_INTERNAL_SERVER_ERROR)
```

---

### 3. 在你的代码上下文中的实际应用

在你提供的原始代码中，`status` 的应用非常精准，这体现了良好的编码习惯：

1.  **公告板接口**：如果没有公告，返回 `status.HTTP_404_NOT_FOUND`，这比返回一个空列表更符合“找不到资源”的语义。
2.  **登录逻辑**：当用户名密码错误时，返回 `status.HTTP_401_UNAUTHORIZED`。这告诉前端：认证失败，请重新登录。
3.  **批量删除**：操作成功后返回 `status.HTTP_200_OK`，并在 Body 中带上删除的数量，这是一种非常友好的反馈方式。



---

### 4. 核心常量小抄（DRF status 模块）

在使用 DRF 时，推荐从 `rest_framework import status` 导入这些常量，而不是直接写数字。这样代码可读性更高，也更不容易出错。

#### 成功响应 (2xx)

| 常量名                              | 数字 | 适用场景 |
|------------------------------------|------|----------|
| `HTTP_200_OK`                      | 200  | GET 获取数据成功、PUT/PATCH 更新成功（最常用） |
| `HTTP_201_CREATED`                 | 201  | POST 创建新资源成功（如注册用户、新建记录） |
| `HTTP_202_ACCEPTED`                | 202  | 请求已接受但处理尚未完成（异步任务常用） |
| `HTTP_204_NO_CONTENT`              | 204  | DELETE 删除成功、或不需要返回内容时 |
| `HTTP_205_RESET_CONTENT`           | 205  | 重置客户端表单或视图状态 |

#### 客户端错误 (4xx) —— 最常用的一类

| 常量名                              | 数字 | 适用场景 |
|------------------------------------|------|----------|
| `HTTP_400_BAD_REQUEST`             | 400  | 表单验证失败、参数错误、缺少必填字段 |
| `HTTP_401_UNAUTHORIZED`            | 401  | 未登录、Token 过期或无效 |
| `HTTP_403_FORBIDDEN`               | 403  | 已登录但权限不足（普通用户访问管理员接口） |
| `HTTP_404_NOT_FOUND`               | 404  | 查询的资源 ID 不存在、URL 找不到 |
| `HTTP_405_METHOD_NOT_ALLOWED`      | 405  | 请求方法不支持（如 GET 接口用了 POST） |
| `HTTP_406_NOT_ACCEPTABLE`          | 406  | 客户端要求的返回格式不支持 |
| `HTTP_408_REQUEST_TIMEOUT`         | 408  | 请求超时 |
| `HTTP_409_CONFLICT`                | 409  | 资源冲突（如用户名已存在） |
| `HTTP_415_UNSUPPORTED_MEDIA_TYPE`  | 415  | 上传的文件类型不支持 |

#### 服务器错误 (5xx)

| 常量名                                   | 数字 | 适用场景 |
|-----------------------------------------|------|----------|
| `HTTP_500_INTERNAL_SERVER_ERROR`        | 500  | 服务器内部错误（代码异常、数据库错误等） |
| `HTTP_501_NOT_IMPLEMENTED`              | 501  | 接口尚未实现 |
| `HTTP_502_BAD_GATEWAY`                  | 502  | 网关错误（上游服务问题） |
| `HTTP_503_SERVICE_UNAVAILABLE`          | 503  | 服务暂时不可用（维护、 overload） |
| `HTTP_504_GATEWAY_TIMEOUT`              | 504  | 网关超时 |

#### 使用示例（简单代码片段）

```python
from rest_framework import status
from rest_framework.response import Response

# 成功创建
return Response(serializer.data, status=status.HTTP_201_CREATED)

# 删除成功（不返回内容）
return Response(status=status.HTTP_204_NO_CONTENT)

# 验证失败
return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

# 资源不存在
return Response({"detail": "Not found."}, status=status.HTTP_404_NOT_FOUND)
```

#### 小建议
- **最常用 Top 5**：`200_OK`、`201_CREATED`、`204_NO_CONTENT`、`400_BAD_REQUEST`、`404_NOT_FOUND`。
- 实际开发中，**客户端错误（4xx）** 用得最多，尤其是权限和验证相关的。
- 如果你想看到 DRF 全部状态码，可以在代码中 `from rest_framework import status` 后按住 Ctrl + 点击 `status` 查看完整列表。

**总结：**
`status` 模块是 API 响应的“表情包”，它通过标准化的数字告诉前端：“我收到货了 (201)”、“你给的地址不对 (404)”或是“我不认识你 (401)”。

你在开发中，是否经常需要根据不同的业务逻辑（比如余额不足、账号被封）返回不同的 400 细分错误？如果有需要，我们可以聊聊如何自定义错误代码。


## `serializers`

在 Django REST Framework (DRF) 中，`serializers.ModelSerializer` 是一个极其实用的工具，它能够自动根据你的数据库模型（Model）生成序列化字段，并处理数据的验证（Validation）和保存（Create/Update）。

基于你提供的代码风格，我为你总结了 `ModelSerializer` 的核心用法及进阶技巧。

---


### 1. 基础结构：模型映射
这是最常用的方式，通过 `class Meta` 指定关联的模型和字段。

```python
from rest_framework import serializers

class BaseItemSerializer(serializers.ModelSerializer):
    """
    基础序列化器示例
    """
    class Meta:
        model = YourModel  # 替换为对应的 Model 类
        # 方式 A: 包含所有字段
        fields = "__all__" 
        # 方式 B: 指定特定字段 (推荐，更安全且性能更好)
        # fields = ["id", "title", "content"]
        # 方式 C: 排除特定字段
        # exclude = ["internal_note"]
```

---

### 2. 进阶用法：自定义字段处理
有时候数据库里的原始数据并不能直接满足前端需求，我们需要对字段进行“加工”。

#### A. 关联字段处理 (Related Fields)
当你需要处理外键（ForeignKey）时，常用的几种方式：

```python
class AdvancedSerializer(serializers.ModelSerializer):
    # 技巧 1: PrimaryKeyRelatedField 
    # 前端传 ID，后端自动转为对象模型，常用于写入操作
    category_id = serializers.PrimaryKeyRelatedField(
        queryset=Category.objects.all(), 
        source="category"  # 对应模型中的外键字段名
    )

    # 技巧 2: SlugRelatedField
    # 返回关联对象的某个特定属性（如：返回作者的用户名而不是 ID）
    author_name = serializers.SlugRelatedField(
        slug_field="username",
        read_only=True      # 设置为只读，防止前端修改
    )

    class Meta:
        model = Article
        fields = ["id", "category_id", "author_name", "title"]
```

#### B. 重写 `to_representation` (自定义返回逻辑)
如果你想在数据返回给前端之前，动态地修改某些字段的值（例如你代码中的图片 URL 加版本号），这是最灵活的方法。



```python
class DynamicSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = "__all__"

    def to_representation(self, instance):
        """
        instance: 数据库查询出的单条记录对象
        """
        # 获取原始序列化后的字典数据
        ret = super().to_representation(instance)

        # 示例：根据业务逻辑修改返回内容
        # 比如给 URL 增加防缓存的时间戳，或者格式化金额
        if instance.image_url:
            import time
            timestamp = int(time.time())
            ret['image_url'] = f"{instance.image_url}?t={timestamp}"
        
        # 示例：添加一个模型中不存在的动态字段
        ret['custom_tag'] = "Featured" if instance.is_hot else "Normal"

        return ret
```

#### C. 动态选择序列化器
你可以重写 `get_serializer_class` 方法，为 `ModelViewSet` 按条件挑选合适的序列化器

```python
from rest_framework.viewsets import ModelViewSet
from .models import Book
from .serializers import SimpleSerializer, DetailSerializer

class BookViewSet(ModelViewSet):
    queryset = Book.objects.all()
    
    # ✅ 重写get_serializer_class方法
    def get_serializer_class(self):
        if self.action == 'list':  # 列表页
            return SimpleSerializer # 选择 SimpleSerializer 作为序列化器
        return DetailSerializer  # 选择 DetailSerializer 作为序列化器
```

---

### 3. 核心功能总结

| 功能点 | 说明 | 示例场景 |
| :--- | :--- | :--- |
| **自动验证** | 根据 Model 字段定义（如 `max_length`）自动校验数据。 | 用户提交表单时拦截非法输入。 |
| **`source` 参数** | 指定该字段对应模型中的哪个属性，支持跨表取值（如 `user.profile.phone`）。 | 字段重命名或获取关联表的深层属性。 |
| **`read_only`** | 标记字段为只读，序列化时存在，反序列化（创建/修改）时忽略。 | 创建时间、最后更新时间、自动生成的 ID。 |
| **`write_only`** | 标记字段只在创建/更新时使用，不会返回给前端。 | 密码字段、验证码。 |

---

### 4. 最佳实践建议

1.  **明确字段列表**：尽量使用 `fields = [...]` 列表而不是 `"__all__"`。这可以防止你在模型中新增敏感字段（如 `internal_status`）时，无意中将其暴露给 API。
2.  **读写分离**：如果一个页面的查询（List/Retrieve）和提交（Create/Update）字段差异很大，建议定义两个不同的 Serializer。
3.  **性能优化**：在 View 层配合 `select_related` (针对一对一/外键) 或 `prefetch_related` (针对多对多) 使用，避免序列化时产生大量的 N+1 查询。

> **提示**：你的代码中关于 `to_representation` 的处理非常优雅，利用时间戳解决图片缓存问题是一个很实用的技巧。同时，`PrimaryKeyRelatedField` 配合 `source` 参数的使用，也完美解决了“前端传 ID，后端存对象”的转换逻辑。



## 静态文件（Static Files）配置

在开发环境中，如果需要 Django 正确提供静态文件（CSS、JavaScript、图片等），需要在主 `urls.py` 中添加以下配置。

### 1. 正确用法

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('students.urls')),
]

# 开发环境下让 Django 服务静态文件
if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

### 2. 说明：

- `static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)`  
  这个函数会根据 `settings.py` 中的 `STATIC_URL` 和 `STATIC_ROOT` 配置，自动生成静态文件相关的 URL 规则。

- **强烈推荐加上 `if settings.DEBUG:` 判断**  
  只有在 `DEBUG = True`（即开发模式）时才让 Django 处理静态文件。  
  在生产环境（`DEBUG = False`）中，应该使用 Nginx、Apache 或 WhiteNoise 等方式来提供静态文件，而不是让 Django 来处理。

### 3. 为什么需要这样写？

在开发阶段，Django 默认不会自动暴露静态文件目录。通过上面这行代码，你就可以在浏览器中正常访问 `/static/` 开头的文件（例如 CSS、JS、上传的图片等）。

**注意**：
- 这段代码**只适用于开发环境**。
- 生产环境部署时，请移除或通过 `if settings.DEBUG` 条件避免加载。
- 如果你同时需要处理媒体文件（用户上传的文件，如头像、图片），还可以加上媒体文件的配置：

```python
if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

这样就能同时支持静态文件和媒体文件了。

---

### 4. 如果不写 `static(...)` 会怎么样？

如果你没有在 `urls.py` 中添加静态文件配置，那么在**本地开发**时会出现以下问题：

- CSS 样式全部失效（页面变得很丑、没有布局）
- JavaScript 文件加载失败（某些功能无法使用）
- 图片无法显示
- 浏览器控制台（F12）会出现大量 `404 Not Found` 错误，类似：
  ```
  GET /static/css/style.css 404 (Not Found)
  ```

#### 什么情况下需要加这行代码？

**开发环境（本地开发）**：
- 你正在自己的电脑上运行项目
- `settings.py` 中设置了 `DEBUG = True`
- 使用 `python manage.py runserver` 启动项目
- **这时必须加上**静态文件配置，否则上面那些问题就会出现

**生产环境（正式上线）**：
- 项目部署到服务器上（使用 Nginx、Gunicorn、uWSGI 等）
- `settings.py` 中设置了 `DEBUG = False`
- **不需要**用 Django 来提供静态文件（应该由 Nginx 或 WhiteNoise 处理）

---

### 5. 推荐的完整写法（最清晰版本）

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('students.urls')),
]

# ==================== 静态文件配置 ====================
# 只在本地开发时让 Django 提供静态文件
if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

### 6. 操作说明（一步一步）

1. 打开项目的主 `urls.py` 文件（通常是 `项目名/urls.py`）
2. 在文件最上方添加引入：
   ```python
   from django.conf import settings
   from django.conf.urls.static import static
   ```
3. 在 `urlpatterns` 列表的**最后面**，添加上面的 `if settings.DEBUG` 判断代码
4. 保存文件后，重新启动服务器：
   ```bash
   python manage.py runserver
   ```
5. 刷新页面，看看 CSS 和图片是否正常显示

**小贴士**：
- 如果你的项目还有用户上传的图片、头像等文件（媒体文件），可以继续加上：
  ```python
  if settings.DEBUG:
      urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
      urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
  ```

这样写既简单又安全，开发时能正常看到样式和图片，上线时也不会出问题。

