- [程序入口](#程序入口)
- [配置国内源](#配置国内源)
- [运行单个文件](#运行单个文件)
- [go语言特点](#go语言特点)
- [所有关键字](#所有关键字)
- [注释](#注释)
- [常量和变量](#常量和变量)
- [import](#import)
- [导出名字](#导出名字)
- [运算符](#运算符)
- [if](#if)
- [switch](#switch)
- [for](#for)
- [defer](#defer)
- [数组](#数组)
- [切片](#切片)
- [Map](#Map)
- [指针](#指针)
- [结构体](#结构体)
- [方法](#方法)
- [接口](#接口)
- [嵌套结构体](#嵌套结构体)
- [数据类型](#数据类型)
- [类型别名](#类型别名)
- [类型转换](#类型转换)
- [包和模块](#包和模块)
- [函数](#函数)
- [闭包](#闭包)
- [Range](#Range)
- [字符串](#字符串)
- [错误](#错误)
- [Goroutines](#Goroutines)
- [Channels](#Channels)



- [标准库](#标准库)
	- [strings](#strings)
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
## go语言特点
```go
Go语言原生支持 Unicode

Go语言的代码通过包组织，一个包由位于单个目录下的一个或多个.go源代码文件组成

每个源文件都以一条 package 声明语句开始，表示该文件属于哪个包

import 声明必须跟在文件的 package 声明之后

必须恰当导入需要的包，缺少了必要的包或者导入了不需要的包，程序都无法编译通过。这项严格要求避免了程序开发过程中引入未使用的包

函数的左括号 { 必须和 func 函数声明在同一行上, 且位于末尾，相似的还有 for 循环

变量会在声明时直接初始化。如果变量没有显式初始化，则被隐式地赋予其类型的零值，数值类型为 0，布尔类型为 false，字符串为 ""（空字符串），接口或引用类型（包括 slice、map、chan 和函数）为 nil，指针类型为 nil

i++ 是语句，所以 j = i++ 非法

go 中没有 ++i 的操作，只有 i++

Go语言只有for循环这一种循环语句。for循环有多种形式

Go语言不允许使用无用的局部变量

空标识符_

在包级别声明的变量会在 main 入口函数执行前完成初始化，局部变量将在声明语句被执行到的时候完成初始化

1.0 版本 Go 没有隐式的数值转换，没有构造函数和析构函数，没有运算符重载，没有默认参数，也没有继承，没有泛型，没有异常，没有宏，没有函数修饰，更没有线程局部存储

创建百万级的goroutine完全是可行的

go 中没有三元运算符

go 有垃圾回收

没有明确初始化的变量会被赋予对应类型的零值

命令行参数可从os包的Args变量获取，os.Args 是一个切片，其第一个元素，os.Args[0], 是命令本身的名字；其它的元素则是程序启动时传给它的参数
```
## 所有关键字
```go
// 关键字有25个
break default func interface select
case defer go map struct
chan else goto package switch
const fallthrough if range type
continue for import return var





// 还有大约30多个预定义的名字
内建常量: true false iota nil

内建类型: int int8 int16 int32 int64
		 uint uint8 uint16 uint32 uint64 uintptr
		 float32 float64 complex128 complex64
		 bool byte rune string error
 
内建函数: make len cap new append copy close delete
		 complex real imag
		 panic recover
```
## 注释
```go
// 单行注释


/* 多行
    注释 */
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
// := 不能在函数外使用
z := 1
x, y := 1, 2
p := new(int) // p, *int 类型, 指向匿名的 int 变量
```
## import
```go
// 第一种写法
import "fmt"
import "math/rand"


// 第二种写法
import (
	"fmt"
	"math/rand"
)
```
## 导出名字
```go
如果一个名字是在函数内部定义，那么它的就只在函数内部有效。如
果是在函数外部定义，那么将在当前包的所有文件中都可以访问。名
字的开头字母的大小写决定了名字在包外的可见性。如果一个名字是
大写字母开头的，那么它将是导出的，也就是说可以被外部
的包访问，例如fmt包的Printf函数就是导出的，可以在fmt包外部访
问。包本身的名字一般总是用小写字母。
```
## 运算符
```go
+=
对string类型， + 运算符连接字符串
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
// Go 只会运行匹配的 case，而非之后所有的 case
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
## for
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
// defer 语句会将函数推迟到外层函数返回之后执行。
// 推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用
func main() {
	defer fmt.Println("world")
	fmt.Println("hello")
}
// 推迟调用的函数调用会被压入一个栈中。 当外层函数返回时，被推迟的调用会按照后进先出的顺序调用
func main() {
	fmt.Println("counting")
	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}
	fmt.Println("done")
}
```
## 数组
```go
// 在 Go 代码中，切片更为常见
// Go 的数组是值。数组变量表示整个数组；它不是指向第一个数组元素的指针。这意味着，当你赋值或传递一个数组值时，你将复制其内容。
var a [5]int
a[4] = 100
len(a)
// 在一行中声明并初始化一个数组
b := [5]int{1, 2, 3, 4, 5}
// 让编译器计算元素的数量
b = [...]int{1, 2, 3, 4, 5}
// 如果使用：指定索引，则其间的元素将被清零
b = [...]int{100, 3: 400, 500}
// 二维数组
twoD := [2][3]int{
	{1, 2, 3},
	{1, 2, 3},
}
```
## 切片
```go
var s []string
// 未初始化的切片等于 nil ，对于 nil 切片，len 和 cap 函数都会返回 0
fmt.Println("uninit:", s, s == nil, len(s) == 0)
// 使用内置 make 创建非零长度的切片
// 调用 make 时，它​​会分配一个数组并返回一个引用该数组的切片
s = make([]string, 3)
// 使用内置的 len 和 cap 函数检查切片的长度和容量
fmt.Println("emp:", s, "len:", len(s), "cap:", cap(s))
s[0] = "a"
// 内置的 append 操作，返回切片
s = append(s, "d")
s = append(s, "e", "f")
a := []string{"John", "Paul"}
b := []string{"George", "Ringo", "Pete"}
a = append(a, b...)
// copy
c := make([]string, len(s))
copy(c, s)
// 范围[)
l := s[2:5]
l = s[:5]
l = s[2:]
l = s[:]
// 在一行中声明并初始化切片
t := []string{"g", "h", "i"}
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
// Map 是 Go 内置的类型
// 使用内置的 make 命令创建一个空的 Map
m := make(map[string]int)
m["k1"] = 7
// 如果键不存在，则返回the zero value of the value type
v3 := m["k3"]
fmt.Println("len:", len(m))
// 内置 delete 从 Map 中删除键/值对
delete(m, "k2")
// 使用内置的 clear 函数删除所有键/值对
clear(m)
// 获取值时可选的第二个返回值指示键是否存在于 Map 中
_, prs := m["k2"]
// 在同一行中声明和初始化新 Map
n := map[string]int{"foo": 1, "bar": 2}
```
## 指针
```go
// Go 拥有指针
// 类型 *T 是指向 T 类型值的指针，其零值为 nil
// 与 C 不同，Go 没有指针运算
func main() {
	i, j := 42, 2701

	p := &i         // 指向 i
	fmt.Println(*p) // 通过指针读取 i 的值
	*p = 21         // 通过指针设置 i 的值
	fmt.Println(i)  // 查看 i 的值

	p = &j         // 指向 j
	*p = *p / 37   // 通过指针对 j 进行除法运算
	fmt.Println(j) // 查看 j 的值
	fmt.Println("pointer:", p) // 指针也可以被打印
}
```
## 结构体
```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
	// 结构体指针
	p := &v
	// 隐式解引用
	p.X = 1e9
}





type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
	v2 = Vertex{X: 1}  // Y:0 被隐式地赋予零值
	v3 = Vertex{}      // X:0 Y:0
	p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
)



// 匿名结构体
dog := struct {
	name   string
	isGood bool
}{
	"Rex",
	true,
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
fmt.Println("area: ", r.area()) // 编译器隐式地取地址调用，同 (&r).area()
fmt.Println("perim:", r.perim())
rp := &r
fmt.Println("area: ", rp.area())
fmt.Println("perim:", rp.perim())
```
## 接口
```go
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
```
## 嵌套结构体
```go
type base struct {
    num int
}
type container struct {
    base
    str string
}
co := container{
	base: base{
		num: 1,
	},
	str: "some name",
}
fmt.Println("num:", co.num)
fmt.Println("also num:", co.base.num)
	
func (b base) describe() string {
    return fmt.Sprintf("base with num=%v", b.num)
}
fmt.Println("describe:", co.describe())
```
##  数据类型
```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
     // 表示一个 Unicode 码位

float32 float64

complex64 complex128




// 浮点数默认 float64

// 比较浮点数
math.Abs(answer - 42) < 0.0001
// fmt.Println 中使用 %T 打印数据类型
// 0x8d
// 整数超出范围，发生“环绕”
var peace string = "peace"
var peace string = `peace` // 原始字符串字面值
```
## 类型别名
```go
type byte = uint8
```
## 类型转换
```go
// go 没有隐式类型转换
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```
## 包和模块
```go
包就是文件夹，模块就是 .go 文件
在 Go 中，如果一个名字以大写字母开头，那么它就是已导出的。例如，Pizza 就是个已导出名，Pi 也同样，它导出自 math 包。
pizza 和 pi 并未以大写字母开头，所以它们是未导出的。
在导入一个包时，你只能引用其中已导出的名字。 任何「未导出」的名字在该包外均无法访问。
```
## 函数
```go
func add(x int, y int) int {
	return x + y
}
// 当连续两个或多个函数的已命名形参类型相同时，除最后一个类型以外，其它都可以省略
func add(x, y int) int {
	return x + y
}
// 多个返回值
func vals() (int, int) {
    return 3, 7
}
a, b := vals()
_, c := vals()
// Go 的返回值可被命名
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
// 将任意数量的整数作为参数，nums 的类型等同于 []int
func sum(nums ...int) {
    fmt.Print(nums, " ")
    total := 0
	
    for _, num := range nums {
        total += num
    }
    fmt.Println(total)
}
sum(1, 2)
sum(1, 2, 3)
nums := []int{1, 2, 3, 4}
sum(nums...)
// 递归
func fact(n int) int {
    if n == 0 {
        return 1
    }
    return n * fact(n-1)
}
// 递归
var fib func(n int) int
fib = func(n int) int {
	if n < 2 {
		return n
	}
	return fib(n-1) + fib(n-2)
}
```
## 闭包
```go
func intSeq() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}
nextInt := intSeq()
fmt.Println(nextInt())
fmt.Println(nextInt())
fmt.Println(nextInt())
```
## Range
```go
nums := []int{2, 3, 4}
for index, num := range nums {
	if num == 3 {
		fmt.Println("index:", index)
	}
}


	
kvs := map[string]string{"a": "apple", "b": "banana"}
for k, v := range kvs {
	fmt.Printf("%s -> %s\n", k, v)
}
for k := range kvs {
	fmt.Println("key:", k)
}




for index, char := range "go" {
	fmt.Println(index, char)
}
```
## 字符串
```go
// Go 语言和标准库将字符串视为 UTF-8 编码文本的容器
// 在 Go 中, 字符被叫做 rune，它是一个表示 Unicode code point 的整数
var a = "go" + "lang"
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
## Goroutines
```go
func f(from string) {
    for i := range 3 {
        fmt.Println(from, ":", i)
    }
}
go f("goroutine")
// 匿名函数
go func(msg string) {
	fmt.Println(msg)
}("going")
```
## Channels
```go
// Channels 是连接 Goroutines 的管道
// 从一个 goroutine 中将值发送到管道，并从另一个 goroutine 中接收值
// 默认情况下，发送和接收都会阻塞，直到发送者和接收者都已准备好

// 创建 channel
messages := make(chan string)
// 发送 "ping" 到 messages 管道
go func() { messages <- "ping" }()
// 接收管道中的信息
msg := <-messages
```
## 标准库
## strings
```go
strings.Join("hello", " world")
```