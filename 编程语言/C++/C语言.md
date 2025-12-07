- [C语言标准](#C语言标准)
- [程序入口](#程序入口)
- [main函数](#main函数)
- [注释](#注释)
- [变量](#变量)
- [常量](#常量)
- [数据类型](#数据类型)
- [字面值常量](#字面值常量)
- [字符](#字符)
- [字符串](#字符串)
- [转义序列](#转义序列)
- [算术运算](#算术运算)
- [控制流](#控制流)
	- [语句和代码块](#语句和代码块)
	- [if](#if)
	- [switch](#switch)
	- [for](#for)
	- [while](#while)
	- [do-while](#do-while)
	- [goto](#goto)
- [复合类型](#复合类型)
	- [数组](#数组)
	- [结构体](#结构体)
	- [union](#union)
	- [枚举](#枚举)
	- [函数](#函数)
	- [指针](#指针)

所有内容基于 C99 标准
```c
// 如果程序流程跳过了局部变量的初始化语句而直接使用该变量，其行为是“未定义”的
```
## C语言标准
```sh
C89/C90    首个官方标准，于1989年由美国国家标准协会（ANSI）发布，随后在1990年被国际标准化组织（ISO）采纳
C99
C11
C17    C11的“缺陷修复版”，无新功能
C23
```
## 程序入口
```c
#include <stdio.h>

int main() {
    printf("hello\n");
}
```
## main函数
```c
// 可以省略返回类型
main() {
    printf("hello\n");
}
```
## 注释
```c
// 单行注释


/*
多行注释
*/
```
## 变量
```c
// 变量命名规则：变量名由字母和数字组成，第一个字符必须是字母。下划线“_”也算作字母
// 大写字母和小写字母是有区别的。所以 x 和 X 是两个不同的名称
// 外部变量和静态变量默认初始化为零


// 未初始化的变量有未定义的值
int num, age;
num = 10;


// 声明并初始化
int height = 178;
```
## 常量
```c
const double e = 2.71828182845905;
const char msg[] = "warning: ";
```
## 数据类型
```c
// char 类型可以是 signed char 或 unsigned char，取决于具体的编译器实现
char、signed char、unsigned char
int
short // 和 short int 相同
long // 和 long int 相同
float
double
long double
```
## 字面值常量
```c
// int 类型
18

// char 类型
'A'

// long 类型，后缀为 l 或 L
18l
18L

// unsigned 类型，后缀为 u 或 U
18u
18U

// unsigned long 类型，后缀为 ul 或 UL
18ul
18UL
```
## 字符
```c
// char 类型是整数
'\013' // 八进制表示
'\x7b' // 十六进制表示


// '\0' 的值就是 0
```
## 字符串
```c
// 字符串字面值是一个字符数组
char s[] = "hello";

// 空字符串
""

// 相邻的两个或多个字符串字面值会自动合并
s = "hello, " "world";
```
## 转义序列
```c
\n
\t
\'
\"
\\
\a
\ooo // 八进制数
\xhh // 十六进制数
\b
\?
\f
\r
\v
```
## 算术运算
```c
// 按优先级递减顺序排列的运算符，右侧是求值顺序
() [] -> .                             left to right
! ~ ++ -- + - *(type) sizeof           right to left
* / %                                  left to right
+ -                                    left to right
<< >>                                  left to right
< <= > >=                              left to right
== !=                                  left to right
&                                      left to right
^                                      left to right
|                                      left to right
&&                                     left to right
||                                     left to right
?:                                     right to left
= += -= *= /= %= &= ^= |= <<= >>=      right to left
,                                      left to right




// 二元运算符
+、-、*、/、%
// 取余运算符 % 只能用于整数类型，余数的符号始终与被除数的符号相同，-5%3 和 -5%-3 的结果都为 -2



// 整数除法会将结果向零截断，7/4 的结果是 1，-7/4 的结果为 -1
int num = 7/4;
```
## 控制流
## 语句和代码块
```c
// 当表达式（例如 x = 0、i++ 或 printf(...)）后面跟上分号时，它就变成了一条语句

// 代码块
{
	int num = 10;
	printf("hi\n");
}
```
## if
```c
int num = 5;
if (num > 2) {
	printf("%d\n", num);
} else if (num > 4) {
	printf("%d\n", num);
} else {
	printf("%d\n", num);
}


// 如果只有一条语句，可以省略大括号
if (num > 2)
	printf("%d\n", num);
else if (num > 4)
	printf("%d\n", num);
else
	printf("%d\n", num);
```
## switch
```c
// 所有 case 中的表达式必须是不同的
int num = 10;
switch (num) {
	case 1: case 2: case 3:
		printf("1 or 2 or 3\n");
		break;
	case 4:
	case 5:
		printf("4 or 5\n");
		break;
	case 6: {
		printf("6\n");
		break;
	}
	default:
		printf("%d\n", num);
}




// 在 C 语言的 switch 语句中，所有 case 标签都共享同一个作用域
// 因此下面代码中在 default 分支中名字 m 是“可见”的，但由于其初始化语句 int m = 100; 并未被执行，该行为是“未定义”的
// 如果程序流程跳过了局部变量的初始化语句而直接使用该变量，其行为是“未定义”的
switch (2) {
	case 1:
		int m = 100;
		printf("%d\n", m);
	default:
		printf("%d\n", m);
}
```
## for
```c
for (int i = 0; i < 10; i++) {
	printf("hi\n");
}



// 如果只有一条语句，可以省略大括号
for (int i = 0; i < 10; i++)
	printf("hi\n");



for (;;)
	printf("hi\n");


for (;;);
```
## while
```c
int num = 5;
while (num > 0) {
	printf("%d\n", num);
	num--;
}



// 如果只有一条语句，可以省略大括号
while (2 < 3)
	printf("hi\n");


while (1);
```
## do-while
```c
int num = 5;
do {
	printf("%d\n", num);
	num--;
} while (num > 0);



// 如果只有一条语句，可以省略大括号
do
	printf("hi\n");
while (2 < 3);
```
## goto
```c
// 输出是 water0123water0123water0123...
repeat:
	printf("water");
for (int i = 0; i < 10; i++) {
	printf("%d", i);
	if (i == 3) goto repeat;
}
```
## 复合类型
## 数组
```c

```
## 结构体
```c

```
## union
```c

```
## 枚举
```c
enum months { JAN = 1, FEB, MAR, APR, MAY, JUN,
	JUL, AUG, SEP, OCT, NOV, DEC };
	/* FEB = 2, MAR = 3, etc. */
```
## 函数
```c
// 最简单的函数
dummy() {}


// 不返回任何值
void dummy() {}


int strlen(const char[]);
```
## 指针
```c

```