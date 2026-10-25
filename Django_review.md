
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
Model.objects.get_or_create()
Model.objects.update_or_create()

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
```python

```
### 5.6 @action()
```python
```
### 5.7 DjangoFilterBackend
```python
```


## 6. urls.py
## 7. serializers.py

## 8. permissions.py