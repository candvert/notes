- [python特点](#python特点)
- [命令行使用](#命令行使用)
- [基础](#基础)
	- [变量](#变量)
	- [常量](#常量)
	- [输入输出函数](#输入输出函数)
	- [注释](#注释)
	- [关键字列表](#关键字列表)
	- [import语句](#import语句)
	- [模块](#模块)
	- [字符串](#字符串)
	- [运算符in](#运算符in)
	- [数据类型](#数据类型)
	- [类型转换](#类型转换)
	- [运算符](#运算符)
	- [pass语句](#pass语句)
- [控制流](#控制流)
	- [if语句](#if语句)
	- [for语句](#for语句)
	- [while语句](#while语句)
	- [match语句](#match语句)
- [复合类型](#复合类型)
	- [列表](#列表)
	- [列表生成式](#列表生成式)
	- [字典](#字典)
	- [元组](#元组)
	- [集合](#集合)
- [函数](#函数)
	- [lambda表达式](#lambda表达式)
	- [装饰器](#装饰器)
- [类](#类)
	- [继承](#继承)
- [简洁语法](#简洁语法)
- [异常处理](#异常处理)
- [判断是否是main](#判断是否是main)

## python特点
```python
# python 中一切皆是对象，包括整数，字符串，函数，类等。所有的类都直接或间接地继承自 object 类

# python 有垃圾回收机制。它主要依靠引用计数来管理内存，原理是每个对象都有一个计数器，记录着当前有多少个引用指向它
```
## 命令行使用
```sh
为了不与同时安装的 Python 2.x 冲突，Python 3.x 解释器默认安装的执行文件名不是 python

要在 linux 命令行使用 python 命令，要输入 python3，而不是 python

要在 windows 命令行使用 python 命令，要输入 py，因为输入 python3 会跳转到微软应用商店
```
## 基础
## 变量
```python
a = 5
a, b = 2, 3
a, b = b, a
```
## 常量
```python
# python中没有常量类型，通常使用大写字母表示常量	MAX_NUM = 10
```
## 输入输出函数
```python
input()  # 可以加入提示信息，比如input("hello")
print('hello')


# 参数 end 可以指定其他字符串结尾
print(value..., seq=' ', end='\n', file=None)
```
## 注释
```python
# 单行注释



# python 本身没有专门的多行注释语法，但可以通过使用三个单引号或三个双引号实现
# 这种方式在语法上创建了一个多行字符串对象，只是因为它没有被变量接收，所以通常不会产生实际效果，但并非严格的注释
'''
多行
注释
'''



# 文档字符串是一种特殊的注释，用于为模块、类、函数或方法提供正式的接口说明文档。它使用三对双引号或三对单引号包裹，并放在函数体的最开头
def calculate_area(radius):
    """
    计算给定半径的圆的面积
    参数:
        radius (float): 圆的半径
    返回:
        float: 圆的面积
    """
    return 3.14159 * radius ** 2
```
## 关键字列表
```python
import keyword
# 打印关键字列表
print(keyword.kwlist)



# 关键字列表：
# ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```
## import语句
```python
# 导入模块
import math
math.sqrt(16)


# 从模块中导入特定成员
from math import sqrt, pi  # 只导入 sqrt 函数和 pi 常量
sqrt(9)


# 使用别名
import numpy as np
from math import sqrt as sq


# 导入所有成员
# 这种方式会导入所有不以下划线（_）开头的名称
from math import *
```
## 模块
```python
# 一个 .py 文件就是一个模块，文件名就是模块名
# 包是一个包含 __init__.py 文件（可以是空文件）的目录，用于组织多个相关的模块或子包



# 假设一个处理声音文件与声音数据的包
sound/                          最高层级的包
      __init__.py               初始化 sound 包
      formats/                  用于文件格式转换的子包
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  用于音效的子包
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  用于过滤器的子包
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...


# 从包中导入单个模块，但它必须通过其全名来引用
import sound.effects.echo
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)



# 另一种导入子模块的方法，但它不必加包前缀
from sound.effects import echo
echo.echofilter(input, output, delay=0.7, atten=4)



# 使用绝对导入来引用同级包的子模块
# 如果 sound.filters.vocoder 模块需要使用 sound.effects 包中的 echo 模块：
from sound.effects import echo



# 相对导入
# 对于 surround 模块，可以使用：
from . import echo
from .. import formats
from ..filters import equalizer
```
## 字符串
```python
# python 中没有所谓的 char 类型，单个字符被视为长度为 1 的字符串

# 单引号和双引号都可以表示字符串
s = 'hello'
s = "hello"


# 跨越多行，行结束符会包括在字符串中，但可以通过在行尾添加 \ 来避免此行为
s = '''\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
''')
s = """hello
world"""


# 原始字符串
r'hello'
r"""hello
world"""


# 字符串可以用 + 合并，也可以用 * 重复
s = 3 * 'un' + 'ium' # 'unununium'
# 相邻的两个或多个字符串字面值会自动合并
s = 'Py' 'thon' # 'Python'
text = ('Put several strings within parentheses '
        'to have them joined together.')



# 格式化字符串
year = 2025
s = f'Now is {year}'
print(s)



s = "hello"
s[0]
# 索引还支持负数，用负数索引时，从右边开始计数
s[-1]  # 最后一个字符
s[-2]  # 倒数第二个字符
# 切片，范围为 [)
s[2:4]
s[:4]
s[2:]
s[-2:]  # 从倒数第二个 (含) 到末尾
s[:]
# 索引越界会报错，但是，切片会自动处理越界索引
s[20] # 报错
s[2:20] # 'llo'
# Python 中字符串是不可变的
s[0] = 'J' # 报错
# 还可以指定步长
print(s[0:5:2])  # hlo
print(s[-1:-6:-2])  # olh
# 运算符 in
print("ell" in s)  # True


# 内置函数 len() 返回字符串的长度
len(s)
```
## 运算符in
```python
# 运算符 in 和 not in 用于成员检测。如果 x 是 s 的成员则 x in s 求值为 True，否则为 False
# 所有内置序列和集合类型以及字典都支持此运算，对于字典来说 in 检测其是否有给定的键


# 空字符串总是被视为任何其他字符串的子串
print("" in "abc")  # True
```
## 数据类型
```python
# 数字类型 --- int, float, complex
# 布尔类型 - bool
# 序列类型 --- list, tuple, range
# 文本序列类型 --- str
# 二进制序列类型 --- bytes, bytearray, memoryview
# 集合类型 --- set, frozenset
# 映射类型 --- dict
```
## 类型转换
```python

```
## 运算符
```python
# // 表示整除，因为 python 中 / 返回的是浮点数，所以用 // 表示 c++ 中的整数相除
a = 10//3  # 3

# ** 表示幂运算
a = 2**4  # 16

print(True and False)
print(True or False)
print(not True)
```
## pass语句
```python
# pass 语句不执行任何动作。当语法上需要一个语句，但程序无需执行任何动作时，可以使用该语句
while True:
    pass


# 常用于创建一个最小的类
class MyEmptyClass:
    pass
```
## 控制流
## if语句
```python
# Python 和 C 一样，任何非零整数都为真，零为假
# 任何序列都可以：长度非零就为真，空序列则为假

a = 2
if a == 1:
    print("1")
elif a == 2:
    print("2")
else:
    print("3")
```
## for语句
```python
for i in range(5):
    print(i)
	continue
	break

# range 的范围是左闭右开
range(1, 5)
range(-5, -1)
# 第三个参数为步长
range(0, 5, 2)
range(5, 0, -2)


# 遍历列表
l = [1, 'c']
for item in l:
	pass
# 要按索引迭代序列，可以组合使用 range() 和 len()
for i in range(len(l)):
    print(i, l[i])


# 循环在未执行 break 的情况下结束，else 子句将会执行
for i in range(5):
	print('hello')
	if i = 6:
		break
else:
	print('world')
```
## while语句
```python
while True:
    print("hello")


# 在 while 循环中，else 子句会在循环条件变为假值后执行
a = 3
while a > 0:
	a -= 1
    print("hello")
else:
    print("over")
```
## match语句
```python
# match 语句和 rust 的模式匹配类似


a = 2
match a:
    case 1:
        print("1")
    case 2:
        print("2")
	# 可以用 | 将多个字面值组合到一个模式中
    case 3 | 4 | 5:
        print("3")
    case _:
        print("nopo")



# 形如解包赋值的模式可被用于绑定变量
# point 是一个 (x, y) 元组
match point:
    case (0, 0):
        print("Origin")
    case (0, y):
        print(f"Y={y}")
    case (x, 0):
        print(f"X={x}")
	# 添加 if 作为守卫子句
    case (x, y) if x == y:
        print(f"X={x}, Y={y}")
    case _:
        raise ValueError("Not a point")



# 如果用类组织数据，可以用“类名后接一个参数列表”这种很像构造器的形式，把属性捕获到变量里
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

def where_is(point):
    match point:
        case Point(x=0, y=0):
            print("Origin")
        case Point(x=0, y=y):
            print(f"Y={y}")
        case Point(x=x, y=0):
            print(f"X={x}")
        case Point():
            print("Somewhere else")
        case _:
            print("Not a point")
```
## 复合类型
## 列表
```python
# 列表可以包含不同类型的元素
l = ["hello", 89, 78.3]
del l[1]  # 删除一个元素
del l  # 删除列表

# 和字符串一样，列表也支持切片
l[-3:]

# 列表还支持合并操作
l2 = l + [36, 49, 64, 81, 100]

# 内置函数 len() 返回列表的长度
len(l)

# 嵌套列表
a = ['hi', [0, 1, 2]]
print(a[1][0])

# 空列表
l2 = []

# 列表的方法，下列方法没写参数
list.append()
list.insert()
list.clear()
list.pop()
list.remove()
list.reverse()
list.copy()
```
## 列表生成式
```python
li = [item * item for item in range(0, 10) if item%2 == 0]
print(list)  # [0, 4, 16, 36, 64]
```
## 字典
```python
dict = {"cc": "dd", 87: "cd"}
print(dict)  # {'cc': 'dd', 87: 'cd'}
```
## 元组
```python
tuple = ("hello", 18, 87.45)
print(tuple)  # ('hello', 18, 87.45)
```
## 集合
```python
arr = {10, 20, 30}
arr2 = {15, 20, 25}
print(arr & arr2)  # 交集
print(arr | arr2)  # 并集


# 集合的方法，下列方法没写参数
arr.add()
arr.remove()
arr.clear()
```
## 函数
```python
# 函数内的第一条语句是字符串时，该字符串就是文档字符串，也称为 docstring
# 即使没有 return 语句的函数也有返回值，这个值被称为 None (是一个内置名称)
# return 语句不带表达式参数时，返回 None
def say(n):
	'''Print a Fibonacci series less than n.'''
	print(n)
	return 'hi'


# python 指不指定返回类型都可以，指定返回类型的写法：
def say(n) -> str:
	return 'hi'


# 默认值参数
def ask_ok(prompt, retries=4, reminder='Please try again!'):
	pass
# kwarg=value 形式被称为关键字参数
# 函数调用时，关键字参数必须跟在位置参数后面
ask_ok(prompt='hi', reminder='what') # 2 个关键字参数
parrot('hi', 20, 'what') # 3 个位置参数


# 最后一个形参为 **name 形式时，接收一个字典
# 该字典包含与函数中已定义形参对应之外的所有关键字参数
# **name 形参可以与 *name 形参组合使用（*name 必须在 **name 前面）， *name 形参接收一个元组，该元组包含形参列表之外的位置参数
def cheeseshop(kind, *arguments, **keywords):
	pass
# 可以这样调用
cheeseshop("a", "b", "c", server="d", client="e", animal="f")



# 函数声明形式，python3.8 引入
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
      -----------    ----------     ----------
        |             |                  |
        |        位置或关键字   |
        |                                - 仅限关键字
         -- 仅限位置
# 比如该函数：
def create_user(id, name, /, age, *, city, email):
	pass
# 正确调用方式：
create_user(1001, "Alice", 25, city="Beijing", email="alice@example.com")
create_user(1002, "Bob", age=30, city="Shanghai", email="bob@example.com")
create_user(1003, "John", 28, email="John@example.com", city="Guangzhou")


def calculate(num1, num2):
    return num1 + num2
print(calculate(3, 5))  # 8
print(calculate(num2=3, num1=5))  # 8 也就是kotlin中的命名参数，在python中交关键字参数

# 和c++一样，有默认值的参数要在无默认值的参数之后
def kjk(a=10,b):	# 该定义是错误的
    pass

# 可变参数
def calculate(*args):
    print(args)		# 打印(1, 2, 3)
calculate(1, 2, 3)

# 关键字可变参数，kwargs是一个字典
def calculate(**kwargs):
    print(kwargs)	# 打印{'num1': 1, 'num2': 2, 'num3': 3}
calculate(num1=1, num2=2, num3=3)

# 匿名函数，python中匿名函数只能包含单个表达式
a = lambda name, age: name + age
b = lambda: print('hi')		# 没有参数
```
## lambda表达式
```python
# lambda 参数列表: 表达式
# 参数列表：可以包含零个、一个或多个参数，多个参数之间用逗号分隔
# 表达式：这是一个单一的 python 表达式（不能是复杂的代码块或多个语句），其计算结果将作为 lambda 表达式的返回值


f = lambda: 'hello'
f2 = lambda x, y: x + y
f2(1, 2)
```
## 函数的作用域
```python
b = 6
def f1(a):
	print(a)
	print(b)
	b = 9

# 该函数调用会报错
# Python 编译函数的定义体时，它判断 b 是局部变量，因为在函数中给它赋值了
f1(3) # 报错




# 如果在函数中赋值时想让解释器把 b 当成全局变量，要使用 global 声明
b = 6
def f1(a):
	global b
	print(a)
	print(b)
	b = 9
```
## 闭包
```python
# 闭包的核心特征是“能够捕获并记住其创建时所在词法作用域中的变量”
# 其实，闭包指延伸了作用域的函数，函数是不是匿名的没有关系，关键是它能访问定义体之外定义的非全局变量


# nonlocal 用于在嵌套函数内部声明并修改外层（非全局）函数变量
def make_averager():
	 count = 0
	 total = 0
	 def averager(new_value):
		 nonlocal count, total
		 count += 1
		 total += new_value
		 return total / count
	 return averager

avg = make_averager()
print(avg(10))
print(avg(11))
```
## 装饰器
```python
# 装饰器是可调用的对象，其参数是另一个函数（被装饰的函数）
# 装饰器在被装饰的函数定义之后立即运行，通常是在导入时（即 Python 加载模块时）
# 装饰器最好通过实现 __call__ 方法的类实现，下面的函数实现用于说明原理


# 假如有个名为 decorate 的装饰器：
@decorate
def target():
	print('running target()')
# 上述代码的效果与下述写法一样：
def target():
	print('running target()')
target = decorate(target)
# 两种写法的最终结果一样：上述两个代码片段执行完毕后得到的 target 不一定是原来那个target 函数，而是 decorate(target) 返回的函数


# 严格来说，装饰器只是语法糖
# 示例：
>>> def deco(func):
...     def inner():
...         print('running inner()')
...     return inner
...
>>> @deco
... def target():
...     print('running target()')
...
>>> target()
running inner()
>>> target
<function deco.<locals>.inner at 0x10063b598>



# 叠放装饰器
@d1
@d2
def f():
	print('f')
# 等同于：
def f():
	print('f')
f = d1(d2(f))
```
## 类
```python
# 支持面向对象编程的所有标准特性：类的继承机制支持多个基类、派生的类能覆盖基类的方法、类的方法能调用基类中的同名方法
# 所有成员函数都为 virtual
# “私有”实例变量在 Python 中并不存在。但是，大多数 Python 代码都遵循这样一个约定：带有一个下划线的名称 (例如 _spam) 应该被当作是 API 的非公有部分 (无论它是函数、方法或是数据成员)



class Student:
    pass

# 定义对象
s = Student()


# 当进入类定义时，将创建一个新的命名空间，并将其用作局部作用域
# 当 (从结尾处) 正常离开类定义时，将创建一个类对象。这基本上是一个围绕类定义所创建的命名空间的包装器。 原始的 (在进入类定义之前有效的) 作用域将重新生效，类对象将在这里与类定义头所给出的类名称进行绑定
class Student:
    """一个简单的示例类"""
    i = 12345
	# 方法的特殊之处就在于实例对象会作为函数的第一个参数被传入
    def f(self):
        return 'hello world'

# 属性引用
Student.i = 20
f2 = Student.f
# __doc__ 也是一个有效的属性，将返回所属类的文档字符串
print(Student.__doc__) # 一个简单的示例类




# __init__ 函数和 c++ 的构造函数相同
# 参数 self 是必须的，并且要位于其他参数的前面
class Student:
    def __init__(self, name):
		self.name = name
        pass
# 数据属性无需声明；与局部变量一样，它们在首次赋值时就会出现
x = Student('John')
x.counter = 1
del x.counter



# 实例变量用于每个实例的唯一数据，而类变量用于类的所有实例共享的属性和方法
class Dog:

    kind = 'canine'         # 类变量被所有实例所共享

    def __init__(self, name):
        self.name = name    # 实例变量为每个实例所独有



# 方法可以通过使用 self 参数调用其他方法
class Bag:
    def __init__(self):
        self.data = []

    def add(self, x):
        self.data.append(x)

    def addtwice(self, x):
        self.add(x)
        self.add(x)



# classmethod 装饰器用于定义类方法
# 按照约定，类方法的第一个参数名为 cls（但是 Python 不介意具体怎么命名）
# staticmethod 装饰器用于定义静态函数
class Demo:
	@classmethod
	def klassmeth(cls, name):
		return name
	@staticmethod
	def statmeth(*args):
		return args



# 在 Python 中，除了 __init__，类里还有许多以双下划线开头和结尾的特殊方法（常被称为“魔法方法”或“双下方法”），它们用于实现类的特定行为
# __call__(self, ...)：让类的实例可以像函数一样被“调用”，例如 obj()
# __iter__(self)：应返回一个迭代器对象。通常返回 self，但需要类同时实现 __next__方法
# __next__(self)：定义迭代器下一次返回的值。当没有更多元素时，应抛出 StopIteration 异常
# __str__(self)：当你使用 print(obj)或 str(obj)时，该方法被调用
# __add__(self, other)：实现加法 +
# __len__(self)：当使用内置函数 len(obj)时被调用，应返回容器的“长度”
# __getitem__(self, key)：定义通过索引或键访问元素的行为，对应 obj[key]
# __setitem__(self, key, value)：定义通过索引或键赋值的行为，对应 obj[key] = value
# __delitem__(self, key)：定义通过索引或键删除元素的行为，对应 del obj[key]
# __contains__(self, item)：定义使用 in 运算符进行成员测试时的行为

```
## 继承
```python
# 继承，在类名后面的括号中写继承的类
class Animal:
    def eat(self):
        print("super eat")


class Dog(Animal):
	pass
# 调用父类的方法通过使用 super().method()
class Dog(Animal):
    def __init__(self):
		super().init()
    def eat(self):
        super().eat()
        print("dog eat")


# 多重继承
class Dog(Animal, Pet):
    pass
```
## dataclass
```python
# 类似于 C 语言 struct 的数据类型

from dataclasses import dataclass

@dataclass
class Employee:
    name: str
    dept: str
    salary: int
```
## 简洁语法
```python
def f1:
	pass
def f2:
	pass
promos = [f1, f2]
max(promo(order) for promo in promos)
```
## 异常处理
```python
# 打印hello
try:
    5/0
except ZeroDivisionError:
    print("hello")

# 打印world，else语句后的是如果没有出现异常就执行的语句
try:
    5/1
except ZeroDivisionError as e:
    print("hello")
else:
    print("world")
    
# 打印world，finally语句后的是无论如何都执行的语句
try:
    5/1
except ZeroDivisionError as e:
    print("hello")
else:
    print("world")
finally:
    print("end")
    
# 通过raise关键字抛出异常
try:
    raise Exception("exception")
except Exception as e:
    pass

# except直接跟分号表示捕获所有异常
try:
    raise Exception("exception")
except:
    pass

# 捕获多个异常
try:
    raise Exception("exception")
except (ZeroDivisionError, RuntimeError) as e:
    pass
```
## 判断是否是main
```python
# 当该 .py 文件被直接运行时，__name__ 变量的值会被设置为 '__main__'，于是if条件下的代码块会被执行
# 当该 .py 文件被作为模块导入到其他程序时，__name__ 变量的值会是模块的名字，if条件下的代码块则不会被执行，从而避免了测试代码在导入时意外运行


if __name__ == '__main__':
    pass
```