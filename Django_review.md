
## 1. Django项目骨架一览

```
项目根目录 project/
├── manage.py               ✅ 核心脚本！命令入口（migrate、runserver、createsuperuser都靠它）
├── 项目配置包，和根同名 project/
│   ├── settings.py         ✅ 全局配置：INSTALLED_APPS、数据库、时区、静态文件
│   ├── urls.py             ✅ 项目总路由，分发到各个子app的urls
├── 子app/
│   ├── models.py           ✅ 数据表模型
│   ├── admin.py            ✅ 后台注册、Inline
│   ├── views.py            ✅ 视图函数/视图集（DRF的ViewSet写这里）
│   ├── urls.py             ✅ app子路由（⚠️需新建）
│   ├── serializers.py      ✅ DRF序列化器（⚠️需新建）
│   └── permissions.py      ✅ 认证与权限（authentication/authorization，⚠️需新建）
└── media/                  用户上传文件（图片、附件，可选）
```

## 2. settings.py
## 3. models.py
### 3.1 `models.Model` 以及 常用的 `Field`

> ✅ 核心前提：自定义模型**必须继承 `models.Model`**

> 只有继承它，Django 才会识别这是数据库模型，自动生成数据表、提供ORM、支持Admin。不继承，就只是普通Python类，和数据库无关。

```python
from django.db import models  # 从 django.db 导入 models 模块


class Sense(models.Model):
    # 短字符串，必须写 max_length
    word_text = models.CharField(max_length=100, blank=True, null=True)

    # 长文本，不需要 max_length
    explanation = models.TextField(blank=True, null=True)

    # 整数
    sort = models.IntegerField(default=0, blank=True, null=True)

    # 浮点数，存在精度误差 ❌
    score_float = models.FloatField(default=0.0, blank=True, null=True)

    # 高精度十进制小数，必须指定 max_digits 和 decimal_places ✅
    score_decimal = models.DecimalField(
        max_digits=5,  # 总共最多5位数字（整数+小数一起算）
        decimal_places=2,  # 小数点后保留2位
        default=0.00,
        blank=True,
        null=True,
    )

    # 布尔型，不建议 null=True；布尔一般只有 True/False
    is_valid = models.BooleanField(default=True)

    # 日期：创建时自动写入，后续不再改动
    create_date = models.DateField(auto_now_add=True, blank=True, null=True)

    # 时间：每次保存修改都会自动更新
    update_time = models.DateTimeField(auto_now=True, blank=True, null=True)

    # 外键示例1：级联删除
    sense_1 = models.ForeignKey(
        "Sense",
        on_delete=models.CASCADE,
        related_name="case_cascade",
        blank=True,
        null=True,
    )
    # 外键示例2：主记录删除，此字段置NULL，必须配合 null=True
    sense_2 = models.ForeignKey(
        "Sense",
        on_delete=models.SET_NULL,
        related_name="case_setnull",
        blank=True,
        null=True,
    )
    # 外键示例3：主记录删除，此字段设置为预先定义的默认值
    sense_3 = models.ForeignKey(
        "Sense",
        on_delete=models.SET_DEFAULT,
        related_name="case_setdefault",
        default="设为默认",
        blank=True,
        null=True,
    )

    # 多对多：默认 symmetrical=True，即 “互为关系”，这种情况下（symmetrical≠False）不需要 related_name，
    tag_list = models.ManyToManyField("Tag", blank=True)

    def __str__(self):
        return self.word_text or "无内容"

    class Meta:
        ordering = ["sort"]
```


### 3.2 字段参数中的 `verbose_name` 和 `related_name`
- verbose_name = "字段的单数显示名称"
- verbose_name_plural = "字段的复数显示名称"
- related_name = "内Model反向查询外Model"

一般来说，外Model查询内Model，只需"OuterModel.字段"就能拿到；

而反之却不能直接查询，需写成"InnerModel.related_name"的样式才能达到目的

常见于 `ForeignKey`

⚠️ 另外要注意，在 `ManyToManyField` 中，当 symmetrical = True（默认）的时候，没有反向查询名字，两边都直接用同一个字段名查。因为关系是对等的：A 和 B 互相绑定，不分 “主、反”。
```python
from django.db import models


class Word(models.Model):
    text = models.CharField(max_length=50, blank=True, null=True)
    # symmetrical=True，默认，可省略
    # 不能写 related_name
    synonym = models.ManyToManyField("self", symmetrical=True, blank=True)
```
```python
w1 = Word.objects.create(text="甲")
w2 = Word.objects.create(text="乙")
w1.synonym.add(w2)
```
查询：
```python
w1.synonym.all()  # 拿到 [w2]
w2.synonym.all()  # 拿到 [w1] ✅ 直接同样写法！
```
👉 不管是 w1 还是 w2，都用 `.synonym`，没有别的反向名字。

### 3.3 `class Meta:` 模型的元配置
- verbose_name = "模型的单数显示名称"
- verbose_name_plural = "模型的复数显示名称"
- ordering = ["-A","B"] 👉 负号为逆序
- unique_together = ("A","B")

### 3.4 `def __str__(self):` 对象的脸
是每个对象呈现的“脸”，以供print()或后台展示
```python
# 通常：
return self.字段
```

Dart：在类方法里直接写成员名，默认自动从当前对象 `this` 查找，`this.` 可以省略。

Python：**不会自动从当前对象查找成员**。
`def` 里面写名字，只会按 LEGB 顺序找普通变量（局部→外层闭包→全局→内置）；
这条查找链不包含实例对象、也不包含类命名空间。
想要访问当前对象身上的成员，必须显式写上 `self.`。

⚠️ 重要：`class` 包住 `def` 不算闭包，类命名空间不在 LEGB 查找链中。

示例：
```python
name = "全局"


class Demo:
    name = "类内"

    def test(self):
        print(name)  # 走LEGB，拿到全局的name
        print(self.name)  # ✅ 指定去当前实例找
```
你可以把 `self.` 理解成，给 Python 解释器加的导盲犬

### 3.5 `IntegerChoices` / `TextChoices` 下拉枚举
作用：枚举选项，用于后台下拉选择
- `IntegerChoices`：数据库存数字，搭配 `IntegerField`
- `TextChoices`：数据库存字符串，搭配 `CharField`，必须写 `max_length`
```python
from django.db import models


# IntegerChoices：值是数字，适合用数字标记类型
class Status(models.IntegerChoices):
    DRAFT = 0, "草稿"  # 等号左边：代码常量；第一个值：存入数据库；第二个：后台显示文字
    PUB = 1, "发布"


# TextChoices：值是字符串，适合简短标识
class Type(models.TextChoices):
    N = "n", "普通"  # 等号左边：代码常量；第一个值：存入数据库；第二个：后台显示文字
    F = "f", "比喻"


class Article(models.Model):
    # choices=Status.choices 把上面枚举绑定到字段；default 设置新建时默认值
    status = models.IntegerField(choices=Status.choices, default=Status.DRAFT)
    typ = models.CharField(max_length=1, choices=Type.choices, default=Type.N)
```

### 3.6 `@property` 只读属性
> 作用：把一个方法伪装成**属性**，调用的时候不用加括号 `()`；只做读取，一般不写保存逻辑。
> 它**不是数据库字段！不会在数据库生成列**，只是在代码层面动态计算出来的值。

示例，直接接在上面的模型里面：
```python
from django.db import models


class Sense(models.Model):
    word_text = models.CharField(max_length=100, blank=True, null=True)
    explanation = models.TextField(blank=True, null=True)

    # 只读属性，调用：obj.full_info  不是 obj.full_info()
    @property
    def full_info(self):
        if self.explanation:
            return f"{self.word_text}：{self.explanation}"
        return self.word_text

    def __str__(self):
        return self.full_info or "无内容"
```

## 4. admin.py

### 4.1 导入
```python
from django.contrib import admin  # 导入 admin 模块
from .models import 模型1, 模型2, 模型3 # 导入当前 app 中的模型
```

### 4.2 注册模型
```python
@admin.register(模型1) # 将模型注册到后台管理站点
class 模型1Admin(admin.ModelAdmin): # 继承 admin.ModelAdmin
    list_display = ("字段1", "字段2", "字段3") # 显示
    list_filter = ("字段4", "字段5") # 过滤
    search_fields = ("字段6", "字段7") # 搜索

@admin.register(模型2)
class 模型2Admin(admin.ModelAdmin):
    list_display = ("字段1", "字段2")
    list_filter = ("字段3")
    search_fields = ("字段4")
```

## 5. views.py
视图常见的编写方式有两种：

- **FBV**（Function-Based View）：基于函数的视图。
- **CBV**（Class-Based View）：基于类的视图。

### 5.1 按需导入模型和序列化器
```python
from .models import 模型1, 模型2, 模型3 # 导入当前 app 中的模型
from .serializers import ( # 导入当前 app 中的序列化器
    模型1Serializer,
    模型2Serializer,
    模型3Serializer,
)
```

### 5.2 APIView
#### 5.2.1 初识 APIView
```python
from rest_framework.views import APIView

class 类视图(APIView):
    def get(self, request):
        pass

    def post(self, request):
        pass

    def put(self, request):
        pass

    def patch(self, request):
        pass

    def delete(self, request):
        pass
```

收到 GET 请求时执行 `get()`；收到 POST 请求时执行 `post()`；收到 PUT 请求时执行 `put()`；收到 PATCH 请求时执行 `patch()`；收到 DELETE 请求时执行 `delete()`。

#### 5.2.2 APIView 的 `Request` 对象和属性
- `request`：DRF 的 `APIView` 会把请求包装成 DRF 的 `Request` 对象，常用属性如下。

| 属性                     | 作用          | 示例                         |
| ---------------------- | ----------- | -------------------------- |
| `request.method`       | 获取请求方法      | `GET`、`POST`               |
| `request.data`         | 获取请求提交的数据   | `request.data.get('name')` |
| `request.query_params` | 获取 URL 查询参数 | `?name=Tom`                |
| `request.user`         | 当前认证用户      | `request.user`             |

```python
from rest_framework.views import APIView
from rest_framework.response import Response

class RequestInfoView(APIView):

    def get(self, request):
        # 1. request.method：获取请求方法
        # GET /api/request-info/?name=Tom
        method = request.method  # "GET"

        # 2. request.query_params：获取 URL 查询参数
        # 例如 ?name=Tom&age=18

        name = request.query_params.get("name", "未提供")
        age = request.query_params.get("age", "未提供")

        # 3. request.user：获取当前认证用户
        # 未认证时通常是 AnonymousUser
        user = str(request.user)

        return Response({
            "method": method,
            "query_params": {
                "name": name,
                "age": age,
            },
            "user": user,
        })

    def post(self, request):
        # 1. request.method：请求方法
        method = request.method  # "POST"

        # 2. request.data：请求体提交的数据
        # 例如 JSON：{"name": "Tom", "age": 18}
        name = request.data.get("name", "未提供")
        age = request.data.get("age", "未提供")

        # 3. request.user：当前认证用户
        user = str(request.user)

        return Response({
            "method": method,
            "data": {
                "name": name,
                "age": age,
            },
            "user": user,
        })
```
Python 的 .get() 方法通常有两个参数：
```python
字典.get("键名", "默认值")

# "键名"：要获取的字段名。
# "默认值"：如果这个键不存在，就返回这个默认值
# 如果没有写第二个参数（即"默认值"），且要获取的字段名不存在的话，则默认返回 None
```

#### 5.2.3 `Response` 对象
```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class TestView(APIView):

    def get(self, request):
        return Response(
            {'message': '获取成功'},
            status=status.HTTP_200_OK,
        )
```
`Response` 的第一个参数通常是返回的数据，`status` 参数设置真正的 HTTP 状态码。

常见状态码：

| 状态码常量                            | 数值  | 含义         |
| -------------------------------- | --- | ---------- |
| `HTTP_200_OK`                    | 200 | 请求成功       |
| `HTTP_201_CREATED`               | 201 | 创建成功       |
| `HTTP_204_NO_CONTENT`            | 204 | 操作成功，无响应内容 |
| `HTTP_400_BAD_REQUEST`           | 400 | 请求数据无效     |
| `HTTP_401_UNAUTHORIZED`          | 401 | 未通过身份认证    |
| `HTTP_403_FORBIDDEN`             | 403 | 不允许访问      |
| `HTTP_404_NOT_FOUND`             | 404 | 资源不存在      |
| `HTTP_405_METHOD_NOT_ALLOWED`    | 405 | 不支持该请求方法   |
| `HTTP_500_INTERNAL_SERVER_ERROR` | 500 | 服务器内部错误    |

实际开发中，推荐使用 `status.HTTP_...` 常量，而不是到处直接写数字。

### 5.3 ModelViewSet
#### 5.3.1 初识 ModelViewSet
```python
from .models import 模型
from .serializers import 模型Serializer
from rest_framework import viewsets

class 模型ViewSet(viewsets.ModelViewSet):
    # 1. 指定数据源
    queryset = 模型.objects.all()

    # 2. 指定序列化器
    serializer_class = 模型Serializer
```
结合当前 app 中的 `urls.py` 文件（后面会学到），可以自动生成一整套标准的 CRUD 路由，无须手写每一条 `path()`。

#### 5.3.2 ModelViewSet 自动提供哪些功能？
| HTTP 方法  | 默认 action        | 功能     |
| -------- | ---------------- | ------ |
| `GET`    | `list`           | 获取数据列表 |
| `POST`   | `create`         | 创建数据   |
| `GET`    | `retrieve`       | 获取单条数据 |
| `PUT`    | `update`         | 整体更新   |
| `PATCH`  | `partial_update` | 部分更新   |
| `DELETE` | `destroy`        | 删除数据   |

假设学生 API 的基础路径是 `/api/students/`：
```python
from .models import 
from .serializers import StudentSerializer
from rest_framework import viewsets

class StudentViewSet(viewsets.ModelViewSet):
    # 1. 指定数据源
    queryset = Student.objects.all()

    # 2. 指定序列化器
    serializer_class = StudentSerializer
```

| 请求方法     | 请求地址               | 功能              |
| -------- | ------------------ | --------------- |
| `GET`    | `/api/students/`   | 查询全部学生          |
| `POST`   | `/api/students/`   | 创建学生            |
| `GET`    | `/api/students/1/` | 查询 ID 为 1 的学生   |
| `PUT`    | `/api/students/1/` | 整体更新 ID 为 1 的学生 |
| `PATCH`  | `/api/students/1/` | 部分更新 ID 为 1 的学生 |
| `DELETE` | `/api/students/1/` | 删除 ID 为 1 的学生   |

注意：这里的 `1` 是学生记录的主键，实际项目中会根据数据库数据变化。

#### 5.3.3 重写 ModelViewSet 自带的 CRUD 逻辑
- 基本模板
```python
from rest_framework import viewsets
from rest_framework.response import Response
from .models import Student
from .serializers import StudentSerializer

class StudentViewSet(viewsets.ModelViewSet):
    # ===== 基础配置 =====
    queryset = Student.objects.all()
    serializer_class = StudentSerializer

    # ===== 一、重写 CRUD 方法 =====

    # GET：查询列表
    def list(self, request, *args, **kwargs):
        pass

    # POST：创建数据
    def create(self, request, *args, **kwargs):
        pass

    # GET：查询单条数据
    def retrieve(self, request, *args, **kwargs):
        pass

    # PUT：整体更新
    def update(self, request, *args, **kwargs):
        pass

    # PATCH：部分更新
    def partial_update(self, request, *args, **kwargs):
        pass

    # DELETE：删除数据
    def destroy(self, request, *args, **kwargs):
        pass

    # ===== 二、重写辅助方法 =====

    # 动态选择序列化器
    def get_serializer_class(self):
        return super().get_serializer_class()

    # 自定义创建逻辑
    def perform_create(self, serializer):
        serializer.save()
```
#### 5.3.4 重写辅助方法之 `get_serializer_class()`
- 动态选择序列化器
假设你有两个序列化器（后面会学到）：
```python
# serializers.py

from rest_framework import serializers
from .models import Student


# 查询列表时使用：只返回姓名和年龄
class StudentListSerializer(serializers.ModelSerializer):
    class Meta:
        model = Student
        fields = ["id", "name", "age"]


# 创建或查询单条数据时使用：返回所有字段
class StudentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Student
        fields = "__all__"
```
在 `views.py` 中：
```python
from rest_framework import viewsets
from .models import Student
from .serializers import StudentSerializer, StudentListSerializer


class StudentViewSet(viewsets.ModelViewSet):
    queryset = Student.objects.all()
    serializer_class = StudentSerializer

    # 根据当前操作，动态选择序列化器
    def get_serializer_class(self):
        if self.action == "list":
            return StudentListSerializer

        return StudentSerializer
```
#### 5.3.5 重写辅助方法之 `perform_create()`
- 自定义创建逻辑
假设你的 `Student` 模型有一个 `created_by` 字段，用来记录是谁创建了这条学生记录。
```python
# models.py

from django.db import models
from django.conf import settings


class Student(models.Model):
    name = models.CharField(max_length=50)
    age = models.IntegerField()
    email = models.EmailField()

    # 记录创建者
    created_by = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )
```
在 `views.py` 中：
```python
from rest_framework import viewsets
from .models import Student
from .serializers import StudentSerializer


class StudentViewSet(viewsets.ModelViewSet):
    queryset = Student.objects.all()
    serializer_class = StudentSerializer

    # 自定义创建逻辑
    def perform_create(self, serializer):
        serializer.save(created_by=self.request.user)
```

### 5.4 模型管理器 `Model.objects.`
在 Django 中，`Model.objects.` 是一个非常重要的知识点。你可以把它理解为：通过 `objects` 管理器，对数据库中的模型数据进行查询、创建、修改和删除。
#### 5.4.1 查询数据
```python
# 1. 查询全部学生，返回 QuerySet（查询集）
students = Student.objects.all()

# 2. 条件查询：查询年龄等于 18 的学生
students = Student.objects.filter(age=18)

# 3. 排除条件：查询年龄不等于 18 的学生
students = Student.objects.exclude(age=18)

# 4. 查询单条数据：必须且只能匹配一条记录，否则抛出异常
student = Student.objects.get(pk=1)

# 5. 获取第一条数据，没有数据则返回 None
student = Student.objects.filter(age=18).first()

# 6. 获取最后一条数据
student = Student.objects.filter(age=18).last()
```
#### 5.4.2 条件查询：常见字段查找
```python
# 等于
Student.objects.filter(age__exact=18)

# 大于 / 大于等于
Student.objects.filter(age__gt=18)
Student.objects.filter(age__gte=18)

# 小于 / 小于等于
Student.objects.filter(age__lt=18)
Student.objects.filter(age__lte=18)

# 属于指定集合
Student.objects.filter(age__in=[18, 20, 22])

# 字符串包含（contains 区分大小写，具体取决于数据库）
Student.objects.filter(name__contains="张")

# 字符串包含（icontains 通常不区分大小写）
Student.objects.filter(name__icontains="zhang")

# 以指定内容开头 / 结尾
Student.objects.filter(name__startswith="张")
Student.objects.filter(name__endswith="三")

# 字段是否为 NULL
Student.objects.filter(email__isnull=True)
```
#### 5.4.3 排序、去重、限制结果
```python
# 按年龄升序排列
Student.objects.order_by("age")

# 按年龄降序排列
Student.objects.order_by("-age")

# 先按年龄升序，再按姓名升序
Student.objects.order_by("age", "name")

# 查询所有不同的年龄值
Student.objects.values("age").distinct()

# 只取前 5 条记录
Student.objects.all()[:5]

# 取第 2 至第 5 条记录（切片右侧不包含）
Student.objects.all()[1:5]
```
#### 5.4.4 指定返回字段
```python
# values()：返回字典组成的 QuerySet
Student.objects.values("id", "name", "age")

# values_list()：返回元组组成的 QuerySet
Student.objects.values_list("name", "age")

# 只返回 name 字段组成的元组
Student.objects.values_list("name", flat=True)
```
#### 5.4.5 创建数据
```python
# 方法一：create()，创建并保存，返回模型实例
student = Student.objects.create(
    name="张三",
    age=18,
    email="zhangsan@example.com",
)

# 方法二：实例化对象，再调用 save() 保存
student = Student(
    name="李四",
    age=20,
    email="lisi@example.com",
)
student.save()
```
#### 5.4.6 修改数据
```python
# 批量修改：将所有 18 岁学生的年龄改为 19
Student.objects.filter(age=18).update(age=19)

# 修改单个对象
student = Student.objects.get(pk=1)
student.age = 21
student.save()

# 只更新指定字段（适用于已修改的模型实例）
student.age = 22
student.save(update_fields=["age"])
```
#### 5.4.7 删除数据
```python
# 删除符合条件的学生
Student.objects.filter(age__lt=18).delete()

# 删除单条记录
student = Student.objects.get(pk=1)
student.delete()

# 注意：以下操作会删除全部学生记录，请勿随意执行！
# Student.objects.all().delete()
```
#### 5.4.8 统计与判断
```python
# 统计学生总数
total = Student.objects.count()

# 判断是否存在 18 岁的学生，返回 True 或 False
exists = Student.objects.filter(age=18).exists()

# 统计学生平均年龄
average_age = Student.objects.aggregate(avg_age=Avg("age"))

# 按年龄分组，统计每个年龄对应的学生数量
age_counts = Student.objects.values("age").annotate(
    total=Count("id")
)
```
#### 5.4.9 获取或创建、获取或更新
```python
# 如果 email 对应的学生存在，就获取；否则创建
student, created = Student.objects.get_or_create(
    email="wangwu@example.com",
    defaults={
        "name": "王五",
        "age": 18,
    },
)

# created=True 表示新建；False 表示获取已有记录


# 如果 email 对应的学生存在，就更新；否则创建
student, created = Student.objects.update_or_create(
    email="wangwu@example.com",
    defaults={
        "name": "王五",
        "age": 20,
    },
)
```
#### 5.4.10 批量操作
```python
# 批量创建：一次提交多个模型对象
Student.objects.bulk_create([
    Student(name="学生甲", age=18, email="a@example.com"),
    Student(name="学生乙", age=19, email="b@example.com"),
])

# 批量更新：先获取对象，再修改字段
students = list(Student.objects.filter(age=18))

for student in students:
    student.age = 19

# 将修改后的多个对象批量更新到数据库
Student.objects.bulk_update(students, ["age"])
```
#### 5.4.11 常见使用原则
```python
# filter()：返回 QuerySet，适合查多条或零条数据
students = Student.objects.filter(age__gte=18)

# get()：返回单个模型实例，要求恰好匹配一条记录
student = Student.objects.get(pk=1)

# create()：创建并保存，返回模型实例
student = Student.objects.create(
    name="赵六",
    age=20,
    email="zhaoliu@example.com",
)

# update()：批量更新，返回受影响的记录数
updated_count = Student.objects.filter(age=20).update(age=21)

# delete()：删除符合条件的记录，返回删除统计信息
# deleted_count, details = Student.objects.filter(age=100).delete()
```
#### 5.4.12 总结
```python
# 常用指令

Model.objects.all()
Model.objects.all()[number1:number2]
Model.objects.filter()
Model.objects.exclude()
Model.objects.get()
Model.objects.filter()
Model.objects.filter().first()
Model.objects.filter().last()
Model.objects.order_by("字段名")
Model.objects.order_by("-字段名")
Model.objects.values("字段名").distinct()
Model.objects.create()
instance.save()
Model.objects.update()
instance.delete()
Model.objects.count()
Model.objects.exists()
instance, created = Model.objects.get_or_create()
instance, created = Model.objects.update_or_create()

# 细说查询条件

Model.objects.filter(字段名__exact=等于)
Model.objects.filter(字段名__gt=大于)
Model.objects.filter(字段名__gte=大于等于)
Model.objects.filter(字段名__lt=小于)
Model.objects.filter(字段名__lte=小于等于)
Model.objects.filter(字段名__in=[属于,指定,集合])
Model.objects.filter(字段名__contains="字符串包含，区别大小写")
Model.objects.filter(字段名__icontains="字符串包含，通常不区分大小写")
Model.objects.filter(字段名__startswith="以指定内容开头")
Model.objects.filter(字段名__endswith="以指定内容结尾")
Model.objects.filter(字段名__isnull=True) # 查询字段为 NULL 的
```

### 5.5 @api_view([])
#### 5.5.1 初识 @api_view([])
`@api_view([])` 是 Django REST Framework（DRF）提供的 FBV 装饰器，用于将普通 Django 函数视图转换为 DRF API 视图，并指定允许使用的 HTTP 请求方法。

最重要的是：方括号 `[]` 中填写允许的请求方法。

例如：

```python
@api_view(["GET"])
```

表示这个接口只允许使用 `GET` 请求。

#### 5.5.2 基础语法
```python
from rest_framework.decorators import api_view
from rest_framework.response import Response


@api_view(["GET"])
def student_list(request):
    # 处理 GET 请求
    return Response({"message": "查询学生列表"})
```

逐行理解：

- `api_view`：从 DRF 导入函数视图装饰器。
- `@api_view(["GET"])`：规定接口只允许 GET 请求。
- `request`：DRF 封装后的请求对象。
- `Response()`：返回 API 响应数据，DRF 会根据内容协商等配置处理响应格式。

注意：`@api_view([])` 如果真的传入空列表，表示没有允许的 HTTP 请求方法，并不是允许所有方法。正常情况下，请求会被拒绝。因此，通常需要填写 `"GET"`、`"POST"` 等方法。

#### 5.5.3 常见 HTTP 方法

| 写法                           | 用途     | 常见场景   |
| ---------------------------- | ------ | ------ |
| `@api_view(["GET"])`         | 查询数据   | 获取学生列表 |
| `@api_view(["POST"])`        | 提交数据   | 创建学生   |
| `@api_view(["PUT"])`         | 整体更新   | 替换学生信息 |
| `@api_view(["PATCH"])`       | 部分更新   | 修改学生年龄 |
| `@api_view(["DELETE"])`      | 删除数据   | 删除学生   |
| `@api_view(["GET", "POST"])` | 允许多种方法 | 查询和创建  |

HTTP 方法名一般使用大写字符串。

#### 5.5.4 同一个接口允许 GET 和 POST

如果希望同一个函数既能查询学生，又能创建学生，可以这样写：

```python
# views.py

from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status

from .models import Student
from .serializers import StudentSerializer


# 同时允许 GET 和 POST
@api_view(["GET", "POST"])
def student_list(request):

    # GET：查询所有学生
    if request.method == "GET":
        students = Student.objects.all()
        serializer = StudentSerializer(students, many=True)

        return Response(serializer.data)

    # POST：创建学生
    elif request.method == "POST":
        serializer = StudentSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        serializer.save()

        return Response(
            serializer.data,
            status=status.HTTP_201_CREATED,
        )
```
这里的：
```python
request.method
```
用于判断当前请求的方法。

虽然 `@api_view(["GET", "POST"])` 已经限制了请求方法，但在函数内部仍然可以使用 `request.method` 区分不同业务逻辑。

这里还有一个重要知识点：

```python
serializer = StudentSerializer(students, many=True)
```

因为 `students` 包含多条学生记录，所以要写 `many=True`。如果序列化的是单个学生对象，则通常不需要它。

#### 5.5.5 与 `APIView`、`ModelViewSet` 的区别

| 特点        | `@api_view`      | `APIView`            | `ModelViewSet`  |
| --------- | ---------------- | -------------------- | --------------- |
| 视图形式      | FBV               | CBV                    | CBV               |
| HTTP 方法处理 | `request.method` | `get()`、`post()` 等方法 | 内置 CRUD actions |
| 代码灵活性     | 适合简单接口           | 适合自定义复杂逻辑            | 适合标准 CRUD       |
| 默认 CRUD   | 不提供              | 不提供                  | 提供              |

你可以这样记忆：

- `@api_view`：函数写接口。
- `APIView`：类写接口。
- `ModelViewSet`：快速构建标准增删改查接口。

### 5.6 @action()
#### 5.6.1 初识 @action()

`@action()` 是 Django REST Framework（DRF）中用于 给 `ViewSet` 或 `ModelViewSet` **添加**自定义 API 接口的装饰器。

你已经学过 `ModelViewSet` 的标准 CRUD 方法，例如 `list()`、`create()`、`retrieve()`。如果现在想增加一个不属于标准 CRUD 的功能，例如：

- 查询某个学生的成绩
- 给学生重置密码
- 批量审核学生
- 查询学生的个人资料

就可以使用 `@action()`。

一句话记忆：**标准 CRUD 用内置方法，额外业务功能用 `@action()`。**

#### 5.6.2 基础语法
```python
from rest_framework import viewsets
from rest_framework.decorators import action
from rest_framework.response import Response

from .models import Student
from .serializers import StudentSerializer


class StudentViewSet(viewsets.ModelViewSet):

    # 标准 CRUD 的基础配置
    queryset = Student.objects.all()
    serializer_class = StudentSerializer

    # ==========================================
    # 自定义接口：查询所有成年学生
    # ==========================================

    @action(
        detail=False,                 # False：查询集合，不需要指定学生 ID
        methods=["get"],              # 只允许 GET 请求
        url_path="adult-students",    # 自定义 URL 路径
    )
    def get_adults(self, request):
        # 第一步：查询年龄大于或等于 18 岁的学生
        students = Student.objects.filter(age__gte=18)

        # 第二步：将多条学生记录转换为序列化数据
        serializer = self.get_serializer(students, many=True)

        # 第三步：返回 API 响应
        return Response(serializer.data)
```
假设原来 `StudentViewSet` 的基础路由是 `/api/students/`，只需要在这个基础路由后加上自定义的 `url_path`，即 `/api/students/adult-students/`，请求就会访问该 `@action()` ，并执行 `get_adults()` 方法。

### 5.7 DjangoFilterBackend

#### 5.7.1 初识 DjangoFilterBackend

`DjangoFilterBackend` 是 Django REST Framework（DRF）中常用的过滤后端，可以让用户通过 URL 查询参数筛选数据，而不需要自己手动编写大量的判断逻辑。

例如，你有一个学生列表接口：

```python
GET /api/students/
```

希望查询年龄大于等于 18 岁的学生，可以通过 URL 传递参数：

```python
GET /api/students/?age__gte=18
```

希望查询指定性别的学生：

```python
GET /api/students/?gender=男
```

这就是 `DjangoFilterBackend` 的主要用途：把 URL 中的查询参数转换成 Django ORM 的过滤条件。

#### 5.7.2 安装与配置

#### 5.7.2.1 安装依赖
在 VS Code 的终端中执行：

```bash
pip install django-filter
```

`django-filter` 是一个独立的第三方库，需要单独安装。
#### 5.7.2.2 注册应用
打开项目的 `settings.py`，在 `INSTALLED_APPS` 中添加：

```python
INSTALLED_APPS = [
    # Django 自带应用
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",

    # Django REST Framework
    "rest_framework",

    # Django 过滤库
    "django_filters",

    # 自己的应用
    "students",
]
```

注意：Python 包的安装名称是 `django-filter`，在 `INSTALLED_APPS` 中的注册名称是 `django_filters`。
#### 5.7.2.3 配置 DRF 的过滤后端
有两种配置方式。

- 方式一：全局配置

在 `settings.py` 中添加：

```python
REST_FRAMEWORK = {
    # 全局启用 DjangoFilterBackend
    "DEFAULT_FILTER_BACKENDS": [
        "django_filters.rest_framework.DjangoFilterBackend",
    ],
}
```

这样，所有使用 DRF 的视图默认都会启用这个过滤后端。

- 方式二：单个视图配置

```python
from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import viewsets

class StudentViewSet(viewsets.ModelViewSet):
    queryset = Student.objects.all()
    serializer_class = StudentSerializer

    # 只为当前视图启用过滤
    filter_backends = [DjangoFilterBackend]
```

如果你只想让部分接口支持过滤，建议先学习第二种方式，更容易理解过滤功能的作用范围。
#### 5.7.3 最基础的用法：`filterset_fields`

这是最适合初学者掌握的方式，可以快速指定哪些字段允许通过 URL 筛选。

#### 5.7.3.1 示例模型 `models.py`
假设你的学生模型如下：

```python
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=50)
    age = models.IntegerField()
    gender = models.CharField(max_length=10)
    city = models.CharField(max_length=50)

    def __str__(self):
        return self.name
```

假设数据库中有以下学生：

| id | name | age | gender | city |
| -- | ---- | --- | ------ | ---- |
| 1  | 张三   | 18  | 男      | 北京   |
| 2  | 李四   | 20  | 男      | 上海   |
| 3  | 王五   | 19  | 男      | 广州   |
| 4  | 赵六   | 22  | 女      | 上海   |
| 5  | 七妹   | 16  | 女      | 香港   |
#### 5.7.3.2 编写视图 `views.py`
```python
from rest_framework import viewsets
from django_filters.rest_framework import DjangoFilterBackend

from .models import Student
from .serializers import StudentSerializer


class StudentViewSet(viewsets.ModelViewSet):
    # 设置基础查询集
    queryset = Student.objects.all()

    # 指定序列化器
    serializer_class = StudentSerializer

    # 启用 DjangoFilterBackend
    filter_backends = [DjangoFilterBackend]

    # 指定允许过滤的字段
    filterset_fields = ["name", "age", "gender", "city"]
```

重点理解这两行：

```python
filter_backends = [DjangoFilterBackend]
filterset_fields = ["name", "age", "gender", "city"]
```

- `filter_backends`：指定使用什么过滤后端。
- `filterset_fields`：指定允许用户通过查询参数筛选哪些模型字段。
#### 5.7.3.3 实际请求示例
假设路由已经注册为 `/api/students/`。

示例 1：按性别过滤

`GET /api/students/?gender=男`

返回张三、李四、王五。因为三人的 gender 都是“男”。

示例 2：按城市过滤

`GET /api/students/?city=上海`

返回李四、赵六。

示例 3：组合过滤

`GET /api/students/?gender=女&city=香港`

返回同时满足两个条件的学生，即七妹。

注意：多个查询参数默认是 AND（并且）关系。 也就是说，数据必须同时满足所有过滤条件。
#### 5.7.4 进阶用法：支持大于、小于等条件

`filterset_fields` 的简单列表写法主要提供字段的精确匹配。如果需要年龄大于等于某个值，或者小于某个值，就可以使用字典写法指定过滤查找类型。

```python
class StudentViewSet(viewsets.ModelViewSet):
    queryset = Student.objects.all()
    serializer_class = StudentSerializer

    filter_backends = [DjangoFilterBackend]

    # 为每个字段指定允许使用的过滤方式
    filterset_fields = {
        "name": ["exact", "icontains"],
        "age": ["exact", "gt", "gte", "lt", "lte"],
        "gender": ["exact"],
        "city": ["exact", "icontains"],
    }
```

常见的查找类型：

| 查找类型        | 含义            | URL 示例               |
| ----------- | ------------- | -------------------- |
| `exact`     | 精确匹配          | `?age=18`            |
| `gt`        | 大于            | `?age__gt=18`        |
| `gte`       | 大于等于          | `?age__gte=18`       |
| `lt`        | 小于            | `?age__lt=18`        |
| `lte`       | 小于等于          | `?age__lte=18`       |
| `icontains` | 包含指定文本，不区分大小写 | `?name__icontains=张` |

这里的 `__` 是 Django ORM 的双下划线查找语法。

例如：

```python
GET /api/students/?age__gte=18
```

相当于：

```python
Student.objects.filter(age__gte=18)
```

会筛选出年龄大于或等于 18 岁的学生。

再比如：

```python
GET /api/students/?name__icontains=张
```

相当于：

```python
Student.objects.filter(name__icontains="张")
```

会筛选名字中包含“张”的学生。

注意： 这里的 `filterset_fields` 字典是在声明允许使用的过滤方法，不是让你直接填写过滤值。具体值由请求 URL 传入。

## 6. urls.py
### 6.1 `urls.py` 是什么？

`urls.py` 是 Django 项目中用于配置 URL 路由的文件，主要负责将用户请求的 URL 分配给对应的视图函数或视图类。

你可以把 Django 网站理解成一家医院：

- `urls.py`：挂号处，根据需求安排去哪个科室。
- `views.py`：医生，负责处理具体业务。
- `models.py`：数据库模型，负责定义数据结构。
- `serializers.py`：DRF 序列化器，负责数据转换与校验。

例如，用户访问：

```python
http://127.0.0.1:8000/api/students/
```

Django 会根据 URL 路由配置，找到对应的视图，执行查询学生数据、序列化数据等操作，最后返回响应。

### 6.2 项目主路由与 app 路由
Django 项目通常将路由分为**项目主路由**和 **App 路由**。

- **项目主路由**：负责引入各个 App 的路由，并设置统一的 URL 前缀。
- **App 路由**：负责配置当前 App 的具体接口，包括普通视图、类视图和 DRF 的 ViewSet 接口。
### 6.3 项目主路由

一个典型的 Django 项目主路由文件如下：

```python
# 项目主路由：项目/config/urls.py

from django.contrib import admin
from django.urls import path, include # 导入路由配置函数

urlpatterns = [ # Django 默认识别的路由列表
    # Django 后台管理
    path("admin/", admin.site.urls), # 将 /admin/ 分配给后台管理系统

    # 学生列表页面
    path("api/", include("students.urls")), # 把 students 应用的 urls.py 中的路由挂载到 /api/ 前缀下
]
```

### 6.4 App 路由

#### 6.4.1 基本结构

一个典型的 App 路由文件如下：

```python
# 项目/App/urls.py

# 1. 导入 path，用于定义 URL 路由规则
from django.urls import path

# 2. 导入当前 App 的 views.py
from . import views


# 3. urlpatterns 路由列表
urlpatterns = [
    # 1. 普通 FBV：直接传入函数
    # --------------------------------------------------------
    # 当用户访问 /api/students/，Django 会调用 views.student_list
    path("students", views.student_list, name="student-list"),


    # 2. 动态参数：从 URL 中获取整数 id
    # --------------------------------------------------------
    # <int:id> 表示从 URL 中获取一个整数，并传递给视图
    # 例如访问 /api/students/1/，id 的值就是 1
    path("students/<int:id>/", views.student_detail, name="student-detail"),

    # 3. CBV：使用 .as_view() 转换为可调用的视图
    # --------------------------------------------------------
    # 假设 views.py 中定义了 LoginView 类
    path("login/", views.LoginView.as_view(), name="login"),

]
```

**补充知识点：**

- `name`：路由名称，用于反向解析 URL。
- `<int:id>`：接收整数类型的**动态参数**，传递给视图函数的 `id` 参数。
- 注意：视图函数必须接收同名参数 id
- 注意：`views.student_list` 、`views.student_detail` 后面没有括号。

正确：

```python
path("students/", views.student_list)
```

错误：

```python
path("students/", views.student_list())
```

因为路由需要保存视图函数本身，等到请求到来时再调用，而不是在配置路由时提前执行函数。

#### 6.4.2 DRF 路由器

如果项目使用 `ModelViewSet` 或 `ViewSet`，通常使用 `DefaultRouter` **自动生成**路由，不必为每个 CRUD 操作手动写一个 `path()`。

```python
# ============================================================
# 使用 DRF Router 配置 ViewSet
# ============================================================

# 1. 导入 DRF 路由器
from rest_framework.routers import DefaultRouter

# 2. 导入当前 App 中的视图集
from .views import BookViewSet

# 3. 创建路由器对象
router = DefaultRouter()

# 4. 注册视图集
# "books" 是 URL 前缀
# BookViewSet 是负责处理请求的视图集
# basename 是生成路由名称时使用的基础名称
router.register("books", BookViewSet, basename="book")

# 5. 将路由器自动生成的路由加入 urlpatterns
# *router.urls 表示把 router.urls 中的路由逐项展开
urlpatterns = [
    *router.urls,
]
```
`*router.urls` 自动生成的路由
```
+---- GET     /api/books/      获取图书列表
|
+---- POST    /api/books/      创建图书
|
+---- GET     /api/books/1/    获取图书详情
|
+---- PUT     /api/books/1/    完整更新图书
|
+---- PATCH   /api/books/1/    部分更新图书
|
+---- DELETE  /api/books/1/    删除图书
```

如果项目已经有普通路由，可以直接混合使用：

```python
urlpatterns = [
    # 普通 FBV
    path("bulletin-board/", views.bulletin_board, name="bulletin-board"),

    # 普通 CBV
    path("login/", views.LoginView.as_view(), name="login"),

    # DRF Router 自动生成的路由
    *router.urls,
]
```
#### 6.4.3 App 路由核心格式总结

```python
from django.urls import path
from . import views
from rest_framework.routers import DefaultRouter
from .views import 视图集

router = DefaultRouter()
router.register("路由地址", 视图集, basename="基础路由名称")

urlpatterns = [
path("路由地址", views.视图函数, name="路由名称"),
path("路由地址 + 动态参数", views.视图函数, name="路由名称"),
path("路由地址", views.视图类.as_view(), name="路由名称"),
*router.urls,
]
```

## 7. serializers.py

## 8. permissions.py
