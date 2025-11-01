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
- [类](#类)
	- [继承](#继承)
- [异常处理](#异常处理)
- [判断是否是main](#判断是否是main)

## python特点
```python
# python 中一切皆是对象，包括整数，字符串，函数等。所有的类都直接或间接地继承自 object 类

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

# 可以嵌套列表
a = ['a', 'b', 'c']
n = [1, 2, 3]
x = [a, n] # [['a', 'b', 'c'], [1, 2, 3]]

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
f = lambda x: x + 1
f()
```
## 类
```python
class Student:
    pass

# 定义对象
s = Student()


# __init__ 函数和 c++ 的构造函数相同
# 可以在类中通过 @staticmethod 和 @classmethod 定义方法，不加注解的方法必须要有一个 self 参数
class Student:
    a = 'a'
	# 参数 self 是必须的，并且要位于其他参数的前面
    def __init__(self, name):
		self.name = name
        pass

    def ni(self):
        pass

    @staticmethod
    def ok():
        pass

    @classmethod
    def cdd(cls):
        pass
    
# 因为python是动态语言，所以可以动态的添加属性和方法，下面代码添加一个 age 属性
a = Student()
a.age = 18
# 下面代码添加一个 say 方法并进行调用
a = Student()
a.say = lambda: print("eat")
a.say()
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
        
# 多继承
class Dog(Animal, Object):
    pass
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