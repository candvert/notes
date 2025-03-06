- [python创建用于测试的http服务器](#python创建用于测试的http服务器)
- [python的内置api和一些三方库的api](#python的内置api和一些三方库的api)
	- [列表](#列表)
	- [元组](#元组)
	- [字典](#字典)
	- [集合](#集合)
	- [enumerate进行遍历](#enumerate进行遍历)
	- [json](#json)
	- [unittest测试（内置测试框架）](#unittest测试（内置测试框架）)
	- [文件操作](#文件操作)
	- [获取时间和sleep函数](#获取时间和sleep函数)
	- [随机数](#随机数)
	- [多线程](#多线程)
	- [正则表达式](#正则表达式)
	- [网络socket](#网络socket)
	- [sqlite](#sqlite)
	- [mysql](#mysql)
	- [网络库requests](#网络库requests)

# python创建用于测试的http服务器
```
mkdir test_server && cd test_server
echo {"good": true} >> ok.json
python3 -m http.server 8000
```
# python的内置api和一些三方库的api
## 列表
```py
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
```py
words = ('good', 42, 'human', 'profit')		# words类型为words: tuple[str, int, str, str]，因为元组不变，所以类型固定
```
## 字典
```py
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
```py
# 集合不像列表和字典，集合不会以特定的顺序存储元素
words = {'good', 42, 'human', 'profit'}		# words的类型为words: set[str | int]
config = {'name': 'John', 'age': 18, 1.0: 'one'}
words = set(config.keys())
words = set(config.values())
```
## enumerate进行遍历
```py
name = ['john', 'joey']
for index, value in enumerate(name):	# enumerate()返回下标和对应的值
    print(index)
    print(value)
```
## json
```py
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
```py
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
## 文件操作
```py
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
## 获取时间和sleep函数
```py
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
```py
import random
a = random.randint(1, 10)	# [1, 10]
a = random.random()			# [0, 1)
```
## 多线程
```py
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
```py
# 使用re.match(pattern, string)进行匹配
import re

res = re.match('h.s', 'his you')
```
## 网络socket
```py
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
```py
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
```py
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
```py
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
