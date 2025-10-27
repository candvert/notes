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
	- [示例程序结构](#示例程序结构)
	- [零值](#零值)
	- [所有关键字](#所有关键字)
	- [命令行参数](#命令行参数)
	- [注释](#注释)
	- [常量和变量](#常量和变量)
	- [变量的生命周期](#变量的生命周期)
	- [import](#import)
	- [声明语句](#声明语句)
	- [init初始化函数](#init初始化函数)
	- [if](#if)
	- [switch](#switch)
	- [循环](#循环)
	- [类型别名](#类型别名)
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
	- [错误](#错误)
	- [defer](#defer)
- [方法](#方法)
	- [String方法](#String方法)
- [接口](#接口)
	- [类型断言](#类型断言)
- [Goroutines和Channels](#Goroutines和Channels)
	- [Goroutines](#Goroutines)
	- [Channels](#Channels)
- [基于共享变量的并发](#基于共享变量的并发)
- [包和工具](#包和工具)
- [泛型](#泛型)
- [测试](#测试)
- [反射](#反射)
- [unsafe](#unsafe)


- [标准库](#标准库)
	- [strings](#strings)
	- [io](#io)
	- [net](#net)

## Go语言的特点
```go
Go 有垃圾回收

Go 语言不允许导入未使用的包

Go 语言不允许使用无用的局部变量

Go 语言原生支持 Unicode

Go 中没有 ++i，只有 i++

Go 语言中 i++ 是语句，而不是表达式，因此 j = i++ 是非法的

Go 没有隐式的数值转换，没有构造函数和析构函数，没有运算符重载，没有默认参数，也没有继承，没有异常，没有宏

Go 没有三元运算符

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
一个包由单个目录下的一个或多个 .go 源代码文件组成
一个目录下所有的 .go 源文件必须归属于同一个包
## 名字的可见性
如果一个名字是在函数外部定义，那么将在当前包的所有文件中都可以访问。
名字的开头字母的大小写决定了名字在包外的可见性。如果一个名字是大写字母开头的，那么它将是导出的，也就是说可以被外部的包访问，例如 fmt 包的 Printf 函数就是导出的，可以在 fmt 包外部访问。包本身的名字一般总是用小写字母。
结构体的字段也遵循相同规则。
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



// 按照优先级递减的顺序排列的二元运算符，同一行为同一优先级
// 取模运算符 % 仅用于整数间的运算
// 5/4 的结果是 1
* / % << >> & &^
+ - | ^
== != < <= > >=
&&
||
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
## 常量和变量
```go
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
```
## 变量的生命周期
```go
对于在包一级声明的变量来说，它们的生命周期和整个程序的运行周期是一致的
```
## import
```go
// Go 语言不允许导入未使用的包


// 第一种写法
import "fmt"
import "math/rand"


// 第二种写法
import (
	"fmt"
	"math/rand"
)


// 在 Go 语言中，每个包都有一个全局唯一的导入路径
```
## 声明语句
```go
// Go语言主要有四种类型的声明语句：var、const、type 和 func
```
## init初始化函数
```go
// 每个文件都可以包含多个 init 初始化函数，这些 init 初始化函数不能被调用或引用
func init() { /* ... */ }
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
## defer
```go
// 只需要在调用普通函数或方法前加上关键字 defer，就完成了 defer 所需要的语法
// 当 defer 语句被执行时，跟在 defer 后面的函数会被延迟执行。直到包含该 defer 语句的函数执行完毕时，defer 后的函数才会被执行，不论包含 defer 语句的函数是通过 return 正常结束，还是由于 panic 导致的异常结束
// 你可以在一个函数中执行多条 defer 语句，它们的执行顺序与声明顺序相反
// 推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用
// defer 语句经常被用于处理成对的操作，如打开、关闭、连接、断开连接
func main() {
	defer fmt.Println("world")
	fmt.Println("hello")
}
// 推迟调用的函数调用会被压入一个栈中。当外层函数返回时，被推迟的调用会按照后进先出的顺序调用
func main() {
	fmt.Println("counting")
	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}
	fmt.Println("done")
}
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
s := []int{0, 1, 2, 3}
s2 := []int{9: -2, 6: -5}

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
// Map 不可比较，唯一合法的 Map 比较是和 nil 相比，即 m == nil


// 创建 Map
ages := map[string]int{
	"alice": 31,
	"charlie": 34,
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
delete(m, "charlie")
// 使用内置的 clear 函数删除所有键值对
clear(m)
// 可选的第二个返回值是布尔类型，true 表示键存在
age, ok := m["charlie"]
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
func sum(nums...int) {
    fmt.Println("hi")
}
sum(1, 2)
sum(1, 2, 3)
// 如果参数是切片，可以使用 nums...
nums := []int{1, 2, 3, 4}
sum(nums...)


// 函数变量，函数类型的零值是nil。调用值为nil的函数值会引起panic错误
func square(n int) int { return n * n }
f := square
fmt.Println(f(3))
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


// 当对象是 nil 时也可以调用
func (r *rect) area() int {
    return r.width * r.height
}
r := nil
r.area


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
// 一个类型如果拥有一个接口需要的所有方法，那么这个类型就实现了这个接口



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
measure(r)



// 接口内嵌
type ok interface {
	geometry
}


// 赋值语法，等号右边需要满足左边的接口
var g geometry
r := rect{2, 4}
g = r
var o ok
g = ok


// *rect 类型满足了 one 接口，而 rect 类型没有
type one interface {
    on()
}
func (r *rect) on() int {
    fmt.Println("ok")
}
var r rect
var one = &r
var one = r // compile error: r lacks on method



// 空接口，因为空接口类型对实现它的类型没有要求，所以我们可以将任意一个值赋给空接口类型
var any interface{}
any = true
any = 12.34
any = "hello"
```
## 类型断言
```go
// 在运行时检查接口变量底层具体类型

// 非安全断言
// 语法 value := interfaceVar.(T)
var i interface{} = "hello"
s := i.(string) // 断言成功，s 的值为 "hello"
f := i.(int)    // 断言失败，程序 panic


// 安全断言
// 语法 value, ok := interfaceVar.(T)
var i interface{} = "hello"
if s, ok := i.(string); ok {
    fmt.Println("是字符串:", s)
} else {
    fmt.Println("不是字符串类型")
}
```
## 类型别名
```go
type Celsius float64
type byte = uint8
```
## 类型转换
```go
// go 没有隐式类型转换


var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

// 将字符串 "x"转换为字节切片（[]byte）
var r = []byte("x")
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
## Goroutines和Channels
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

var mu sync.Mutex
var	balance int

mu.Lock()
balance += 1
mu.Unlock()
```
多个读取器，单个写入器锁：
```go
import "sync"

var mu sync.RWMutex
var balance int

mu.RLock()
result := balance
mu.RUnlock()
```
## 包和工具
## 泛型
## 测试
## 反射
## unsafe
## 标准库
## strings
```go
strings.Join("hello", " world")
```
## io
```go
io 包保证任何由文件结束引起的读取失败都返回同一个错误 io.EOF，该错误在 io 包中定义
```
## net
```go
import "net"

listener, err := net.Listen("tcp", "localhost:8000")
for {
	conn, err := listener.Accept()
	io.WriteString(conn, time.Now().Format("15:04:05\n"))
}


conn, err := net.Dial("tcp", "localhost:8000")

```