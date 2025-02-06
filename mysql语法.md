- [mysql语法](#mysql语法)
	- [基本语法](#基本语法)
- [Windows上使用](#Windows上使用)
	- [MySQL Shell](#MySQL Shell)
	- [MySQL Command Line Client](#MySQL Command Line Client)

# mysql语法

## 基本语法

```mysql
基础篇：

mysql -h (主机名) -P (端口号) -u (用户名) -p

SQL语句：
DDL语句： 	数据库操作
			SHOW DATABASES;
			CREATE DATABASE 数据库名;
			USE 数据库名;
			SELECT DATABASE();
			DROP DATABASE 数据库名;

			表操作
			SHOW TABLES;
			CREATE TABLE 表名(
				字段 字段类型 [约束条件],			// 约束条件可以有NOT NULL, UNIQUE, PRIMARY KEY, AUTO_INCREMENT, DEFAULT, CHECK, FROEIGN KEY
				...
			);
			DESC 表名;
			SHOW CREATE TABLE 表名;
			ALTER TABLE 表名 ADD/MODIFY/CHANGE/DROP/RENAME TO ...;
			DROP TABLE 表名;

DML语句：	INSERT INTO 表名(字段1，字段2) VALUES(值1，值2),(值1，值2);
			UPDATE 表名 SET 字段1=值1, 字段2=值2 [WHERE 条件];
			DELETE FROM 表名 [WHERE 条件];

DQL语句：	SELECT
				字段列表
			FROM
				表名
			WHERE
				条件列表			> >= < <= = <> like between...and in and or !
			GROUP BY
				分组字段列表
			HAVING
				分组后条件列表
			ORDER BY
				排序字段列表		ASC DESC
			LIMIT
				分页参数			

DCL语句：	CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
			ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '密码';
			DROP USER '用户名'@'主机名';

			GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
			REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';

函数：		1.字符串函数
			CONCAT, LOWER, UPPER, LPAD, RPAD, TRIM, SUBSTRING
			2.数值函数
			CEIL, FLOOR, MOD, RAND, ROUND
			3.日期函数
			CURDATE, CURTIME, NOW, YEAR, MONTH, DAY, DATE_ADD, DATEDIFF
			4.流程函数
			IF, IFNULL, CASE [...] WHEN ... THEN ... ELSE ... END

多表查询：	内连接
				隐式：SELECT ... FROM 表A, 表B WHERE 条件...
				显示：SELECT ... FROM 表A INNER JOIN 表B ON 条件...
			外连接：
				左外：SELECT ... FROM 表A LEFT JOIN 表B ON 条件...
				右外：SELECT ... FROM 表A RIGHT JOIN 表B ON 条件...
			自连接：
			子查询：标量子查询，列子查询，行子查询，表子查询

事务：
	事务是一组操作的集合，这组操作，要么全部执行成功，要么全部执行失败。

	START TRANSACTION;
	COMMIT;
	ROLLBACK;

	事务四大特性ACID
	原子性(Atomicity)，一致性(Consistency)，隔离性(Isolation)，持久性(Durability)

	并发事务问题
	脏读，不可重复读，幻读

	事务隔离级别
	READ UNCOMMITED, READ COMMITED, REPEATABLE READ, SERIAILIZABLE
```

# Windows上使用
## MySQL Shell
```
在Windows上安装mysql后，可以启动mysql shell来编写和运行一些代码。
mysql shell中的命令都以\开头。
mysql shell可以使用js、python和sql语言，切换语言命令为\js或\py或\sql。
```
![](images/mysql_1.png)
```
连接本地数据库：
\connect root@localhost
```
## MySQL Command Line Client
```
在Windows上安装mysql后，可以启动MySQL Command Line Client以连接并操作数据库。
```
![](images/mysql_2.png)
