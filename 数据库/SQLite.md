- [简介](#简介)
- [下载](#下载)
- [命令行使用](#命令行使用)
- [数据类型](#数据类型)

```sh
每个值都属于下面5个存储类（storage class）中的一种：
NULL 值为空值
INTEGER 有符号整数，根据值的大小存储在0、1、2、3、4、6或8个字节中
REAL 浮点数，存储为8字节IEEE浮点数
TEXT 字符串，使用数据库编码（UTF-8、UTF-16BE或UTF-16LE）存储
BLOB a blob of data，存储的内容与输入的完全一致
```
## 简介
```sh
SQLite是一个C语言库
SQLite是世界上使用最多的数据库，所有手机都内置SQLite
```
## 下载
```sh
下载网址：
https://www.sqlite.org/download.html
Windows上下载sqlite-tools-win-x64，其中包含命令行工具sqlite3.exe
```
## 命令行使用
```sh
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
```sh
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


SQLite支持列上的类型亲和性（type affinity），列的类型亲和性是存储在该列中的数据的推荐类型，但任何列仍然可以存储任何类型的数据。只是有些列在选择时会优先使用一种存储类而不是另一种。列的首选存储类称为其“亲和性”。
SQLite3数据库中的每一列都被分配了下列类型亲和性之一：
TEXT    NUMERIC     INTEGER     REAL        BLOB
具有TEXT亲和性的列使用存储类NULL、TEXT或BLOB存储所有数据。如果将数字数据插入具有TEXT亲和性的列中，则会将其转换为文本形式然后再存储。
具有NUMERIC亲和性的列可能包含所有五种存储类的值。当将字符串插入到NUMERIC列时，如果字符串是格式正确的整数或实数，则字符串的存储类将分别转换为INTEGER或REAL（按优先顺序）。如果字符串不是格式正确的整数或实数，则该值将存储为TEXT。
具有INTEGER亲和性的列的行为与具有NUMERIC亲和性的列的行为相同。INTEGER和NUMERIC亲和性之间的差异仅出现在CAST表达式：表达式“CAST(4.0 AS INT)”返回整数4，而“CAST(4.0 AS NUMERIC)”将值保留为浮点数4.0。
具有REAL亲和性的列的行为类似于具有NUMERIC亲和性的列，只是它强制将整数值转换为浮点表示。（作为内部优化，没有小数部分且存储在具有REAL亲和性的列中的小浮点数被作为整数写入磁盘以占用更少的空间，并且在读出该值时自动转换回浮点数。这种优化在SQL级别完全不可见，只能通过检查数据库文件的原始位才能检测到。）
具有亲和力BLOB的列不会优先选择一种存储类别，也不会尝试将数据从一种存储类别强制转换为另一种存储类别。

对于未声明为STRICT的表，列的亲和性由列的声明类型决定，并按顺序遵循以下规则：
1. 如果声明的类型包含字符串“INT”，则其被分配INTEGER亲和力。
2.  如果列的声明类型包含字符串“CHAR”、“CLOB”或“TEXT”中的任何一个，则该列具有TEXT亲和性。请注意，类型VARCHAR包含字符串“CHAR”，因此被分配了TEXT亲和性。
3. 如果列的声明类型包含字符串“BLOB”或者未指定类型，则该列具有亲和力BLOB。
4. 如果列的声明类型包含任何字符串“REAL”、“FLOA”或“DOUB”，则该列具有REAL亲和性。
5. 否则，亲和力为NUMERIC。
请注意，确定列亲和性的规则顺序很重要。声明类型为“CHARINT”的列将匹配规则1和规则2，但第一个规则优先，因此列亲和性将为INTEGER。

SQLite不对字符串、BLOB或数值的长度施加任何长度限制（除了全局的SQLITE_MAX_LENGTH限制）。
```