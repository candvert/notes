- [简介](#简介)
- [下载](#下载)
- [命令行使用](#命令行使用)
- [数据类型](#数据类型)

## 简介
```
SQLite是一个C语言库
SQLite是世界上使用最多的数据库，所有手机都内置SQLite
```
## 下载
```
下载网址：
https://www.sqlite.org/download.html
Windows上下载sqlite-tools-win-x64，其中包含命令行工具sqlite3.exe
```
## 命令行使用
```
将命令行工具所在目录配置到环境变量后使用命令：
sqlite3 data.db     打开或创建一个数据库
或先sqlite3命令进入sqlite数据库再使用.open data.db打开数据库
sqlite3 data.db .dump > tmp.txt     导出数据库
sqlite3 data.db < tmp.txt           导入数据库

以下都是先输入sqlite3进入sqlite数据库后输入的命令：
.show
.help
.quit
.databases
.tables
.schema     输出建表语句
创建表：
create table ok (
	id integer primary key,
	name text,
	age int
);
删除表：
drop table ok;
插入：
insert into ok values(1, 'john', 18);
insert into ok(name, age) values('Amily', 18);
查看：
select * from ok;
```
## 数据类型
```
SQLite使用动态类型系统，值的数据类型与值本身相关，而不是由包含它的列（column）决定
从版本3.37.0开始，SQLite提供了严格类型执行的STRICT表，即和mysql相同每列的数据类型固定

每个值都属于下面5个存储类（storage class）中的一种：
NULL 值为空值
INTEGER 有符号整数，根据值的大小存储在0、1、2、3、4、6或8个字节中
REAL 浮点数，存储为8字节IEEE浮点数
TEXT 字符串，使用数据库编码（UTF-8、UTF-16BE或UTF-16LE）存储
BLOB a blob of data，存储的内容与输入的完全一致

存储类比数据类型更通用。例如，INTEGER存储类包括7种不同长度的整数数据类型
但是，一旦INTEGER值从磁盘读取并放入内存中进行处理，它们就会转换为最通用的数据类型（8字节有符号整数）

SQLite没有单独的布尔存储类。布尔值存储为整数0（假）和1（真）
从版本3.23.0开始，SQLite可以识别关键字TRUE和FALSE，但这些关键字实际上只是整数1和0的另一种写法

SQLite没有专门用于存储日期和时间的存储类。但是，SQLite的内置日期和时间函数能够将日期和时间存储为TEXT、REAL或INTEGER值：
    TEXT为ISO8601字符串（“YYYY-MM-DD HH:MM:SS.SSS”）
    INTEGER作为Unix时间，自1970-01-01 00:00:00 UTC以来的秒数
应用程序可以选择以任何这些格式存储日期和时间，并使用内置的日期和时间函数在格式之间自由转换
```