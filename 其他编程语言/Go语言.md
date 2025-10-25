- [程序入口](#程序入口)
- [配置国内源](#配置国内源)
- [运行单个文件](#运行单个文件)
- [创建模块](#创建模块)
- [每个源文件的结构](#每个源文件的结构)
- [包](#包)
- [名字的可见性](#名字的可见性)
- [零值](#零值)
- [所有关键字](#所有关键字)
- [注释](#注释)
- [常量和变量](#常量和变量)
- [变量的生命周期](#变量的生命周期)
- [import](#import)
- [声明语句](#声明语句)
- [init初始化函数](#init初始化函数)
- [运算符](#运算符)
- [if](#if)
- [switch](#switch)
- [循环](#循环)
- [defer](#defer)
- [数组](#数组)
- [切片](#切片)
- [Map](#Map)
- [make函数](#make函数)
- [指针](#指针)
- [new](#new)
- [函数](#函数)
- [结构体](#结构体)
- [方法](#方法)
- [接口](#接口)
- [嵌套结构体](#嵌套结构体)
- [类型别名](#类型别名)
- [类型转换](#类型转换)
- [闭包](#闭包)
- [Range](#Range)
- [字符串](#字符串)
- [错误](#错误)
- [Goroutines](#Goroutines)
- [Channels](#Channels)


- [反射](#反射)
- [unsafe](#unsafe)
- [泛型](#泛型)



- [标准库](#标准库)
	- [strings](#strings)

go 有垃圾回收
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
## 创建模块
使用 go mod init 命令来初始化一个新的 Go 模块。这会生成一个 go.mod 文件
## 每个源文件的结构
每个源文件都以一条 package 声明语句开始，说明该源文件是属于哪个包
package 声明语句之后是 import 语句
必须恰当导入需要的包，缺少了必要的包或者导入了不需要的包，程序都无法编译通过。这项严格要求避免了程序开发过程中引入未使用的包
## 包
一个包由单个目录下的一个或多个 .go 源代码文件组成
一个目录下所有的 .go 源文件必须归属于同一个包
## 名字的可见性
如果一个名字是在函数外部定义，那么将在当前包的所有文件中都可以访问。
名字的开头字母的大小写决定了名字在包外的可见性。如果一个名字是大写字母开头的，那么它将是导出的，也就是说可以被外部的包访问，例如 fmt 包的 Printf 函数就是导出的，可以在 fmt 包外部访问。包本身的名字一般总是用小写字母。

```go
Go 语言原生支持 Unicode


函数的左括号 { 必须和 func 函数声明在同一行上, 且位于末尾，相似的还有 for 循环


Go 语言不允许导入未使用的包

Go 语言不允许使用无用的局部变量


Go 语言中 i++ 是语句，而不是表达式，因此 j = i++ 是非法的

Go 中没有 ++i 的操作，只有 i++

Go 语言只有 for 循环这一种循环语句。for 循环有多种形式

空标识符_

在包级别声明的变量会在 main 入口函数执行前完成初始化，局部变量将在声明语句被执行到的时候完成初始化

Go 没有隐式的数值转换，没有构造函数和析构函数，没有运算符重载，没有默认参数，也没有继承，没有异常，没有宏，没有函数修饰，没有线程局部存储

Go 没有三元运算符

创建百万级的 goroutine 完全是可行的

Go 有垃圾回收

命令行参数可从 os 包的 Args 变量获取，os.Args 是一个切片，其第一个元素，os.Args[0], 是命令本身的名字；其它的元素则是程序启动时传给它的参数
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
## make函数
```go
s := make([]string)
s = make([]string, 3)
s = make([]string, 3, 10)
```
## 指针
```go
// 与 C 不同，Go 没有指针运算
x := 1

p := &i
*p = 21
fmt.Println(*p)
fmt.Println("pointer:", p) // 指针也可以被打印
```
## new
```go
p := new(int) // p, *int 类型, 指向匿名的 int 变量
fmt.Println(*p) // "0"


func newInt() *int {
	return new(int)
}
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



// struct Embedding
type Point struct {
	X, Y int
}
// 匿名字段，即允许字段只有类型而没有名字
// 其实匿名字段是有名字的，名字和类型名相同，但是这些名字是可选的当使用点表达式时，可以省略任意多个
// 因为匿名字段实际有隐式的名字，所以不能有两个相同类型的字段，因为会导致名称冲突
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
// `json:"released"` 称为字段标签，默认会使用字段名作用 json 的键名，字段标签
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
## 反射
## unsafe
## 泛型
## 标准库
## strings
```go
strings.Join("hello", " world")
```