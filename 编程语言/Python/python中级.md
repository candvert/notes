- [python创建简单网页服务器](#python创建简单网页服务器)

- [添加包](#添加包)
- [dir函数](#dir函数)
- [functools.wraps](#functools.wraps)


- [pathlib](#pathlib)


- [python创建用于测试的http服务器](#python创建用于测试的http服务器)
- [python的内置api和一些三方库的api](#python的内置api和一些三方库的api)
	- [列表](#列表)
	- [元组](#元组)
	- [字典](#字典)
	- [集合](#集合)
	- [enumerate进行遍历](#enumerate进行遍历)
	- [json](#json)
	- [unittest测试（内置测试框架）](#unittest测试（内置测试框架）)
	- [文件路径操作](#文件路径操作)
	- [文件读写操作](#文件读写操作)
	- [面向对象的文件操作pathlib](#面向对象的文件操作pathlib)
	- [获取时间和sleep函数](#获取时间和sleep函数)
	- [随机数](#随机数)
	- [多线程](#多线程)
	- [正则表达式](#正则表达式)
	- [网络socket](#网络socket)
	- [sqlite](#sqlite)
	- [mysql](#mysql)
	- [网络库requests](#网络库requests)

	- [os模块](#os模块)

## python创建简单网页服务器
进入目标目录，在命令行输入：
```python
# 浏览器会默认显示目录下的 index.html 文件
# 没有 index.html 文件，则会以文件列表的形式显示目录下的所有文件和文件夹
python -m http.server 8880
```

## 添加包
```sh
# windows 上的默认安装目录：
# C:\Users\leiyu\AppData\Local\Programs\Python\Python313\Lib\site-packages


# 安装包
pip install package_name
# 卸载包
pip uninstall package_name
# 列出包
pip list
```
## dir函数
```python
# 内置函数 dir() 用于查找模块定义的名称。返回结果是经过排序的字符串列表
import sys
dir(sys)


# 没有参数时，dir() 列出当前已定义的名称
dir()
```
## functools.wraps
```python
# 当使用装饰器包装函数时，实际上是用装饰器内部的包装函数（通常命名为 wrapper）替换了原函数。这会导致原函数的元数据（如 __name__、__doc__等）被包装函数的元数据覆盖
# functools.wraps是一个装饰器，它能将原函数的重要元数据复制到装饰器返回的包装函数上。使用时，只需用 @wraps(原函数名)来装饰你的包装函数

from functools import wraps

def my_decorator(func):
    @wraps(func)  # 使用 wraps 保留原函数元数据
    def wrapper(*args, **kwargs):
        """包装函数的文档字符串"""
        print("函数被调用了")
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def example():
    """这是原函数的文档字符串"""
    print("Hello, World!")

print(example.__name__)  # 输出: example
print(example.__doc__)   # 输出: 这是原函数的文档字符串
```
## pathlib
```python
from pathlib import Path

# 创建路径对象
path = Path('/home/user/documents')

# 路径拼接
file_path = path / 'subfolder' / 'file.txt'

# 获取各部分信息
print(file_path.name)        # 'file.txt'
print(file_path.parent)      # '/home/user/documents/subfolder'
print(file_path.suffix)      # '.txt'
print(file_path.stem)        # 'file'

# 路径操作
new_path = file_path.with_name('new_file.txt')
new_suffix = file_path.with_suffix('.pdf')

# 检查存在性
if file_path.exists():
    print("文件存在")

# 遍历目录
for item in path.iterdir():
    print(item)
```
常用操作示例：
```python
from pathlib import Path

# 获取当前工作目录
current_dir = Path.cwd()

# 家目录
home_dir = Path.home()

# 相对路径转绝对路径
abs_path = Path('relative/path').resolve()

# 创建目录
Path('new_folder').mkdir(exist_ok=True)

# 读取文件内容
content = Path('file.txt').read_text()

# 写入文件
Path('output.txt').write_text('Hello World')
```
跨平台路径处理：
```python
from pathlib import Path

# pathlib 自动处理不同操作系统的路径分隔符
path = Path('folder') / 'subfolder' / 'file.txt'

# 在Windows上会使用 \，在Unix系统上会使用 /
```

**建议**：对于新项目，优先使用 `pathlib`，它提供了更面向对象和直观的API，代码更易读和维护
## 使用functools.lru_cache做备忘
```python
# 它把耗时的函数的结果保存起来，避免传入相同的参数时重复计算

import functools
from clockdeco import clock

@functools.lru_cache()
@clock
def fibonacci(n):
	if n < 2:
		return n
	return fibonacci(n-2) + fibonacci(n-1)

print(fibonacci(6))




# lru_cache 可以使用两个可选的参数来配置
# maxsize 参数指定存储多少个调用的结果。缓存满了之后，旧的结果会被扔掉
functools.lru_cache(maxsize=128, typed=False)
```
# python创建用于测试的http服务器
```sh
mkdir test_server && cd test_server
echo {"good": true} >> ok.json
python -m http.server 8000

然后访问 http://localhost:8000 即可看到页面
```
# python的内置api和一些三方库的api
## 列表
```python
name = ['john', 'smith', 83.4, 'joey', 42]		# name的类型为name: list[str | float | int]

# 元素首字母大写
name[0].title()				# John
# 添加元素
name.append('ross')			# 返回值/None
name.insert(0, 'ross')		# 参数1/索引；参数2/值；返回值/None
# 删除索引对应的元素
del name[0]
# 删除元素
name.pop()				# 返回值/尾部元素
name.pop(2)				# 返回值/索引对应元素
# 根据值删除元素
name.remove('joey')		# 返回值/None

name = [83.4, 42, 23, 89]
# 排序列表
name.sort()				# 返回值/None
# 临时排序，不改变原有列表
sorted(name)			# 返回值/新列表
# 反转列表中元素的顺序
name.reverse()			# 返回值/None
print(name)				# [89, 23, 42, 83.4]
# 求列表长度
len(name)				# 返回值/int
# 求列表中最大值，最小值，和
max(name)
min(name)
sum(name)

# 遍历列表
for n in name:
    print(n)

# 用range()生成列表
li = list(range(1, 10))		# range范围为[)
# 用列表解析生成列表
li = [item * item for item in range(0, 10) if item%2 == 0]

# 切片
name[ : ]
name[ : : ]
```
## 元组
```python
words = ('good', 42, 'human', 'profit')		# words类型为words: tuple[str, int, str, str]，因为元组不变，所以类型固定
```
## 字典
```python
config = {'name': 'John', 'age': 18, 1.0: 'one'}	# config的类型为config: dict[str | float, str | int]

# 访问字典中的值
config['name']				# 返回值/键对应的值
config.get('mail', "mail doesn't exist")	# 参数1/键；参数2/键不存在时返回的值
# 添加键值对
config['hobby'] = 'swim'
# 删除键值对
del config['age']

# 遍历字典的键值对
for key, value in config.items():
    print(config[key])
# 遍历字典的键
for key in config.keys():
    print(key)
# 遍历字典的值
for value in config.values():
    print(value)
```
## 集合
```python
# 集合不像列表和字典，集合不会以特定的顺序存储元素
words = {'good', 42, 'human', 'profit'}		# words的类型为words: set[str | int]
config = {'name': 'John', 'age': 18, 1.0: 'one'}
words = set(config.keys())
words = set(config.values())
```
## enumerate进行遍历
```python
name = ['john', 'joey']
for index, value in enumerate(name):	# enumerate()返回下标和对应的值
    print(index)
    print(value)
```
## json
```python
# json.dump()将字典保存到文件中
import json

config = {'name': 'John', 'age': 18, 1.0: 'one'}
file = open('D:\\py.json', 'w')
json.dump(config, file)				# 使用json.dump()将字典保存到文件中
json.dump(config, file, indent=4)	 # 使用indent格式化输出


# json.load()从文件中读取数据
import json

file = open('D:\\py.json', 'r')
data = json.load(file)


# json.dumps()将字典转为字符串并用json.loads()将字符串转为字典
import json

config = {'name': 'John', 'age': 18, 1.0: 'one'}
a = json.dumps(config)		# 返回值/str
b = json.loads(a)			# 返回值/Any
```
## unittest测试（内置测试框架）
```python
# 测试用例是通过继承 unittest.TestCase 类创建的。你可以在类中定义多个测试方法，测试方法的名称必须以 test_ 开头
# 在 unittest 框架中，setUp 方法用于在每个测试方法执行之前设置测试环境。它可以用于初始化测试中需要的资源、创建对象、打开文件
# 或建立数据库连接等
import unittest

def eat():
    return 3

class EatTestCase(unittest.TestCase):
    def setUp(self):
        self.instance = EatTestCase()

    def test_eat_function(self):
        self.assertEqual(3, eat())

if __name__ == '__main__':
    unittest.main()
```
## 面向对象的文件操作pathlib
```python
from pathlib import Path
Path.cwd()
Path.is_dir()
Path.is_file()
Path.exists()
Path.absolute()
Path.resolve()
Path.stat()
Path.lstat()
Path.iterdir()
Path.walk()
Path.rename()
Path.replace()
Path.rmdir()
p = Path(r'D:\dir\old_file.txt')
# old_file.txt
p.name
# old_file
p.stem
# .txt
p.suffix
# D:\dir
p.parent
p.exists()
# D:\dir\old_file.txt
p.absolute()
p.is_absolute()
# D:\dir\old_file.txt
p.resolve()
p.stat()
p.is_file()
p.is_dir()
p.open()
p.read_text()
p.iterdir()
p.rename(r'D:\file.txt')
p = Path(r'D:\new_file.txt')
p.touch()
p = Path(r'D:\new_dir')
p.mkdir()
p.unlink() # 移除文件
p.rmdir()
p = Path(r'D:\dir')
q = p / "new_file.txt"
# new_file.txt
q.name
```
## 文件路径操作
```python
import os
# 打开文件，open(name, mode=None, buffering=None)
file = open('D:\\file', 'r', encoding='utf-8')
# 读取文件内容
file.read(num)	# num是读几个字节
file.readln()
# 关闭文件
file.close()
# with open语句，可以自动关闭文件
with open('D:\\file', 'r', encoding='utf-8') as f:
    print(f.readline())
# 写入内容
file.wirte('hello')
```
## 文件读写操作
```python
# 列出目录中所有文件
list = os.listdir(path='.')
# 文件重命名
os.rename(src, dst, *, src_dir_fd=None, dst_dir_fd=None)
# 判断是文件还是文件夹
os.path.isfile(path)
os.path.isdir(path, /)
# 获取文件最后修改时间
os.path.getmtime(path, /)
# 分离文件名和扩展名，file_extension以句点开头或为空
file_base_name, file_extension = os.path.splitext(path, /)

os.mkdir(path, mode=0o777, *, dir_fd=None)

os.makedirs(name, mode=0o777, exist_ok=False)

os.rmdir(path, *, dir_fd=None)

os.path.getsize(path, /)

os.path.join(path, /, *paths)

os.walk(top, topdown=True, onerror=None, followlinks=False)
```
## 获取时间和sleep函数
```python
# 获取当前时间
from datetime import datetime
a = datetime.now().strftime('%Y-%m-%d %H:%M:%S')	# a的类型为str
b = datetime.now()									# b的类型为datetime
print(a)	# 2024-10-02 19:15:38
print(b)	# 2024-10-02 19:15:38.149608

# 休眠
from time import sleep
sleep(2)	# 休眠2秒
```
## 随机数
```python
import random
a = random.randint(1, 10)	# [1, 10]
a = random.random()			# [0, 1)
```
## 多线程
```python
# threading.Lock()创建锁，threading.Thread(target=函数, args=(参数))创建线程
import threading

lock = threading.Lock()

def thread_function(name):
    with lock:  # 使用锁
        print(f"线程 {name} 开始运行")
        # 在这里添加你的代码
        print(f"线程 {name} 结束运行")

threads = []
for i in range(5):
    thread = threading.Thread(target=thread_function, args=(i,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()  # 等待所有线程完成
```
## 正则表达式
```python
# 使用re.match(pattern, string)进行匹配
import re

res = re.match('h.s', 'his you')
```
## 网络socket
```python
# 服务器端
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(('localhost', 8800))
server_socket.listen(5)

while True:
    client_socket, addr = server_socket.accept()
    client_socket.send(b'Hello')
    client_socket.close()

# 客户端
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('localhost', 8800))
response = client_socket.recv(1024)		# 应该使用response.decode('utf-8')将结果转为str
client_socket.close()

# 向百度发送请求
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

client_socket.connect(('www.baidu.com', 80))  # 80 是 HTTP 默认端口

request = "GET / HTTP/1.1\r\nHost: www.baidu.com\r\nConnection: close\r\n\r\n"
client_socket.send(request.encode('utf-8'))  # 发送请求

response = b""
while True:
    data = client_socket.recv(1024)  # 持续接收数据
    if not data:
        break
    response += data

print(response.decode('utf-8'))  # 解码并打印响应

client_socket.close()
```
## sqlite
```python
# 简短示例
import sqlite3

name1 = 'john'
name2 = 'alice'
age1 = 18
age2 = 15
sex1 = 'male'
sex2 = 'female'
conn = sqlite3.connect('student.db')
cursor = conn.cursor()
cursor.execute('CREATE TABLE student(name, age, sex)')
cursor.executemany('INSERT INTO student VALUES(?, ?, ?)', [(name1, age1, sex1), (name2, age2, sex2)])
cursor.execute('UPDATE student SET age=? WHERE name="alice"', (16,))
cursor.execute('DELETE FROM student WHERE name=?', (name2,))
res = cursor.execute('SELECT * FROM student')
conn.commit()
print(res.fetchall())


# 导入sqlite3
import sqlite3

# 连接到 SQLite 数据库
conn = sqlite3.connect('example.db')
cursor = conn.cursor()

# 创建表
cursor.execute('''
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER
)
''')

# 插入数据
cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', ('Alice', 30))
cursor.executemany('INSERT INTO users (name, age) VALUES (?, ?)', [
    ('Bob', 25),
    ('Charlie', 35)
])
conn.commit()

# 查询数据
cursor.execute('SELECT * FROM users')
rows = cursor.fetchall()
for row in rows:
    print(row)

# 更新数据
cursor.execute('UPDATE users SET age = ? WHERE name = ?', (31, 'Alice'))
conn.commit()

# 删除数据
cursor.execute('DELETE FROM users WHERE name = ?', ('Bob',))
conn.commit()

# 关闭 cursor 和连接
cursor.close()
conn.close()
```
## mysql
```python
from pymysql.connections import Connection

conn = Connection(
    host='localhost',
    port=3306,
    user='root',
    password=''
)
cursor = conn.cursor()		# 获取数据库指针以执行sql语句
conn.select_db('database')
cursor.execute('SELECT * FROM table')	# 执行sql语句
results: tuple = cursor.fetchall()	# 获取结果
conn.close()
```
## 网络库requests
```python
r = requests.get('https://api.github.com/events')
r = requests.post('https://httpbin.org/post', data={'key': 'value'})
r = requests.get(url, cookies=cookies)
r.headers
r.headers['Content-Type']
r.content
r.status_code

# get函数
def get(url: str | bytes,
        params: Any | None = None,
        *,
        data: Any = ...,
        headers: Mapping[str, str | bytes | None] | None = ...,
        cookies: Any = ...,
        files: Any = ...,
        auth: Any = ...,
        timeout: Any = ...,
        allow_redirects: bool = ...,
        proxies: Any = ...,
        hooks: Any = ...,
        stream: bool | None = ...,
        verify: Any = ...,
        cert: Any = ...,
        json: Any | None = ...) -> Response

# Requests中明确引发的所有异常都继承自 request.exceptions.RequestException。

# 将内容存到文件中
import json
import requests

r = requests.get('https://baidu.com')
with open('D:\\py.json', 'w', encoding='utf-8') as file:
    json.dump(dict(r.headers), file, indent=4)
```
## os模块
```python
os.fdopen(fd, *args, **kwargs)
os.close(fd)
os.fchmod(fd, mode)
os.fchown(fd, uid, gid)
os.fstat(fd)
os.open(path, flags, mode=0o777, *, dir_fd=None)
os.read(fd, n, /)
os.write(fd, str, /)
os.mkdir(path, mode=0o777, *, dir_fd=None)
os.makedirs(name, mode=0o777, exist_ok=False)
os.remove(path, *, dir_fd=None)
os.removedirs(name)
os.rename(src, dst, *, src_dir_fd=None, dst_dir_fd=None)
os.renames(old, new)
os.rmdir(path, *, dir_fd=None)
os.scandir(path='.')
os.stat(path, *, dir_fd=None, follow_symlinks=True)
os.listdir(path='.')

class os.DirEntry
class os.stat_result
os.utime(path, times=None, *, [ns, ]dir_fd=None, follow_symlinks=True)

os._exit(n)
os.kill(pid, sig, /)
```
