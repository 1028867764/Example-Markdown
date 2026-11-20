# 1. Python基础

## 1.1 基本语法规则

### 1.1.1 缩进（Indentation）
Python 用**缩进**（通常是4个空格）来表示代码块，而不是用 `{}`。

```python
# 示例：if语句的缩进
age = 18

if age >= 18:
    print("你已经成年了！")   # 这行必须缩进
    print("可以考驾照啦~")
else:
    print("你还未成年")       # 这行也必须缩进

print("程序结束")             # 这行不缩进，属于整个程序
```

**注意**：同一代码块的缩进量必须完全一致，否则会报错 `IndentationError`。

### 1.1.2 注释（# 和 """ """）

```python
# 这是一行注释，程序不会执行这行

print("Hello World")  # 这是在代码后面的行内注释

"""
这是多行注释
可以写很多行
用来解释复杂的代码块
"""
print("多行注释示例")
```

### 1.1.3 编码声明（通常写在文件最顶部）
```python
# -*- coding: utf-8 -*-
# 或者
# coding=utf-8

print("支持中文输出没有问题")
```

## 1.2 变量与数据类型

### 1.2.1 数字（int、float、complex）

```python
# 整数 int
a = 10
b = -5
print(a, type(a))        # 10 <class 'int'>

# 浮点数 float
c = 3.14
d = 2.0
print(c, type(c))        # 3.14 <class 'float'>

# 复数 complex
e = 1 + 2j
print(e, type(e))        # (1+2j) <class 'complex'>
```

### 1.2.2 高精度数字（Decimal）

Python 提供了 `decimal` 模块中的 `Decimal` 类型，用于进行高精度小数计算，可以避免普通 `float` 浮点数可能出现的精度误差。

```python
from decimal import Decimal

a = Decimal("0.1")
b = Decimal("0.2")

result = a + b

print(result, type(result)) # 0.3 <class 'decimal.Decimal'>
```

注意：创建 `Decimal` 时，建议使用字符串作为参数，避免引入 `float` 原有的精度误差。

`Decimal` 常用于金额计算、财务计算等对精度要求较高的场景。

### 1.2.3 字符串（str）—— 基础操作、格式化（f-string）

```python
name = "小明"
age = 18
height = 1.75

# 基础操作
print(name + "今年" + str(age) + "岁")   # 字符串拼接
print(name * 3)                         # 字符串重复

# 字符串长度
print(len(name))                        # 2

# 切片
print(name[0])      # 小
print(name[1:])     # 明

# f-string（推荐的现代格式化方式）
print(f"大家好，我叫{name}，今年{age}岁，身高{height}米")
print(f"两年后我就是{age + 2}岁了")
```

### 1.2.4 布尔值（bool）

```python
is_adult = True
is_student = False

print(is_adult, type(is_adult))   # True <class 'bool'>

# 布尔值可以参与运算（True=1, False=0）
print(True + 5)     # 6
print(False * 10)   # 0
```

### 1.2.5 类型转换

```python
age_str = "25"

# 字符串转整数
age = int(age_str)
print(age, type(age))          # 25 <class 'int'>

# 整数转字符串
print("我今年" + str(age) + "岁")

# 浮点数转整数（向下取整）
print(int(3.99))      # 3
print(int(-3.99))      # -3

# 任意类型转布尔值（非0、非空即为True）
print(bool(0))        # False
print(bool(5))        # True
print(bool(""))       # False
print(bool("hello"))  # True
```

## 1.3 运算符

```python
a = 10
b = 3

# 1. 算术运算符
print(a + b)    # 加法  → 13
print(a - b)    # 减法  → 7
print(a * b)    # 乘法  → 30
print(a / b)    # 除法  → 3.333...
print(a // b)   # 整除  → 3
print(a % b)    # 取余  → 1
print(a ** b)   # 幂运算 → 1000

# 2. 比较运算符
print(a > b)    # True
print(a == b)   # False
print(a != b)   # True

# 3. 逻辑运算符
x = True
y = False
print(x and y)  # False
print(x or y)   # True
print(not x)    # False

# 4. 赋值运算符
num = 10
num += 5        # 等价于 num = num + 5
print(num)      # 15

# 5. 成员运算符
fruits = ["苹果", "香蕉", "橙子"]
print("苹果" in fruits)      # True
print("西瓜" not in fruits)  # True

# 6. 身份运算符（判断是否是同一个对象）
p = [1, 2, 3]
q = [1, 2, 3]
r = p

print(p is q)     # False（内容相同但不是同一个对象）
print(p is r)     # True
```

## 1.4 输入输出

```python
# 输出
print("欢迎学习Python！")
print("第一行", "第二行", "第三行", sep=" | ", end=" ===>\n")

# 输入
name = input("请输入你的名字：")
print(f"你好，{name}！欢迎来到Python世界~")

# 输入数字（需要类型转换）
age = int(input("请输入你的年龄："))
print(f"明年你就是{age + 1}岁了！")

# 输入浮点数
height = float(input("请输入你的身高（米）："))
print(f"你的身高是 {height} 米")
```

# 2. 控制流与程序结构

## 2.1 条件判断

### 2.1.1 if、elif、else

```python
# 基本 if 判断
score = 85

if score >= 90:
    print("优秀")
elif score >= 80:
    print("良好")
elif score >= 60:
    print("及格")
else:
    print("不及格")

print("判断结束")
```

### 2.1.2 嵌套 if

```python
age = int(input("请输入你的年龄："))

if age >= 18:
    print("你已经成年")
    if age >= 60:
        print("你可以退休了")
    elif age >= 22:
        print("你可以结婚了")
    else:
        print("你还是年轻人")
else:
    print("你还未成年，需要监护人陪同")
```

### 2.1.3 三元表达式（一行 if-else，代码简洁）

```python
age = 20

# 普通写法
if age >= 18:
    status = "成年"
else:
    status = "未成年"

# 三元表达式（推荐小白先理解普通写法，再使用）
status = "成年" if age >= 18 else "未成年"
print(f"你的状态：{status}")
```

## 2.2 循环语句

### 2.1.1 while 循环（当条件为True时重复执行）

```python
# 基础 while 循环 - 打印 1 到 5
i = 1
while i <= 5:
    print(f"当前数字：{i}")
    i = i + 1          # 必须手动改变条件，否则死循环！

print("循环结束")
```

### 2.1.2 for 循环（最常用）

```python
# 1. 用 range() 生成数字序列
print("用 range() 循环：")
for i in range(5):           # 0,1,2,3,4
    print(i)

print("\n从1到10：")
for i in range(1, 11):       # 1到10
    print(i, end=" ")
print()  # 换行

# 2. 遍历序列（列表、字符串等）
# 遍历列表
print("\n遍历列表：")
fruits = ["苹果", "香蕉", "橙子", "葡萄"]
for fruit in fruits:
    print(f"我喜欢吃：{fruit}")

# 遍历字符串
print("\n遍历字符串：")
for char in "Python":
    print(char)
```

### 2.1.3 continue、break、else 子句、pass

```python
print("=== break 和 continue 示例 ===")

for i in range(1, 11):
    if i == 3:
        continue          # 跳过本次循环，i=3时不打印
    if i == 7:
        break             # 直接结束整个循环
    print(i)              # 1,2,4,5,6

print("\n=== for ... else 示例 ===")
# else 子句：当循环正常结束（没有被 break）时执行
for i in range(5):
    print(i)
else:
    print("循环正常结束，没有遇到break")

print("\n=== pass 示例 ===")
# pass 什么都不做，常用于占位
for i in range(3):
    pass
print("pass不会影响程序运行")
```


## 2.3 异常处理基础

### 2.3.1 try...except...finally...else

```python
print("=== 异常处理基础 ===")

try:
    num1 = int(input("请输入第一个数字："))
    num2 = int(input("请输入第二个数字："))
    
    result = num1 / num2          # 可能发生除零错误
    print(f"计算结果：{num1} ÷ {num2} = {result}")

except ZeroDivisionError:
    print("错误：除数不能为0！")

except ValueError:
    print("错误：请输入正确的数字！")

except Exception as e:            # 捕获所有其他异常
    print(f"发生未知错误：{e}")

else:
    print("程序正常执行，没有发生异常！")   # 只有没有异常时才执行

finally:
    print("无论是否发生异常，这段代码都会执行（常用于关闭文件、释放资源）")
```

### 2.3.2 常见异常类型示例

```python
print("=== 常见异常类型演示 ===")

# 1. ZeroDivisionError
try:
   print(10 / 0)
except ZeroDivisionError:
   print("不能除以0")

# 2. ValueError
try:
    num = int("abc")
except ValueError:
    print("无法将字符串转换为整数")

# 3. IndexError（索引超出范围）
lst = [1, 2, 3]
try:
    print(lst[5])
except IndexError:
    print("列表索引超出范围")

# 4. KeyError（字典键不存在）
d = {"name": "小明"}
try:
    print(d["age"])
except KeyError:
    print("字典中没有这个键")

# 5. TypeError（类型错误）
try:
    print("hello" + 123)
except TypeError:
    print("不同类型不能直接相加")
```

# 3. 数据结构

## 3.1 列表（list）—— 最常用、最灵活的数据结构

```python
# 创建列表
fruits = ["苹果", "香蕉", "橙子", "葡萄"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]

print(fruits)
length = len(fruits)      # 列表长度
print(f"{length}")        # 4

# 1. 查（访问元素）
print(fruits[0])          # 第一个元素：苹果
print(fruits[-1])         # 最后一个元素：葡萄
print(fruits[1:3])        # 切片：从索引1到3（不包含3） → ['香蕉', '橙子']

# 2. 改（修改元素）
fruits[1] = "芒果"
print(fruits)             # ['苹果', '芒果', '橙子', '葡萄']

# 3. 增（添加元素）
fruits.append("西瓜")           # 在末尾添加
fruits.insert(1, "草莓")        # 在指定位置插入
print(fruits)                   # ['苹果', '草莓', '芒果', '橙子', '葡萄', '西瓜']

# 4. 删（删除元素）
fruits.remove("橙子")           # 删除指定值（第一个匹配的）
popped = fruits.pop()           # 删除并返回最后一个元素
print("被删除的元素:", popped)   # 被删除的元素: 西瓜
del fruits[0]                   # 根据索引删除

# 5. 列表推导式（非常常用！优雅又快速）
squares = [x**2 for x in range(1, 6)]
print("平方列表:", squares)     # [1, 4, 9, 16, 25]

even_numbers = [x for x in range(10) if x % 2 == 0]
print("偶数列表:", even_numbers)  # 偶数列表: [0, 2, 4, 6, 8]
```


## 3.2 元组（tuple）—— 不可变列表

```python
# 1. 创建元组
point = (3, 4)
colors = ("red", "green", "blue")
single = (5,)                  # 注意：单个元素元组必须加逗号！

print(point)
print(type(point))  # <class 'tuple'>

# 2. 元组不可变（不能修改、添加、删除）
# point[0] = 10   # 这行会报错！

# 3. 元组打包与解包（非常实用）
x, y = (10, 20)                # 解包
print(f"x = {x}, y = {y}")

# 4. 多个变量一次性赋值
name, age, city = ("小明", 18, "上海")
print(name, age, city)

# 元组常用场景：函数返回多个值、作为字典的键等
```

## 3.3 字典（dict）—— 键值对存储

```python
# 创建字典
student = {
    "name": "小红",
    "age": 17,
    "score": 92,
    "city": "北京"
}

print(student)
print(student["name"])        # 通过键获取值（不推荐）

# 1. 查
print(student.get("age"))                    # 推荐使用get()，避免KeyError
print(student.get("gender", "未知"))         # 如果键不存在，返回默认值

# 2. 增 / 改
student["gender"] = "女"                     # 新增
student["score"] = 95                        # 修改
print(student)

# 3. 删
student.pop("city")                          # 删除指定键并返回其值
print(student)

# 4. setdefault() 方法：如果键不存在则设置默认值
student.setdefault("hobby", "学习Python")
print(student)

# 5. 字典推导式
squares_dict = {x: x**2 for x in range(6)}
print(squares_dict)         # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

## 3.4 集合（set）—— 无序、不重复

```python
# 1. 创建集合
fruits_set = {"苹果", "香蕉", "橙子", "苹果"}   # 自动去重
print(fruits_set)                               # {'苹果', '香蕉', '橙子'}

# 2. 添加和删除
fruits_set.add("西瓜")
fruits_set.remove("香蕉")       # 如果元素不存在会报错
fruits_set.discard("不存在的水果")   # 推荐使用，不会报错

# 3. 集合运算（数学中的集合操作）
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print("并集:", a | b)           # 或 a.union(b)
print("交集:", a & b)           # 或 a.intersection(b)
print("差集:", a - b)           # a中有的但b中没有的
print("对称差:", a ^ b)         # 两个集合中只在一个集合里的元素
```

## 3.5 字符串进阶

```python
s = "  Hello Python World!  "

# 1. 常用字符串方法
print(s.strip())                    # 去除两端空格
print(s.lower())                    # 转小写
print(s.upper())                    # 转大写

print("Python" in s)                # 判断是否包含子串

# 2. split() 和 join()
words = s.strip().split()           # 默认按空格分割
print(words)                        # ['Hello', 'Python', 'World!']

new_s = "-".join(words)
print(new_s)                        # Hello-Python-World!

# 3. replace() 和 find()
print(s.replace("Python", "编程")) 
print(s.find("Python"))             # 返回第一次出现的位置，找不到返回-1

# 4. 格式化字符串（复习f-string）
name = "小明"
age = 18
print(f"我是{name}，今年{age}岁")
```

# 4. 函数

## 4.1 函数定义与调用（def）

```python
# 1. 定义一个最简单的函数
def say_hello():
    """这是一个打招呼函数"""
    print("你好！欢迎学习Python函数！")


# 2. 调用函数
say_hello()  # 你好！欢迎学习Python函数！
say_hello()  # 你好！欢迎学习Python函数！


# 3. 带参数的函数
def greet(name):
    print(f"你好，{name}！今天也要加油哦~")


greet("小明")  # 你好，小明！今天也要加油哦~
greet("小红")  # 你好，小红！今天也要加油哦~
```

## 4.2 参数类型

```python
# 1. 位置参数（最常见，按顺序传递）
def add(a, b):
    return a + b

print(add(10, 20))        # 30


# 2. 默认参数（有默认值的参数必须放在最后）
def introduce(name, age=18, city="北京"):
    print(f"我叫{name}，今年{age}岁，来自{city}")

introduce("小明")                    # 使用默认值
introduce("小红", 20)                # 修改age
introduce("小刚", 22, "上海")        # 全部指定


# 3. keyword 参数（明确指定参数名，顺序可以打乱）
introduce(age=19, name="小芳", city="广州")


# 4. 可变参数
# 4.1 *args：接收任意数量的位置参数（打包成元组）
def sum_all(*args):
    total = 0
    for num in args:
        total += num
    return total

print(sum_all(1, 2, 3))  # 输出: 6
print(sum_all(10, 20, 30, 40, 50))  # 输出: 150


# 4.2 **kwargs：接收任意数量的 keyword 参数（打包成字典）
def show_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

show_info(name="小明", age=18, hobby="编程", city="上海")
```

## 4.3 返回值（return）

```python
# 1. 有返回值的函数
def multiply(x, y):
    return x * y

result = multiply(6, 7)
print("6×7 =", result)


# 2. 返回多个值（实际上返回的是元组）
def get_student_info():
    name = "小红"
    age = 17
    score = 95
    return name, age, score          # 返回元组

# 3. 解包接收返回值
n, a, s = get_student_info()
print(f"姓名：{n}，年龄：{a}，成绩：{s}")


# 4. return 后面可以直接结束函数
def check_age(age):
    if age < 18:
        return "未成年，不能上网吧"
    return "成年，可以上网吧"        # 执行到这里就返回，不再执行下面的代码
```

## 4.4 作用域（LEGB 规则）

```python
x = 100  # Global 全局变量


def outer():
    x = 50  # Enclosing 闭包作用域

    def inner():
        x = 10  # Local 局部变量
        print("inner中x =", x)

    inner()  # 输出: 10
    print("outer中x =", x)


outer()  # 输出: 50
print("全局x =", x)  # 输出: 100


# global 关键字（修改全局变量）
count = 0

def increment():
    global count  # 声明使用全局变量
    count += 1
    print("当前count =", count)


increment()  # 输出: 当前count = 1
increment()  # 输出: 当前count = 2
print("最终count =", count)  # 输出: 最终count = 2
```

## 4.5 匿名函数（lambda）

```python
# lambda 表达式：用于定义简单的一行函数

# 1. 普通函数写法
def square(x):
    return x ** 2

# 2. lambda 写法
# lambda 参数:表达式
square_lambda = lambda x: x ** 2

print(square(5))
print(square_lambda(5))


# 3. 常用场景：配合 sorted、map 等
students = [
    {"name": "小明", "score": 85},
    {"name": "小红", "score": 92},
    {"name": "小刚", "score": 78}
]

# 按成绩从高到低排序
# sorted 函数的 key 指定排序依据
# 默认升序，即 reverse = False
sorted_students = sorted(students, key=lambda s: s["score"], reverse=True)  

print(sorted_students)

# [{'name': '小红', 'score': 92}, {'name': '小明', 'score': 85}, {'name': '小刚', 'score': 78}]
```
如果你想按照多个条件排序，例如，先按照成绩从高到低排序，成绩相同时再按照年龄从小到大排序：
```python
students = [
    {"name": "小明", "score": 85, "age": 20},
    {"name": "小红", "score": 92, "age": 19},
    {"name": "小刚", "score": 85, "age": 18},
]

sorted_students = sorted(students, key=lambda s: (-s["score"], s["age"]))

for student in sorted_students:
    print(student["name"])

# 小红
# 小刚
# 小明
```

## 4.6 高阶函数（map、filter）

```python
# 1. map()：对序列中每个元素应用同一个函数
nums = [1, 2, 3, 4, 5]

squared = list(map(lambda x: x**2, nums))
print("平方结果:", squared)

# 2. filter()：过滤满足条件的元素
even_numbers = list(filter(lambda x: x % 2 == 0, nums))
print("偶数:", even_numbers)
```

## 4.7 递归函数基础

```python
# 递归：函数自己调用自己
# 必须有终止条件（base case），否则会无限递归导致栈溢出

def factorial(n):
    """计算阶乘 n!"""
    if n == 0 or n == 1:        # 终止条件
        return 1
    return n * factorial(n - 1) # 递归调用

print("5的阶乘 =", factorial(5))   # 120
```

# 5. 模块与包、库

## 5.1 模块导入（import、from...import）

```python
# 1. 导入整个模块
import math
print("圆周率 π ≈", math.pi)
print("5的平方根 =", math.sqrt(5))

# 2. 导入模块并起别名（常用）
import math as m
print("sin(30°) =", m.sin(m.radians(30)))

# 3. 从模块中导入特定函数或变量（推荐方式，代码更简洁）
from math import sqrt, pow, pi

print("16的平方根 =", sqrt(16))
print("2的3次方 =", pow(2, 3))
print("π =", pi)

# 4. 导入模块中所有内容（不推荐，容易造成命名冲突）
# from math import *     # 慎用！
```

## 5.2 标准库常用模块

### 5.2.1 math 数学模块

```python
import math

print(math.ceil(3.2))  # 向上取整 → 4
print(math.floor(3.8))  # 向下取整 → 3
print(math.ceil(-3.2))  # 向上取整 → -3
print(math.floor(-3.8))  # 向下取整 → -4
print(math.pow(2, 10))     # 2^10 = 1024.0
print(math.factorial(5))   # 5! = 120
```

### 5.2.2 random 随机数模块

```python
import random

# 1. 随机数
print(random.random())           # 0.0 ~ 1.0 之间的随机浮点数
print(random.randint(1, 10))     # 1~10之间的随机整数（包含两端）
print(random.uniform(1.5, 10.5)) # 指定范围的随机浮点数

# 2. 随机从列表中选择
fruits = ["苹果", "香蕉", "橙子", "葡萄"]
print("今天吃：", random.choice(fruits))

# 3. 随机打乱列表顺序
numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print("打乱后的列表：", numbers)
```

### 5.2.3 datetime 和 time 时间模块

```python
from datetime import datetime, timedelta

# 1. 获取当前时间
now = datetime.now()
print("当前时间:", now)
print("年:", now.year, "月:", now.month, "日:", now.day)
print("格式化输出:", now.strftime("%Y-%m-%d %H:%M:%S"))

# 2. 时间计算
tomorrow = now + timedelta(days=1)
print("明天是:", tomorrow.strftime("%Y-%m-%d"))

# 3. 暂停时间
import time

print("开始等待...")
time.sleep(2)                    # 程序暂停2秒
print("等待结束！")
```

### 5.2.4 json 数据交换格式

```python
import json

student = {
    "name": "小明",
    "age": 18,
    "score": [95, 88, 92],
    "is_student": True
}
# 1. 字典转JSON字符串
# ensure_ascii 控制是否将中文等字符转换成 Unicode 编码，默认为 True
# indent 控制 JSON 的缩进空格数量，默认为 None
json_str = json.dumps(student, ensure_ascii=False, indent=2)
print("JSON字符串：\n", json_str)

# 2. JSON字符串转字典
data = json.loads(json_str)
print("姓名:", data["name"])
```

### 5.2.5 计数器

```python
from collections import Counter

words = ["apple", "banana", "apple", "orange", "banana", "apple"]
counter = Counter(words)
print(
    "单词出现次数:", counter
)  # 单词出现次数: Counter({'apple': 3, 'banana': 2, 'orange': 1})

print("出现最多的单词:", counter.most_common(1))  # 出现最多的单词: [('apple', 3)]
```

## 5.3 包（package）和模块（module）

**包的目录结构示例：**

```
myproject/
├── __init__.py          # 必须有这个文件（Python 3.3+ 可为空）
├── module1.py
├── module2.py
└── utils/
    ├── __init__.py
    └── helper.py
```

**示例代码（module1.py）**

```python
# module1.py
def greet():
    print("我是 module1 中的 greet 函数")
```

**在其他文件中导入包内模块：**

```python
# 主文件 main.py
from myproject.module1 import greet
from myproject.utils.helper import some_function

greet()
```

**`__init__.py` 的作用：**
- 让文件夹成为一个“包”
- 可以控制 `from package import *` 时导入哪些模块
- 可以执行包初始化代码

## 5.4 第三方库安装
**关于 `python -m pip`：**

- `python`：调用 Python 解释器。
- `-m`：让 Python 以模块的方式运行指定模块。
- `pip`：指定运行 pip 模块。

例如，`python -m pip install requests` 表示使用当前 Python 解释器运行 pip，并安装 requests。

**建议优先使用 `python -m pip`，而不是直接使用 `pip`，因为电脑可能安装了多个 Python 版本，直接使用 `pip` 可能调用错误的 Python 环境。**

在 **命令行（终端）** 中执行以下命令（不要在 Python 代码里运行）：
```bash
# 常用命令（推荐使用 python -m pip）
python -m pip install requests             # 安装最新版
python -m pip list                         # 查看已安装的库
python -m pip show requests                # 查看某个库的信息
python -m pip uninstall requests           # 卸载库
```

**在代码中使用第三方库示例（安装后即可使用）：**

```python
# 假设已经安装 requests
import requests

try:
    response = requests.get("https://www.baidu.com", timeout=5)
    print("状态码:", response.status_code)
    print("响应内容前100字符:", response.text[:100])
except Exception as e:
    print("请求失败:", e)
```

## 5.5 相关术语
在 Python 中常见的相关术语：

| 中文   | 英文                  | 说明                   |
| ---- | ------------------- | -------------------- |
| 库    | Library             | 可供程序使用的代码集合          |
| 标准库  | Standard Library    | Python 自带的库          |
| 第三方库 | Third-party Library | 由第三方开发的库             |
| 模块   | Module              | 一个 `.py` 文件，或其他形式的模块 |
| 包    | Package             | 用于组织多个模块的结构          |
| 框架   | Framework           | 提供一套开发结构和规则的工具       |

另外，`pip` 全称通常解释为 **Pip Installs Packages**，是 Python 的包安装管理工具。

注意：日常交流中，Library 和 Package 有时会混用，但严格来说，它们并不是完全相同的概念。

# 6. 面向对象编程（OOP）

## 6.1 类与对象

```python
# 定义一个最简单的类
class Person:
    """这是一个人的类"""
    pass   # 暂时不写内容，用 pass 占位


# 创建对象（实例化）
p1 = Person()   # p1 就是一个对象
p2 = Person()

print(type(p1))        # <class '__main__.Person'>
print(p1)              # 输出内存地址
```

## 6.2 属性与方法

```python
class Person:
    # 类属性（所有实例共享）
    species = "人类"
    
    # 实例方法（必须带 self）
    def eat(self):
        print("我在吃饭～")
    
    def run(self):
        print("我在跑步！")


# 1. 创建对象并调用方法
p = Person()
p.eat()
p.run()

# 2. 给对象动态添加实例属性（不推荐，后面会用 __init__ 统一管理）
p.name = "小明"
p.age = 18
print(p.name, p.age)
```

## 6.3 构造函数 `__init__`

```python
class Person:
    def __init__(self, name, age, gender="男"):
        """构造函数：创建对象时自动调用"""
        self.name = name      # 实例属性
        self.age = age
        self.gender = gender
        print(f"创建了一个人：{name}")

    def introduce(self):
        print(f"大家好，我叫{self.name}，今年{self.age}岁，性别{self.gender}")


# 创建对象时必须传入参数
p1 = Person("小明", 18)
p2 = Person("小红", 17, "女")

p1.introduce()
p2.introduce()
```

## 6.4 实例属性 vs 类属性

```python
class Student:
    # 类属性（属于整个类，所有实例共享）
    school = "第一中学"
    count = 0                 # 统计创建了多少学生
    
    def __init__(self, name):
        self.name = name      # 实例属性（每个学生不同）
        Student.count += 1    # 修改类属性
    
    def show_info(self):
        print(f"姓名：{self.name}，学校：{Student.school}")


s1 = Student("张三")
s2 = Student("李四")

s1.show_info()
s2.show_info()

print("总共创建了", Student.count, "个学生")
print("学校名称：", Student.school)
```

## 6.5 封装、继承、多态

```python
# 1. 封装：把属性和方法封装在类里，通过方法访问
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.__balance = balance      # 双下划线：私有属性（外部无法直接访问）

    def deposit(self, money):
        if money > 0:
            self.__balance += money
            print(f"存入 {money} 元，余额 {self.__balance} 元")
    
    def withdraw(self, money):
        if money > self.__balance:
            print("余额不足！")
        else:
            self.__balance -= money
            print(f"取出 {money} 元，余额 {self.__balance} 元")
    
    # 提供公开方法获取余额（封装思想）
    def get_balance(self):
        return self.__balance


# 2. 继承：子类继承父类的属性和方法
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        print("动物叫声...")


class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)      # 调用父类构造方法
        self.breed = breed
    
    # 重写（override）父类方法 —— 多态
    def speak(self):
        print(f"{self.name} 汪汪汪！我是 {self.breed}")


class Cat(Animal):
    def speak(self):
        print(f"{self.name} 喵喵喵～")


# 3. 多态：同一方法在不同对象上有不同表现
animals = [Dog("旺财", "金毛"), Cat("咪咪")]
for animal in animals:
    animal.speak()          # 同样的方法调用，不同结果
```

## 6.6 魔术方法（特殊方法）

```python
class Book:
    def __init__(self, title, author, pages):
        self.title = title
        self.author = author
        self.pages = pages
    
    def __str__(self):
        """print(对象) 时调用"""
        return f"《{self.title}》 作者：{self.author}，共 {self.pages} 页"
    
    def __len__(self):
        """len(对象) 时调用"""
        return self.pages


book = Book("Python从入门到精通", "小明老师", 520)
print(book)           # 自动调用 __str__
print(len(book))      # 自动调用 __len__
```

## 6.7 静态方法

```python
class MathUtils:

    @staticmethod
    def add(a, b):
        """静态方法：不需要 self，也不依赖类属性"""
        return a + b


print(MathUtils.add(10, 20))        # 直接用类名调用静态方法，无需创建实例
```