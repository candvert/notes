```
SQLite是一个C语言库
SQLite是世界上使用最多的数据库，所有手机都内置SQLite

下载网址：
https://www.sqlite.org/download.html
Windows上下载sqlite-tools-win-x64，其中包含命令行工具sqlite3.exe

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