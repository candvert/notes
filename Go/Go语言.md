- [Go语言的特点](#Go语言的特点)
- [快速开始](#快速开始)
	- [程序入口](#程序入口)
	- [配置国内源](#配置国内源)
	- [运行单个文件](#运行单个文件)
- [基础内容](#基础内容)
	- [创建模块](#创建模块)
	- [每个源文件的结构](#每个源文件的结构)
	- [包](#包)
	- [名字的可见性](#名字的可见性)
	- [包初始化](#包初始化)
	- [init初始化函数](#init初始化函数)
	- [示例程序结构](#示例程序结构)
	- [零值](#零值)
	- [所有关键字](#所有关键字)
	- [命令行参数](#命令行参数)
	- [注释](#注释)
	- [常量](#常量)
	- [变量](#变量)
	- [变量的生命周期](#变量的生命周期)
	- [import](#import)
	- [声明语句](#声明语句)
	- [字符串](#字符串)
	- [原始字符串](#原始字符串)
	- [if](#if)
	- [switch](#switch)
	- [循环](#循环)
	- [类型别名和类型定义](#类型别名和类型定义)
	- [类型转换](#类型转换)
- [基本数据类型](#基本数据类型)
	- [复数](#复数)
	- [字符串](#字符串)
- [复合类型](#复合类型)
	- [数组](#数组)
	- [切片](#切片)
	- [Map](#Map)
	- [结构体](#结构体)
	- [Json支持](#Json支持)
	- [模板语法](#模板语法)
- [函数](#函数)
	- [匿名函数](#匿名函数)
	- [错误](#错误)
	- [defer](#defer)
	- [recover](#recover)
- [方法](#方法)
	- [String方法](#String方法)
- [接口](#接口)
	- [类型断言](#类型断言)
	- [type switch语句](#type%20switch语句)
- [Goroutines和Channels](#Goroutines和Channels)
	- [Goroutines](#Goroutines)
	- [Channels](#Channels)
	- [select](#select)
- [基于共享变量的并发](#基于共享变量的并发)
- [包和工具](#包和工具)
	- [包注释](#包注释)
	- [go的环境变量](#go的环境变量)
	- [go doc](#go%20doc)
	- [内部包](#内部包)
- [泛型](#泛型)
- [测试](#测试)
	- [外部测试包](#外部测试包)
	- [测试覆盖率](#测试覆盖率)
	- [基准测试函数](#基准测试函数)
	- [示例函数](#示例函数)
- [反射](#反射)
- [unsafe](#unsafe)

## Go语言的特点
```go
Go 有垃圾回收

Go 语言不允许导入未使用的包

Go 语言不允许使用无用的局部变量

Go 语言原生支持 Unicode

Go 中没有 ++i，只有 i++

Go 语言中 i++ 是语句，而不是表达式，因此 j = i++ 是非法的

Go 没有隐式的数值转换，没有构造函数和析构函数，没有运算符重载，没有默认参数，没有继承，没有异常，没有宏，没有函数和方法重载

Go 没有三元运算符

数组或切片越界会导致 panic

当 panic 发生时，所有 defer 的函数会执行，然后程序崩溃

Go 语言只有 for 循环这一种循环语句，for 循环有多种形式

创建百万级的 goroutine 完全是可行的

函数的左括号 { 必须和 func 函数声明在同一行上, 且位于行尾，相似的还有 for 循环

在包级别声明的变量会在 main 入口函数执行前完成初始化，局部变量将在声明语句被执行到的时候完成初始化
```
## 快速开始
## 程序入口
程序入口是 main 包中的 main 函数
```go
package main

import "fmt"

func main() {
	fmt.Println("hi")
}
```
## 配置国内源
```sh
到 https://goproxy.cn/ 查看如何配置
```
## 运行单个文件
```go
// 直接运行
go run a.go


// 先编译后运行
go build a.go
./a
```
## 基础内容
## 创建模块
go mod init 命令在当前目录下生成一个 go.mod 文件
```sh
go mod init web
```
生成的 go.mod 文件
```sh
module web

go 1.25.1
```
一个项目只能有一个根目录的 go.mod 文件，不能有其他 go.mod 文件
## 每个源文件的结构
每个源文件都以一条 package 声明语句开始，说明该源文件属于哪个包
package 声明语句之后是 import 语句
必须恰当导入需要的包，缺少了必要的包或者导入了不需要的包，程序都无法编译通过。这项严格要求避免了程序开发过程中引入未使用的包
## 包
一个包由单个目录下的一个或多个 .go 文件组成
一个目录下所有的 .go 文件必须归属于同一个包，惯例是包名和该目录名相同
## 名字的可见性
如果一个名字是在函数外部定义，那么将在当前包的所有文件中都可以访问。
名字的首字母的大小写决定了名字在包外的可见性。如果一个名字是大写字母开头的，那么它将是导出的，也就是说可以被外部的包访问，例如 fmt 包的 Printf 函数就是导出的，可以在 fmt 包外部访问。包本身的名字一般总是用小写字母。
结构体的字段也遵循相同规则。
## 包初始化
包初始化首先按照声明顺序初始化包级变量，但依赖关系会先被解析
```go
package any

var a = b + c // a 第三个初始化, 为 3
var b = f() // b 第二个初始化, 为 2
var c = 1 // c 第一个初始化, 为 1

func f() int { return c + 1 }
```
如果包有多个 .go 源文件，它们按照源文件提供给编译器的顺序进行初始化；在调用编译器之前，go 工具会先按名称对 .go 源文件进行排序

程序启动时，每次只初始化一个包，按照程序中导入的顺序进行，先初始化被依赖的包，main 包是最后初始化的
## init初始化函数
```go
// 每个源文件都可以包含任意多个 init 初始化函数，此类 init 函数不能被调用或引用，但除此之外，它们和普通函数相同
// 在每个源文件中，init 函数都会在程序启动时按照声明的顺序自动执行

// init 函数必须是这种形式，即没有返回值
func init() { /* ... */ }
```
## 示例程序结构
目录结构：
```go
- go.mod
- main.go
- web
	- connect.go
	- disConnect.go
	- http
		- http.go
```
connect.go 文件：
```go
package web

import "fmt"

func Connect() {
	fmt.Println("connectted")
}
```
http.go 文件
```go
package http

import "fmt"

func Http() {
	fmt.Println("send http")
}
```
在 main.go 中使用 main/web 包和 main/web/http 包
```go
package main

import "main/web"
import "main/web/http"

func main() {
	web.Connect()
	http.Http()
}
```
## 零值
如果变量没有显式初始化，则被隐式地赋予其类型的零值
数值类型为 0，布尔类型为 false，字符串为 ""（空字符串）
接口或引用类型（包括 slice、map、chan 和函数）为 nil，指针类型为 nil
数组或结构体等聚合类型对应的零值是每个元素或字段都是对应该类型的零值
## 所有关键字
```go
// 关键字有25个
break default func interface select
case defer go map struct
chan else goto package switch
const fallthrough if range type
continue for import return var





// 还有大约 30 多个预定义的名字
内建常量: true false iota nil

内建类型: int int8 int16 int32 int64
		 uint uint8 uint16 uint32 uint64 uintptr
		 float32 float64 complex128 complex64
		 bool byte rune string error
 
内建函数: make len cap new append copy close delete
		 complex real imag
		 panic recover



// Unicode 字符 rune 类型是和 int32 等价的类型，通常用于表示一个 Unicode 码点。这两个名称可以互换使用。同样 byte 也是 uint8 类型的等价类型
type byte = uint8
type rune = int32



// 优先级递减顺序排列的二元运算符，同一行为同一优先级
// 前两行的每个运算符都有相应的赋值运算符，例如 +=、*=
// 取余运算符 % 仅用于整数间的运算。余数的符号始终与被除数的符号相同，-5%3 和 -5%-3 的结果都为 -2
// 整数除法会将结果向零截断，7/4 的结果是 1，-7/4 的结果为 -1
// 有符号数的右移操作会将空出的位填充为符号位的值
* / % << >> & &^
+ - | ^
== != < <= > >=
&&
||




// 8 进制
n := 0666
// 16 进制
n = 0xde12
n = 0Xde12
n = 0xdE12


// rune 类型
// Rune literals are written as a character within single quotes
// 小于 256 的值可以使用 '\x41' 形式表示，但是更大的值就不能使用，比如 '\xe4\xb8\x96' 非法。更大的值要用 '\u4e16' 或 '\U00004e16' 表示
// \u 后面要跟 4 个十六进制数，即 '\uhhhh'。\U 后面要跟 8 个十六进制数，即 '\Uhhhhhhhh'
// 都表示 '世'
c1 := '世'
c2 := '\u4e16'
c3 := '\U00004e16'


var z float64
fmt.Println(z, -z, 1/z, -1/z, z/z) // "0 -0 +Inf -Inf NaN"
nan := math.NaN()
fmt.Println(nan == nan, nan < nan, nan > nan) // "false false false"



var x complex128 = complex(1, 2) // 1+2i
var y complex128 = complex(3, 4) // 3+4i
fmt.Println(x*y) // "(-5+10i)"
fmt.Println(real(x*y)) // "-5"
fmt.Println(imag(x*y)) // "10"
z :=  3 + 4i
```
## 命令行参数
```go
命令行参数可从 os 包的 Args 变量获取，os.Args 是一个切片，其第一个元素，os.Args[0], 是命令本身的名字；其它的元素则是程序启动时传给它的参数
```
## 注释
```go
// 单行注释


/* 多行
    注释 */


// 在每个源文件的包声明前仅跟着的注释是包注释。通常，包注释的第一句应该先是包的功能概要说明。一个包通常只有一个源文件有包注释。如果包注释很大，通常会放到一个独立的 doc.go文件中
```
## 常量
```go
// 常量的求值是在编译时


const z = 1
const x, y = 1, "hi"
const x, y int = 1, 2
const (
	x = 1
	y = 2
)
const (
	x int = 1
	y = 2
)


// 声明一组常量时，除了组中的第一个常量外，其余常量都可以省略右侧表达式，这意味着应该再次使用之前的表达式及其类型
const (
	a=1
	b
	c=2
	d
)
fmt.Println(a, b, c, d) // "1 1 2 2"


// iota 每次递增 1
type Weekday int
const (
	Sunday Weekday = iota
	Monday
	Tuesday
	Wednesday
	Thursday
	Friday
	Saturday
)
fmt.Println(Sunday, Monday, Tuesday) // "0 1 2"



// 许多常量并没有被指定为特定的类型。编译器会以比基本类型值更高的数值精度来表示这些未指定类型的常量，并且对它们进行的算术运算也比机器算术运算更精确；您可以假设至少有 256 位的精度
const (
	deadbeef = 0xdeadbeef // untyped int with value 3735928559
	a=uint32(deadbeef) // uint32 with value 3735928559
	b=float32(deadbeef) // float32 with value 3735928576 (rounded up)
	c=float64(deadbeef) // float64 with value 3735928559 (exact)
	d=int32(deadbeef) // compile error: constant overflows int32
	e=float64(1e309) // compile error: constant overflows float64
	f=uint(-1) // compile error: constant underflows uint
)
```
## 变量
```go
// 没有明确初始化的变量会被赋予对应类型的零值
// 零值是：
// 	数值类型为 0
//	布尔类型为 false
//	字符串为 ""（空字符串）
//  接口或引用类型（包括 slice、map、chan 和函数）为 nil
//  指针类型为 nil
var z int
var z = 1
var z int = 1

var x, y int
var x, y = 1, "hi"
var x, y int = 1, 2

var (
	x = 1
	y = 2
)
var (
	x int = 1
	y = 2
)



// 短变量声明
// := 只能在函数内部使用
z := 1
x, y := 1, "hi"
p := new(int) // p, *int 类型, 指向匿名的 int 变量



i := 0 // implicit int(0)
r := '\000' // implicit rune('\000')
f := 0.0 // implicit float64(0.0)
c := 0i // implicit complex128(0i)
```
## 变量的生命周期
```go
对于在包一级声明的变量来说，它们的生命周期和整个程序的运行周期是一致的
```
## import
```go
// Go 语言不允许导入未使用的包
// 每个导入声明都会建立从当前包到导入包的依赖关系。如果这些依赖关系形成一个循环，go build 命令会报错


// 第一种写法
import "fmt"
import "math/rand"


// 第二种写法
import (
	"fmt"
	"math/rand"
)


// 重命名导入
import (
	"crypto/rand"
	mrand "math/rand" // alternative name mrand avoids conflict
)


// 包的匿名导入，使用空标识符_
import _ "image/png"
```
## 声明语句
```go
// Go语言主要有四种类型的声明语句：var、const、type 和 func
```
## 字符串
```go
// go 的字符串使用的是 UFT-8 编码，也就是 ASCII 只占用一个字节，其他字符占用 1 ~ 4 个字节
// 字符串的零值是 "" （空字符串）
// 字符串是不可变的字节序列
// 字符串可以使用 ==、< 等进行比较





s := "hello, world"
// 内置的 len 函数返回字符串包含的字节数
fmt.Println(len(s)) // 12
fmt.Println(len("世界")) // 6
fmt.Println(s[0], s[7]) // 104 119
c := s[len(s)] // panic: index out of range
fmt.Println(s[0:5]) // hello
s += " good"
// 字符串是不可变的
s[0] = 'L' // compile error: cannot assign to s[0]


// 使用八进制表示一个字节，最大不能超过 \377
"\123"
// 使用十六进制表示一个字节
"\x8F"


// 都表示 6 个字节的 “世界”
"世界"
"\xe4\xb8\x96\xe7\x95\x8c" // 每个字节使用 \xhh 表示
"\u4e16\u754c" // 每两个字节使用 \uhhhh 表示
"\U00004e16\U0000754c" // 每三个字节使用 \Uhhhhhhhh 表示


// 返回字符串中有多少个 UTF-8 字符
n := utf8.RuneCountInString(s)


// 使用 range 循环时，会自动按字符进行迭代，而不是按字节
for i, r := range "Hello, 世界" {
	fmt.Printf("%d\t%q\t%d\n", i, r, r)
}


// 因为 rune 的大小固定，可以将字符串转换为 []rune 以方便索引
s := "プログラム"
r := []rune(s)
fmt.Printf("%x\n", r) // [30d7 30ed 30b0 30e9 30e0]
fmt.Printf("%q\n", r[0]) // プ
fmt.Println(string(r)) // プログラム
```

![](/images/go_1.png)
## 原始字符串
```go
// 使用 ``
s := `Go is a tool for managing Go source code.
Usage:
go command [arguments]
...
`
```
## if
```go
if command == "go" {
	fmt.Println("hi")
} else if command == "goo" {
	fmt.Println("hi")
} else {
	fmt.Println("hi")
}


if v := math.Pow(x, n); v < lim {
	return v
}
```
## switch
```go
// Go 只会运行匹配的 case 下的代码，而非之后所有的 case
// switch 的 case 无需为常量，且取值不限于整数
switch command {
	case "go":
		fmt.Println("hi")
	case "goo", "do":
		fmt.Println("hi")
	default:
		fmt.Println("hi")
}


switch os := runtime.GOOS; os {
case "darwin":
	fmt.Println("macOS.")
case "linux":
	fmt.Println("Linux.")
default:
	fmt.Printf("%s.\n", os)
}
// 无条件 switch
// 这种形式能将一长串 if-then-else 写得更加清晰
switch {
case t.Hour() < 12:
	fmt.Println("早上好！")
case t.Hour() < 17:
	fmt.Println("下午好！")
default:
	fmt.Println("晚上好！")
}
// 类型 switch
switch t := i.(type) {
case bool:
	fmt.Println("I'm a bool")
case int:
	fmt.Println("I'm an int")
default:
	fmt.Printf("Don't know type %T\n", t)
}
```
## 循环
```go
// Go 只有一种循环结构：for 循环
for {
	fmt.Println("hi")
	continue
	break
}

for sum < 1000 {
	sum += sum
}


// 初始化语句是可选的，初始化语句如果存在，必须是一条简单语句，即短变量声明、自增语句、赋值语句或函数调用
for i := 0; i < 10; i++ {
	sum += i
}

for ; sum < 1000; {
	sum += sum
}

nums := []int{2, 3, 4}
for _, num := range nums {
	fmt.Println("num:", num)
}
```
## 类型别名和类型定义
```go
// 类型别名
// 类型别名与源类型​​完全相同​，相互之间不需要类型转换
type byte = uint8



// 类型定义
// 类型定义创建一个​​新类型​，相互之间需要​​显式转换，它们拥有相同的底层数据表示，但方法集是独立的，​​新类型不继承​​源类型的方法集
type Celsius float64
```
## 类型转换
```go
// go 没有隐式类型转换


var i int = 42
var f float64 = float64(i)
// 浮点数到整数的转换会丢弃任何小数部分
var u uint = uint(f)

// 将字符串 "hello" 转换为字节切片
var r = []byte("hello")
```
## 基本数据类型
## 复数
## 字符串
## 复合类型
## 数组
```go
// 在 Go 代码中，切片更为常见
// 因为数组大小也是类型的一部分，所以 [3]int 和 [4]int 是不同类型
// 数组的大小必须是一个常量表达式


// 数组变量表示整个数组；它不是指向第一个数组元素的指针。这意味着，当你赋值或传递一个数组时，你将复制其内容
var a [5]int
a[4] = 100
// 内置 len 函数返回数组的长度
len(a)
// 传入指向数组的指针
zero(&a)
// 在一行中声明并初始化一个数组
b := [5]int{1, 2, 3, 4, 5}
// 让编译器计算元素的数量
b = [...]int{1, 2, 3, 4, 5}
// 包含 10 个元素的数组，下标为 9 的元素的值为 -2，下标为 6 的元素的值为 -5
r := [...]int{9: -2, 6: -5}
// 二维数组
twoD := [2][3]int{
	{1, 2, 3},
	{1, 2, 3},
}


s := [...]string{
	"George",
	"Ringo",
	"Pete", // 句尾的,是必须的
}


// 如果数组的元素类型是可比较的，那么该数组类型也是可比较的
a := [2]int{1, 2}
b := [...]int{1, 2}
c := [2]int{1, 3}
fmt.Println(a == b, a == c, b == c) // "true false false"
d := [3]int{1, 2}
fmt.Println(a == d) // compile error: cannot compare [2]int == [3]int
```
## 切片
```go
// 切片的底层是数组
// 切片的元素具有相同的类型
// 一个切片由三个部分构成：指针、长度和容量，容量一般是从切片的第一个元素开始到底层数组的结尾的元素数量
// 切片不可比较，唯一合法的切片比较是和 nil 相比，即 s == nil


// 声明一个切片，切片的零值为 nil
var s []string
// 对于 nil 切片，len 和 cap 函数都会返回 0
fmt.Println("uninit:", s, s == nil, len(s) == 0)


// 声明并初始化切片
a1 := []int{0, 1, 2, 3}
// 下标为 9 的元素的值为 -2，下标为 6 的元素的值为 -5
a2 := []int{9: -2, 6: -5}
s := []string{
	"George",
	"Ringo",
	"Pete", // 句尾的,是必须的
}


// 创建数组的切片
arr := [...]int{1, 2, 3, 4, 5, 6, 7, 8, 9}
l := arr[2:5]
// 创建指针的切片
ptr := &arr
l = ptr[2:5]


// 使用内置 make 创建非零长度的切片
// 调用 make 时，它​​会分配一个数组并返回一个该数组的切片
s := make([]string, 3)
// 使用内置的 len 和 cap 函数检查切片的长度和容量
fmt.Println("emp:", s, "len:", len(s), "cap:", cap(s))
s[0] = "a"
// 创建切片 s 的切片，范围[)
l := s[2:5]
l = s[:5]
l = s[2:]
l = s[:]
// 内置的 append 函数，当超过容量时，便会分配一个新数组，大小为原来底层数组的两倍，并将原数组的数据复制到新数组（和 C++ vector 的动态增长相同）。返回切片
s = append(s, "d")
s = append(s, "e", "f")
a := []string{"John", "Paul"}
b := []string{"George", "Ringo", "Pete"}
a = append(a, b...)
// 内置的 copy 函数，将切片 s 的内容复制到切片 c 中，返回被复制的元素数量
c := make([]string, len(s))
copy(c, s)


// 超过容量导致 panic，但超过长度可以扩展切片（在容量内）
arr := [...]int{1, 2, 3, 4, 5, 6, 7, 8, 9}
l := arr[2:4]
ll := l[:20] // 会导致 panic
ll := l[:5] // ll 的长度超过原本切片 l 的长度


// 存在切片不为 nil，但长度和容量为 0
// go 函数应该相同对待长度为 0 的切片，不管是不是 nil
s := []int{} // 长度为 0
s2 := make([]int, 3)[3:] // 长度为 0
s = []int{0, 1, 2, 3}
s3 := s[:0] // 长度为 0


// 二维切片
// 与二维数组不同，内部切片的长度可以变化
twoD := make([][]int, 3)
for i := range 3 {
	innerLen := i + 1
	twoD[i] = make([]int, innerLen)
	for j := range innerLen {
		twoD[i][j] = i + j
	}
}
```
## Map
```go
// Map 是 Go 的内置类型
// Map 是哈希表的引用，键必须是唯一的，键值对是无序的
// 键必须可以使用 == 进行比较，以确认该键是否存在
// Map 的零值是 nil
// Map 之间不可比较，唯一合法的 Map 比较是和 nil 相比，即 m == nil


// 创建 Map
ages := map[string]int{
	"alice": 31,
	"charlie": 34, // 句尾的,是必须的
}
// 遍历
for name, age := range ages {
	fmt.Printf("%s\t%d\n", name, age)
}

// 使用内置的 make 函数创建一个空的 Map
m := make(map[string]int)
m["alice"] = 31
// 如果键不存在，则返回值类型对应的零值
m["bob"] = m["bob"] + 1
// 使用内置 len 函数返回 Map 的元素数量
fmt.Println("len:", len(m))
// 内置 delete 函数从 Map 中删除键值对
delete(m, "alice")
// 使用内置的 clear 函数删除所有键值对
clear(m)
// 可选的第二个返回值是布尔类型，true 表示键存在
age, ok := m["charlie"]
```
## 结构体
```go
// 结构体字段的首字母如果是大写的，那么它就是导出的
// 如果结构体的所有字段都是可比较的，那么该结构体就是可比较的，可以使用 == 或 != 进行比较
// 空结构体 struct{}，大小为 0


type Point struct{ X, Y int }
// 下面两种形式的初始化不能混用
// 第一种形式
p := Point{1, 2}
// 第二种形式
p2 := Point{Y: 2}
p3 := Point{}
// 一个语法糖，初始化一个结构体并取得它的地址
ptr := &Point{1, 2}



// 匿名字段，即允许字段只有类型而没有名字
// 其实匿名字段是有名字的，名字和类型名相同，但是这些名字是可选的当使用点表达式时，可以省略任意多个
// 因为匿名字段实际有隐式的名字，所以不能有两个相同类型的字段，因为会导致名称冲突
type Point struct {
	X, Y int
}
type Circle struct {
	Point
	Radius int
}
type Wheel struct {
	Circle
	Spokes int
}
var w Wheel
w.X = 8 // equivalent to w.Circle.Point.X = 8
w.Y = 8 // equivalent to w.Circle.Point.Y = 8
w.Radius = 5 // equivalent to w.Circle.Radius = 5
w.Spokes = 20
// 初始化方式
w = Wheel{Circle{Point{8, 8}, 5}, 20}
w = Wheel{
	Circle: Circle{
		Point: Point{X: 8, Y: 8},
		Radius: 5,
	},
	Spokes: 20, // NOTE: trailing comma necessary here (and at Radius)
}
// 假设 Point 上有方法 Draw，那么可以和字段一样直接调用：
w.Draw()
// 或者可以这样：
toDraw := w.Draw
toDraw()
// 还可以这样
toDraw2 := Point.Draw
toDraw2(p)



type Point struct {
	X, Y int
}
type Circle struct {
	P Point
	Radius int
}
c := Circle{
	P: Point{
		X: 10,
		Y: 10, // NOTE: trailing comma necessary here (and at Radius)
	},
	Radius: 5, 
}
c.P.X = 20
c.X // 错误




// 匿名结构体
dog := struct {
	name   string
	isGood bool
}{
	"Rex",
	true,
}
```
## Json支持
```go
// `json:"released"` 称为字段标签，转换为 json 时默认会使用字段名作为 json 的键名，字段标签
// `json:"released"` 中的键 json 控制 encoding/json 包的行为，其他的键控制相应的 encoding/... 包的行为
// `json:"color,omitempty"` 中的 omitempty 意味着如果 Color 字段的值为零值，则该字段不会转换为 json
type Movie struct {
	Title string
	Year int `json:"released"`
	Color bool `json:"color,omitempty"`
	Actors []string
}
var movies = []Movie{
	{Title: "Casablanca", Year: 1942, Color: false,
		Actors: []string{"Humphrey Bogart", "Ingrid Bergman"}},
	{Title: "Cool Hand Luke", Year: 1967, Color: true,
		Actors: []string{"Paul Newman"}},
	{Title: "Bullitt", Year: 1968, Color: true,
		Actors: []string{"Steve McQueen", "Jacqueline Bisset"}},
	// ...
}
// 只有导出的字段会转换为 json
// Marshal 函数返回一个字节切片，该切片为一个长字符串，不包含空格和换行符
data, err := json.Marshal(movies)
// MarshalIndent 函数返回的是缩进良好的结果，后两个参数是可选的
data, err := json.MarshalIndent(movies, "", " ")
if err != nil {
	log.Fatalf("JSON marshaling failed: %s", err)
}
fmt.Printf("%s\n", data)



// Unmarshal 函数
// 从 json 转换为 go 结构体的过程是大小写不敏感的，也就是说不用担心首字母大小写问题
var titles []struct{ Title string }
if err := json.Unmarshal(data, &titles); err != nil {
log.Fatalf("JSON unmarshaling failed: %s", err)
}
fmt.Println(titles) // "[{Casablanca} {Cool Hand Luke} {Bullitt}]"
```
## 模板语法
```go
// {{...}} 叫做 actions
// {{.Title | printf "%.64s"}} 中的 | 类似 Linux 命令行的管道符，printf 为模板语法内置的 fmt.Sprintf 功能
const templ = `{{.TotalCount}} issues:
{{range .Items}}----------------------------------------
Number: {{.Number}}
User: {{.User.Login}}
Title: {{.Title | printf "%.64s"}}
Age: {{.CreatedAt | daysAgo}} days
{{end}}`


// template.Must 接受一个模板和 err，如果 err 是 nil 则返回模板，否则 panic
// template.New 创建并返回一个模板
// Funcs 添加在模板中可以访问的函数，返回这个模板
var report = template.Must(template.New("issuelist").
	Funcs(template.FuncMap{"daysAgo": daysAgo}).
	Parse(templ))

result, err := github.SearchIssues(os.Args[1:])
if err := report.Execute(os.Stdout, result); err != nil {
	log.Fatal(err)
}



// html/template 包
import "html/template"
var issueList = template.Must(template.New("issuelist").Parse(`
<h1>{{.TotalCount}} issues</h1>
<table>
<tr style='text-align: left'>
	<th>#</th>
	<th>State</th>
	<th>User</th>
	<th>Title</th>
</tr>
{{range .Items}}
<tr>
	<td><a href='{{.HTMLURL}}'>{{.Number}}</a></td>
	<td>{{.State}}</td>
	<td><a href='{{.User.HTMLURL}}'>{{.User.Login}}</a></td>
	<td><a href='{{.HTMLURL}}'>{{.Title}}</a></td>
</tr>
{{end}}
</table>
`))
```
## 函数
```go
// 如果两个函数参数列表和返回值列表中的变量类型一一对应，就被视为同一函数类型
// 函数调用必须按照声明顺序为所有参数提供实参
// Go语言没有默认参数，也没有任何方法可以通过参数名指定形参，因此形参和返回值的变量名对于函数调用者而言没有意义
// Go语言使用可变栈，栈的大小按需增加（初始时很小）。这使得我们使用递归时不必考虑溢出和安全问题
// 在Go中，函数被看作第一类值（first-class values）：函数像其他值一样，拥有类型，可以被赋值给其他变量，传递给函数，从函数返回。对函数值（function value）的调用类似函数调用


func say() {
	fmt.Println("hi")
}

// 没有函数体的函数，表示不是由 Go 实现的
func Sin(x float64) float //implemented in assembly language

// 一组形参有相同的类型，我们不必为每个形参都写出参数类型
func add(x, y int) int {
	return x + y
}
// 返回值也可以命名，值为对应类型的零值
func sub(x, y int) (z int) { z = x - y; return}
// _ 表示不使用该参数
func first(x int, _ int) int { return x }
// 不提供参数名称
func zero(int, int) int { return 0 }


// 多个返回值
func val() (int, int) {
    return 3, 7
}
_, b := val()
// 一个函数内部可以将另一个有多返回值的函数作为返回值
func val2(x, y int) (int, int) {
    return vals()
}
// 当你调用接受多参数的函数时，可以将一个返回多参数的函数作为该函数的参数
val2(val())


// bare return
func val3() (x, y int) {
	x = 3
	return
}




// 可变参数，需要在参数列表的最后一个参数类型之前加上省略符号 “...”
// nums 被看作是 []int
func sum(nums ...int) {
    fmt.Println("hi")
}
sum(1, 2)
sum(1, 2, 3)
// 如果参数是切片，可以使用 nums...
nums := []int{1, 2, 3, 4}
sum(nums...)


// 函数变量，函数类型的零值是 nil。调用值为 nil 的函数值会引起 panic
func square(n int) int { return n * n }
f := square
fmt.Println(f(3))


// 匿名函数
f := func() {
	fmt.Println("hi")
}
// 直接调用
func() {
	fmt.Println("hi")
}()
```
## 匿名函数
```go
// 匿名函数可以访问其定义时环境中的变量
func squares() func() int {
	var x int
	return func() int {
		x++
		return x * x
	}
}

func main() {
	f := squares()
	fmt.Println(f()) // "1"
	fmt.Println(f()) // "4"
	fmt.Println(f()) // "9"
	fmt.Println(f()) // "16"
}
```
## 错误
```go
// 按照惯例，错误是最后一个返回值
func f(arg int) (int, error) {
    if arg == 42 {
        return -1, errors.New("can't work with 42")
    }
    return arg + 3, nil
}
```
## defer
```go
// 只需要在调用普通函数或方法前加上关键字 defer，就完成了 defer 所需要的语法

// 直到包含 defer 语句的外层函数执行完毕，defer 语句的函数才会执行
// 即使外层函数发生 panic，defer 语句也会执行

// defer 语句的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用
// defer 语句经常被用于处理成对的操作，如打开、关闭、连接、断开连接
func main() {
	defer fmt.Println("world")
	fmt.Println("hello")
}
// 你可以在一个函数中执行多条 defer 语句，它们的执行顺序与声明顺序相反
func main() {
	fmt.Println("counting")
	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}
	fmt.Println("done")
}
```
## recover
```go
// 有些情况无法恢复，比如内存不足，Go 运行时会以致命错误终止程序

// 如果在 deferred 函数中调用了内置函数 recover，并且包含该 defer 语句的外层函数发生了 panic，recover 函数会使程序从 panic 中恢复，并返回 panic value。发生 panic 的函数不会继续运行，但会正常返回。如果未发生 panic，recover 调用会返回 nil

func Parse() (s *Syntax, err error) {
	defer func() {
		if p := recover(); p != nil {
			err = fmt.Errorf("internal error: %v", p)
		}
	}()
	// ...parser...
}
```
## 方法
```go
// 方法可以被声明到任意类型

type rect struct {
    width, height int
}
// 定义 rect 类型上的方法
func (r *rect) area() int {
    return r.width * r.height
}
// 定义 rect 类型上的方法
func (r rect) perim() int {
    return 2*r.width + 2*r.height
}
r := rect{width: 10, height: 5}
r.area() // 编译器隐式地取地址调用，同 (&r).area()
r.perim()
rp := &r
rp.area()
rp.perim() // 编译器解引用，同 (*r).area()



// 当变量的值为 nil 时也可以调用
type Person struct{}
func (p *Person) Say() {
	fmt.Println("hi")
}
r := new(Person)
r = nil
r.Say()
// 不使用 receiver 则可以省略
func (*Person) Move() {
	fmt.Println("move")
}


// 如果一个类型名本身是一个指针的话，是不允许其出现在接收器中的
type P *int
func (P) f() { /* ... */ } // compile error: invalid receiver type
```
## String方法
```go
// 当为结构体定义 String 方法后，fmt 会直接调用用户定义的 String 方法
```
## 接口
```go
// 接口类型只包含方法的声明
// 一个类型只要实现了接口中声明的所有方法，那么就实现了该接口
// 因为空接口 interface{} 不包含任何方法声明，所以 Go 语言中的所有类型都默认实现了空接口

type Shower interface {
	show(string) int
}
// 接口类型的零值是 nil
var w Shower
// 对值为 nil 的接口变量调用方法会导致 panic
w.show("hi") // panic: nil pointer dereference



type geometry interface {
    area() float64
    perim() float64
}
type rect struct {
    width, height float64
}
func (r rect) area() float64 {
    return r.width * r.height
}
func (r rect) perim() float64 {
    return 2*r.width + 2*r.height
}
func measure(g geometry) {
    fmt.Println(g)
    fmt.Println(g.area())
    fmt.Println(g.perim())
}
r := rect{2, 4}
// 因为 rect 实现了 geometry 接口，所以可以这样调用
measure(r)



// 接口内嵌
type Reader interface {
	Read()
}
type All interface {
	Reader
}
type rect struct {
    width, height float64
}
func (*rect) Read() {
    fmt.Println("read")
}
var r All
re := &rect{10, 10}
r = re
r.Read()



// *rect 类型满足了 Reader 接口，而 rect 类型没有
type Reader interface {
    Read()
}
func (r *rect) Read() int {
    fmt.Println("ok")
}
var r rect
var i Reader = &r
var i Reader = r // compile error: r lacks Read method
// 赋值给接口类型的变量后就只能调用该接口类型声明的方法
i.Read()
i.Close() // compile error: Reader lacks Close method



// 空接口，因为空接口不包含任何方法声明，所以 Go 语言中的所有类型都默认实现了空接口
var any interface{}
any = 12.34
any = "hello"
any = map[string]int{"one": 1}



// 可以这样在编译时检查某个类型是否满足某个接口
var _ io.Writer = (*bytes.Buffer)(nil)


// strct 可以拥有接口类型字段
type See struct {
	what interface{}
	who string
}



// 通常使用空接口作为参数类型来接收任意类型参数
// args 被看作是 []interface{}
func Add(args ...interface{}) { /* ... */ }
Add(4, "who")
```
## 类型断言
```go
// 判断接口类型变量的动态类型是否为断言类型

// 非安全断言
var i interface{} = "hello"
s := i.(string) // 断言成功，s 的值为 "hello"
f := i.(int)    // 断言失败，程序 panic
z, ok := i.(string) // 断言成功时 ok 为 true





// 断言是否满足接口
type Reader interface{
	Read()
}
type Writer interface {
	Write()
}
type Ok struct{ x int }
func (* Ok) Read() { fmt.Println("Read") }
func (* Ok) Write() { fmt.Println("Write") }

var o Ok{5}
var w Reader
w = o
z1 := w.(Writer) // 成功
z2 := w.(Water) // panic
z3, ok := w.(Writer) // 断言成功时 ok 为 true
```
## type switch语句
```go
// type 是一个关键字，​​且只能用在 switch 语句中​​，不能在其它地方单独使用
func printType(v interface{}) {
	// 当 case 后跟​​单个类型​​时，变量 t 为该具体类型
    switch t := v.(type) {
    case int:
        fmt.Printf("整数: %d\n", t)
    case string:
        fmt.Printf("字符串: %s\n", t)
	// 当 case 后跟​​​​多个类型​​时，变量 t 为变量 v 的接口类型
    case bool, float64:
        fmt.Printf("未知类型: %T\n", t)
	// 当 case 后为 nil ​​时，变量 t 为变量 v 的接口类型
	case nil:
		fmt.Printf("未知类型: %T\n", t)
	// default 分支，变量 t 为变量 v 的接口类型
    default:
        fmt.Printf("未知类型: %T\n", t)
    }
}

// 调用示例
printType(42)        // 输出: 整数: 42
printType("hello")   // 输出: 字符串: hello
printType(true)      // 输出: 布尔值: true
printType(3.14)      // 输出: 未知类型: float64
```
## Goroutines和Channels
为 go build、go run、go test 添加 -race 选项，会记录所有对共享变量的访问，所有同步操作，包括 go 语句，channel 操作，和对 sync.Mutex.Lock 等的调用

goroutines 和 threads 的区别：
- 操作系统的 threads 有一个固定大小的栈（通常是 2MB）。而 goroutines 起始时有一个较小的栈（通常是 2KB），栈的大小会随需要增大或缩小，最大可达 1GB
- 切换操作系统的 threads 开销较大。go 运行时包含自己的调度器，使得切换 goroutines 的开销较小
- go 的调度器使用 GOMAXPROCS 来决定同时有多少个操作系统线程执行 go 代码。其默认的值是 CPU 的核心数，所以在一个有 8 个核心的机器上时，最多有 8 个线程同时运行 go 代码
- 操作系统的每个 threads 都有一个 id，因此可以使用该 id 做到线程局部存储。而 goroutinues 没有 id，这是设计上故意为之，因为线程局部存储总被滥用
## Goroutines
```go
// goroutine 由 go 语句创建
// go 语句就是普通的函数或方法调用，以 go 关键字为前缀
// go 语句本身立即完成
// 除了从主程序返回或退出程序之外，没有其他编程方式可以让一个 goroutine 停止另一个

f() // call f(); wait for it to return
go f() // create a new goroutine that calls f(); don't wait
```
## Channels
```go
// channel 的零值是 nil，在 nil channel 上的发送和接收操作会永远阻塞
// 两个 channel 可以使用 == 进行比较
// 一个可以发送 int 类型数据的 channel 的类型为 chan int
// channel 有两个主要操作，send 和 receive，都使用 <- 运算符
// channel 支持第三种操作，即 close，它表示该 channel 上不会再发送任何值，后续的发送尝试将导致 panic
// 对一个已经被 close 的 channel 进行接收操作依然可以接收到之前已经成功发送的数据，如果该 channel 中已经没有数据则接收操作立刻完成，并返回类型的零值
// 一般来说没必要主动关闭 channel，垃圾回收器会决定何时回收不管 channel 有没有关闭
// 尝试关闭一个已经关闭的 channel 会导致 panic，关闭 nil channel 也一样


// 使用内置的 make 函数创建 channel
ch := make(chan int) // ch has type 'chan int'
ch <- x // a send statement
x = <-ch // a receive expression in an assignment statement
<-ch // a receive statement; result is discarded
// ok 为 true 时表示接收成功，为 false 时表示 channel 已关闭并且 channel 中没有多余数据
x, ok = <-ch

// for 循环
for x := range ch {
	fmt.Println(x)
}

// 使用内置的 close 函数关闭 channel
close(ch)


// 以最简单方式调用 make 函数创建的是 unbuffered channel
// make 接受第二个可选参数，即一个整数，表示通道的容量。如果容量非零，make 会创建一个 buffered channel
// 在 unbuffered channel 上执行发送操作会阻塞发送 goroutine，直到另一个 goroutine 在同一 channel 上执行相应的接收操作。相反，如果首先尝试接收操作，则接收 goroutine 将被阻塞，直到另一个 goroutine 在同一 channel 上执行发送操作
// buffered channel 有一个元素队列，队列的大小为使用 make 函数创建时指定的容量大小。在 buffered channel 上进行的发送操作会在队尾插入一个元素，接收操作会移除队首的一个元素。如果通道已满，则发送操作将阻塞其 goroutine，直到另一个 goroutine 的接收操作提供空间。相反，如果通道为空，则接收操作将阻塞，直到另一个 goroutine 发送值
ch = make(chan int) // unbuffered channel
ch = make(chan int, 0) // unbuffered channel
ch = make(chan int, 3) // buffered channel with capacity 3
// 使用内置 cap 函数可以获取 channel 的容量
fmt.Println(cap(ch))
// 内置的 len 函数返回正在 channel 的队列中的元素数量
fmt.Println(len(ch))


// 类型 chan<- int 是一个 int 类型的只发送通道，允许发送但不允许接收
// 类型 <-chan int 是一个 int 类型的只接收通道，允许接收但不允许发送
// 在任何赋值操作中都允许从双向到单向通道类型的转换，但是无法再变回双向通道类型
func sender(out chan<- int) { }
func receiver(in <-chan int) { }
ch := make(chan int)
go sender(ch) // 隐式转换为 chan<- int 类型
go receiver(ch) // 隐式转换为 <-chan int 类型

var out chan<- int
var in <-chan int
```
## select
```go
// 不包含任何 case 的 select。即 select{}，会永远等待
// 如果多个 case 同时 ready，那么 select 会随机选一个
// 如果 case 的 channel 是 nil，那么该 case 永远不会被选择

select {
case <-ch1:
// ...
case x := <-ch2:
// ...use x...
case ch3 <- y:
// ...
default:
// ...
}
```
## 基于共享变量的并发
互斥锁：
```go
import "sync"

// 惯例是 sync.Mutex 变量初始化的下一行紧跟使用该互斥锁的变量的初始化
var mu sync.Mutex
var	balance int

mu.Lock()
balance += 1
mu.Unlock()
```
多个读取器，单个写入器锁（只有很特定的情况才使用，因为相比 Mutex，RWMutex 需要更复杂的内部记录）：
```go
var mu sync.RWMutex
var number int

func Get() int {
	mu.RLock()
	defer mu.RUnlock()
	return number
}

func Set(i int) {
	mu.Lock()
	defer mu.Unlock()
	number = i
}
```
考虑并发的懒加载：
```go
// sync.Once 只有一个 Do 方法，该方法接收一个函数作为参数，确保该函数只调用一次
var loadIconsOnce sync.Once
var icons map[string]image.Image

func Icon(name string) image.Image {
	loadIconsOnce.Do(loadIcons)
	return icons[name]
}
```
## 包和工具
## 包注释
每个包应该只有一个源文件有包注释，包注释位于 package 语句之前
```go
// Package tempconv performs Celsius and Fahrenheit conversions.
package tempconv
```
如果包注释内容很多，惯例是将其放入专门的 doc.go 文件中
```go
// Copyright 2009 The Go Authors. All rights reserved.

/*
...
...
...
*/
package tempconv
```
## go的环境变量
go env 命令打印所有和 go 有关的环境变量

GOOS：所在操作系统
GOARCH：处理器架构（amd64、arm 等）
## go doc
```sh
# 整个包的注释
go doc time

# 某个包成员的注释
go doc time.Since

# 某个方法的注释
go doc time.Duration.Seconds
```
## 内部包
Go语言的构建工具对包含 internal 名字的路径段的包导入路径做了特殊处理。这种包叫 internal 包，一个 internal 包只能被和 internal 目录有同一个父目录的包所导入。例如，net/http/internal/chunked 内部包只能被 net/http/httputil 或 net/http 包导入，但是不能被net/url包导入
## 泛型
## 测试
在包目录下的以 `_test.go` 结尾的文件是测试文件

测试函数的名字以 Test 开头
基准测试函数（benchmark function）的名字以 Benchmark 开头，作用是测量性能
示例函数（example function）的名字以 Example 开头，提供 machine-checked 文档

每个测试文件都必须导入 testing 包
```go
import "testing"

// 测试函数的签名（函数名 Test 后面的单词必须以大写字母开头）
func TestSin(t *testing.T) { /* ... */ }


t.Logf("%d", 10)
t.Errorf("%d", 10)
```
示例（这种 table-driven 的测试在 go 中很常见）：
```go
import "testing"

func TestIsPalindrome(t *testing.T) {
	var tests = []struct {
		input string
		want bool
	}{
		{"", true},
		{"a", true},
		{"aa", true},
		{"ab", false},
		{"kayak", true},
		{"detartrated", true},
		{"A man, a plan, a canal: Panama", true},
		{"Evil I did dwell; lewd did I live.", true},
		{"Able was I ere I saw Elba", true},
		{"été", true},
		{"Et se resservir, ivresse reste.", true},
		{"palindrome", false}, // non-palindrome
		{"desserts", false}, // semi-palindrome
	}
	for _, test := range tests {
		if got := IsPalindrome(test.input); got != test.want {
			t.Errorf("IsPalindrome(%q) = %v", test.input, got)
		}
	}
}
```
go test 命令不带任何参数运行当前目录下的测试文件
-v 选项会打印每个测试的名称和执行时间
-run 选项指定正则表达式，只有函数名满足该正则表达式的测试会运行
```go
 go test -v -run="French|Canal"
```
测试失败的消息通常是 "f(x) = y，want z" 的形式，x 为传递的参数，y 为得到的结果
## 外部测试包
外部测试包通过将测试代码声明为独立的包来解决包循环依赖问题
外部测试包的文件以 `_test.go` 结尾，和其他测试文件位于同一目录，不同之处在于其包声明语句为 `package xxx_test`，包名需要以 `_test` 结尾
```go
myproject
├── go.mod
├── main.go
└── play/
    ├── logic.go
	├── logic_test.go // 普通测试文件
	├── play_test.go  // 外部测试文件
    └── export_test.go  // 用于导出包内部名字的测试文件
	
```
play_test.go 文件：
```go
package play_test

import (
    "play"
    "testing"
)

func TestNewBuffer(t *testing.T) { /* ... */ }
```
若外部测试包需访问被测包的未导出函数，可通过包内测试文件 `_test.go` 暴露内部细节，如果该测试文件仅用于此目的且本身不包含任何测试，则通常将其称为 export_test.go
```go
package play

var IsSpace = isSpace  // 导出内部函数 isSpace，这样外部测试包就可以通过 IsSpace 访问
```
## 测试覆盖率
go test 的 -coverprofile 选项收集覆盖率数据
```sh
 go test -coverprofile=c.out
 # 处理日志，生成 HTML，并在浏览器打开
 go tool cover -html=c.out


# 打印已执行语句的比例的摘要
 go test -cover


# "hotter" 块的代码和 "colder" 块的代码
 -covermode=count
```
## 基准测试函数
基准测试函数的名字以 Benchmark 开头，参数为 `*testing.B`。testing.B 有一个导出的字段 N，指定测量次数
```go
import "testing"

// 基准测试函数的签名
func BenchmarkIsPalindrome(b *testing.B) {
	for i := 0; i < b.N; i++ {
		IsPalindrome("A man, a plan, a canal: Panama")
	}
}


t.Logf("%d", 10)
t.Errorf("%d", 10)
```
`go test -bench=.` 命令运行当前目录下的所有基准测试函数，-bench 选项指定正则表达式，符合正则表达式的基准测试函数才会运行
-benchmem 选项会包含内存分配数据
```go
go test -bench=. -benchmem
```
测试不同数据量的惯例做法（比如操作 10 个数组元素和操作 100 个数组元素）：
```go
func benchmark(b *testing.B, size int) { /* ... */ }
func Benchmark10(b *testing.B) { benchmark(b, 10) }
func Benchmark100(b *testing.B) { benchmark(b, 100) }
func Benchmark1000(b *testing.B) { benchmark(b, 1000) }
```
`go test -cpuprofile=cpu.out` CPU 占用时间分析
`go test -blockprofile=block.out` goroutinue 阻塞分析
`go test -memprofile=mem.out` 内存占用分析
```go
go test -run=NONE -bench=ClientServerParallelTLS64 \
-cpuprofile=cpu.log net/http
go tool pprof -text -nodecount=10 ./http.test cpu.log
```
## 示例函数
示例函数的名字以 Example 开头，ExampleIsPalindrome 将会显示在 IsPalindrome 函数的文档中，而一个名为 Example 的示例函数则会与整个包关联起来
```go
func ExampleIsPalindrome() {
	fmt.Println(IsPalindrome("A man, a plan, a canal: Panama"))
	fmt.Println(IsPalindrome("palindrome"))
	// Output:
	// true
	// false
}
```
如果示例函数包含类似上面的 `// Output:` 注释，go test 将执行该函数，并检查其标准输出是否与注释中的文本匹配
## 反射
## unsafe