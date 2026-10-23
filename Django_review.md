
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
from django.db import models


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

### 3.3 `class Meta:` 模型的元配置块
- verbose_name = "模型的单数显示名称"
- verbose_name_plural = "模型的复数显示名称"
- ordering = [-"A","B"] 👉 负号为逆序
- unique_together = ["A","B"]

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

## 5. views.py
## 6. urls.py
## 7. serializers.py

## 8. permissions.py