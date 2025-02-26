- [定义变量](#定义变量)
- [输入输出函数](#输入输出函数)
- [注释](#注释)
- [关键字列表](#关键字列表)
- [字符串操作](#字符串操作)
- [数据类型和类型转换](#数据类型和类型转换)
- [算数运算和逻辑运算符](#算数运算和逻辑运算符)
- [if语句](#if语句)
- [match语句(c++中的switch)](#match语句(c++中的switch))
- [for语句](#for语句)
- [while语句](#while语句)
- [pass语句](#pass语句)
- [列表](#列表)
- [列表生成式](#列表生成式)
- [元组](#元组)
- [字典](#字典)
- [集合](#集合)
- [格式化输出](#格式化输出)
- [异常处理](#异常处理)
- [函数](#函数)
- [类](#类)
	- [类中的self](#类中的self)
- [assert](#assert)
- [模块](#模块)
- [判断是否是main](#判断是否是main)
- [打包程序](#打包程序)
- [`__file__`等双下划线变量](#`__file__`等双下划线变量)
- [使用切片将列表传入函数以实现拷贝而非引用](#使用切片将列表传入函数以实现拷贝而非引用)
- [内置函数](#内置函数)

# Python基本语法

python中一切皆是对象，包括函数、类、模块都是对象

```python
# 解构，只要是可迭代的都可解构，但数量必须匹配
li = [2, 'hello', 2.7]
a, _, b = li
# 可以使用*表示任意数量变量，*的变量一定是一个列表，即使没有元素
record = ('Dave', 'dave@example.com', '773-555-1212', '847-555-1212')
name, email, *phone_numbers = record
```
## 定义变量
```python
# 定义变量a为5
a=5
a, b = 2, 3		# a的值为2，b的值为3
# python中没有常量类型，通常使用大写字母表示常量	MAX_NUM = 10
```
## 输入输出函数
```python
input()  # 可以加入提示信息，比如input("hello")
print(value..., seq=' ', end='\n', file=None)
```
## 注释
```python
# 单行注释

'''
多行
注释
'''

"""
多行注释
"""
```
## 关键字列表
```python
import keyword

print(keyword.kwlist)
```
## 字符串操作
```python
str = "你好"
print(str + str)  # 你好你好
print(str * 3)  # 你好你好
print("你" in str)  # True
print(str[0:1])  # 你
print(str[:1])  # 你
print(str[0:])  # 你好
print(str[-2:-1])  # 你

# 切片格式[start:end:step]
str = "hello"
print(str[0:5:2])  # hlo
print(str[::2])  # hlo

print(str.index("o"))  # 4
print(str.count("l"))  # 2
```
## 数据类型和类型转换
```python
# python中的类型：str, int, float, complex, list, tuple, dict, set, bool

a = int("38")
print(a)  # 38
```
## 算数运算和逻辑运算符
```python
a = 10//3  # //代表整除，因为python中/返回的是浮点数，所以用//代表c++中的整数相除
print(a)  # 3

a = 2**4  # **代表幂运算
print(a)  # 16

print(True and False)  # False，and相当于c++中&&
print(True or False)  # True，or相当于c++中||
print(not True)  # False，not相当于c++中!
```
## if语句
```python
# 不像c++中的if，python中if后的条件不用加括号
a = 2
if a == 1:
    print("1")
elif a == 2:
    print("2")
else:
    print("3")
```
## match语句(c++中的switch)
```python
# case _相当于c++中的default
a = 2
match a:
    case 1:
        print("1")
    case 2:
        print("2")
    case 3:
        print("3")
    case _:
        print("nopo")
```
## for语句
```python
# range的范围是左闭右开
sum = 0
for i in range(1,4):
    sum += i
print(sum)  # 6
```
## while语句
```python
while True:
    print("hello")
```
## pass语句
```python
# pass起到占位符的作用
while True:
    pass
```
## 列表
```python
list = ["hello", 89, 78.3]
del list[1]  # 删除一个元素
del list  # 删除列表

# 空列表
li = list()
li = []

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
## 元组
```python
tuple = ("hello", 18, 87.45)
print(tuple)  # ('hello', 18, 87.45)
```
## 字典
```python
dict = {"cc": "dd", 87: "cd"}
print(dict)  # {'cc': 'dd', 87: 'cd'}
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
## 格式化输出
```python
name = "John"
age = 18
print("name=%s,age=%d" % (name, age))		# name=John,age=18
print(f"name={name},age={age}")				# name=John,age=18
print("name={0},age={1}".format(name,age))	# name=John,age=18
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
## 函数
```python
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
## 类
```python
class Teacher():
    pass

class Student:
    pass

# 定义对象
s = Student()

# __init__函数和c++的构造函数相同
# 参数self是必须的，它指代的是调用函数时传递的实例
# 可以在类中通过@staticmethod和@classmethod定义方法，不加注解的方法必须要有一个self参数
class Student:
    a = 'a'
    def __init__(self):
        pass

    def ni(self):
        pass

    @staticmethod
    def ok():
        pass

    @classmethod
    def cdd(cls):
        pass
    
# 因为python是动态语言，所以可以动态的添加属性和方法，下面代码添加一个name属性
a = Student()
a.name = 'John'
# 下面代码添加一个say方法并进行调用
a = Student()
a.say = lambda: print("eat")
a.say()


# 继承，在类名后面的括号中写继承的类
class Animal:
    def eat(self):
        print("super eat")
        
class Dog(Animal):
	pass
# 调用父类的方法通过使用super().method()
class Dog(Animal):
    def eat(self):
        super().eat()
        print("dog eat")
        
# 多继承
class Dog(Animal, Object):
    pass
```
### 类中的self
```python
# a是一个局部变量，只能在__init__函数中访问，b可以在其他函数中访问
class MyWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        a = 10
        self.b = 10
        
    def ok(self):
		a = 11    # 错误，不能访问a
		b = 11    # 正确，可以访问b
```
## assert
```python
# assert关键字后跟表达式，条件为False抛出AssertionError
assert 10 < 1
```
## 模块
```python
# 导入语法
import 模块名
import 模块名 as 别名
from 模块名 import 类，变量，方法
from 模块名 import *
from 模块名 import 功能名 as 别名

# python中文件名就是模块名
# python中的包必须包含一个__init__.py文件，其可以为空
# 如果如下目录，那么在main.py中导入cal.py模块的语法就是from calculate import cal
# -main.py
# -calculate
# 	-__init__.py
#   -cal.py
```
## 工具函数
```python
# 文件操作
# 打开文件，open(name, mode=None, buffering=None)
file = open('D:\\file', 'r', encoding='utf-8')
# 读取文件内容
file.read(num)	# num是读几个字节
file.readln()
# 关闭文件
file.close()
# with open语句，可以自动关闭文件
with open('D:\\file', 'r') as f:
    print(f.readline())
# 写入内容
file.wirte('hello')


# 获取当前时间
from datetime import datetime
a = datetime.now().strftime('%Y-%m-%d %H:%M:%S')	# a的类型为str
b = datetime.now()									# b的类型为datetime
print(a)	# 2024-10-02 19:15:38
print(b)	# 2024-10-02 19:15:38.149608


# 休眠
from time import sleep
sleep(2)	# 休眠2秒


# 多线程
import threading


# 随机数
import random
a = random.randint(1, 10)	# [1, 10]
a = random.random()			# [0, 1)


# 正则表达式
import re
```
## 判断是否是main
```python
if __name__ == '__main__':
    pass
```
## 打包程序
```python
# 使用pyinstaller打包程序
pip install pyinstaller
```
## `__file__`等双下划线变量
```python
# 常见的双下划线变量
# 	1. __file__:
# 		返回当前模块的文件路径。用于获取模块所在的文件位置。
# 	2. __name__:
# 		表示模块的名称。当模块被直接运行时，其值为 "__main__"；当被导入时，值为模块的名称。
# 	3. __init__:
# 		构造方法，用于初始化类的实例。
# 	4. __dict__:
# 		返回类的属性字典，包含所有可用的属性和方法。
# 	5. __class__:
# 		返回实例的类。
```
## 使用切片将列表传入函数以实现拷贝而非引用
## 内置函数
```python
num = 42
# id返回唯一标识符，通常是内存地址
id(num)
sum([3, 7])				# 10
sum((2, 4))				# 6
len([])
round(1.23, 1)			# 1
round(23, -1)			# 20
format(1.23, '0.1f')	# '1.2'
'value is {:0.1f}'.format(1.23)
hex(1234)				# '0x4d2'
```
