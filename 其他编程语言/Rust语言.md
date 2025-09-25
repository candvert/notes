- [安装](#安装)
- [Cargo命令](#Cargo命令)
- [Rustup命令](#Rustup命令)
- [使用vscode开发](#使用vscode开发)
- [Cargo.toml文件](#Cargo.toml文件)
- [示例](#示例)

- [重要的点](#重要的点)
- [重要概念](#重要概念)
- [没用的点但是面试会问](#没用的点但是面试会问)

- [变量](#变量)
- [静态变量](#静态变量)
- [常量](#常量)
- [数据类型](#数据类型)
- [强制类型转换](#强制类型转换)
- [条件语句](#条件语句)
- [循环语句](#循环语句)
- [match语句](#match语句)
- [所有权ownership](#所有权ownership)
- [String和&str](#String和&str)
- [枚举](#枚举)
- [Option枚举](#Option枚举)
- [函数](#函数)
- [结构体](#结构体)
- [方法](#方法)
- [不是方法的关联函数](#不是方法的关联函数)
- [crates](#crates)
- [modules](#modules)
- [将模块拆分成多个文件](#将模块拆分成多个文件)
- [path](#path)
- [use](#use)

- [vector](#vector)
- [String](#String)
- [HashMap](#HashMap)
- [错误处理](#错误处理)
- [?运算符](#?运算符)
- [泛型](#泛型)
- [trait](#trait)
- [生命周期](#生命周期)
- [自动化测试](#自动化测试)
- [闭包](#闭包)
- [迭代器](#迭代器)
## 安装
```sh
只需安装 Rustup，其会附带包管理器 Cargo 和 Rust 编译器 rustc
Windows 上还需要通过 Visual Studio Installer 安装 MSVC 和 Windwos 11 SDK 两项
```
![](/images/rust_1.png)
## Cargo命令
```sh
创建一个新项目
cargo new <project_name>
运行项目
cargo run
添加包
cargo add <package_name>
删除包
cargo rm <package_name>
编译项目
cargo build
检查项目有没有错误
cargo check
发布版本
cargo build --release
运行测试
cargo test
```
## Rustup命令
```sh
rustup --version
rustup --help
更新 rust 版本
rustup update
卸载
rustup self uninstall
查看文档
rustup doc
```
## 使用vscode开发
```sh
使用 vscode 开发 rust，只需要安装 rust-analyzer 插件
```
## Cargo.toml文件
```rust
rand = "0.8.5" // 添加依赖


当出现 panic 时，程序默认会开始 展开（unwinding），这意味着 Rust 会回溯栈并清理它遇到的每一个函数的数据，不过这个回溯并清理的过程有很多工作。如果你想要在 release 模式中 panic 时直接终止，可添加：
[profile.release]
panic = 'abort'
```
## 示例
```rust
// 文件名若包含多个单词，应当使用下划线来分隔单词。例如命名为 hello_world.rs
$ cargo new hello_cargo
$ cd hello_cargo
$ cd src
$ nvim main.rs
use std::io;

fn main() {
    let mut x = String::new();
    println!("Please enter:");
    io::stdin()
        .read_line(&mut x)
        .expect("Failed");
    println!("{x}");
}
$ cargo run
```
## 重要的点
```rust
变量默认是不可改变的
常量必须注明值的类型，常量只能被设置为常量表达式，而不可以是其他任何只能在运行时计算出的值
遮蔽可以改变值的类型和可变性
有符号数以二进制补码形式存储
整型默认类型是 i32
整型溢出：
	在 debug 模式编译时，Rust 检查这类问题并使程序 panic。panic 这个术语被 Rust 用来表明程序因错误而退出
	使用 --release 在 release 模式中构建时，Rust 不会检测会导致 panic 的整型溢出。相反发生整型溢出时会进行环绕操作
浮点型默认类型是 f64
整数除法会向零舍入到最接近的整数
Rust 的 char 类型的大小为四个字节 (four bytes)，并代表了一个 Unicode 标量值（Unicode Scalar Value）
元组长度固定
不带任何值的元组有个特殊的名称，叫做 单元（unit） 元组。这种值以及对应的类型都写作 ()，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值
Rust 中的数组长度是固定的，数组中的每个元素的类型必须相同
程序在索引操作中使用一个无效的值时导致运行时错误
Rust 代码中的函数和变量名使用 snake case 规范风格
在函数签名中，必须 声明每个参数的类型
函数体由一系列的语句和一个可选的结尾表达式构成
Rust 是一门基于表达式（expression-based）的语言：
	语句（Statements）是执行一些操作但不返回值的指令
	表达式（Expressions）计算并产生一个值
表达式可以是语句的一部分
函数调用是一个表达式。宏调用是一个表达式。用大括号创建的一个新的块作用域也是一个表达式
表达式的结尾没有分号。如果在表达式的结尾加上分号，它就变成了语句，而语句不会返回值
Rust 并不会尝试自动地将非布尔值转换为布尔值。必须总是显式地使用布尔值作为 if 的条件
所有权让 Rust 无需垃圾回收（garbage collector）即可保障内存安全
字符串字面值是硬编码进程序代码中的
就字符串字面值来说，我们在编译时就知道其内容，所以文本被直接硬编码进最终的可执行文件中
引用在其生命周期内保证指向某个特定类型的有效值
// 我自己总结的：生命周期的概念是为了保证引用总是有效的，也就是防止悬垂指针
“字符串 slice” 的类型声明写作 &str
```
## 重要概念
```rust
// 真正与其他语言不同的点也就是：所有权、引用（借用）、生命周期
// 所有权就是 C++ 的 RAII 和移动操作的结合，引用和 C++ 的引用差不多，而生命周期是为了保证引用总是有效的，防止悬垂指针

所有权
Option枚举，用来表示其他语言中的 null
package、crate、module、path
Result<T, E>，用来进行错误处理，用来表示其他语言中的 exception
trait
生命周期
闭包
宏
```
## 没用的点但是面试会问
```rust
字符串字面值就是 slice
还记得字符串字面值被储存在二进制文件中吗
let s = "Hello, world!";
这里 s 的类型是 &str：它是一个指向二进制程序特定位置的 slice。这也就是为什么字符串字面值是不可变的；&str 是一个不可变引用。
```
## 变量
```rust
// 使用 let 关键字来声明变量，支持类型推导
// 函数和变量名使用 snake case 规范风格，即所有字母都是小写并使用下划线分隔单词
// 枚举和结构体使用 Pascal Case 规范风格
// 在 Rust 中，变量默认是不可变的
let apples = 5;
// 可变
let mut bananas = 5;


// 遮蔽
// mut 与遮蔽的一个区别是，当再次使用 let 时，实际上创建了一个新变量，我们可以改变值的类型，改变可变性，并且复用这个名字
let spaces = " ";
let spaces = 5;
let x = 5;
let x = x + 1;
let mut guess = " ";
let guess: u32 = 5;
```
## 静态变量
```rust
// static 变量是在运行时分配内存的
// 可以使用 unsafe 修改
// 静态变量的生命周期为整个程序的运行周期
```
## 常量
```rust
// 常量只能被设置为常量表达式，而不可以是其他任何只能在运行时计算出的值
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
## 数据类型
```rust
// 整形
// 整型默认类型是 i32
// 整数除法会向零舍入到最接近的整数
// isize 和 usize 和架构相关，64 位架构上它们是 64 位的，32 位架构上它们是 32 位的
// 无符号
u8 u16 u32 u64 u128 usize
// 有符号
i8 i16 i32 i64 i128 isize

// 合法的整型字面值
十进制                  98_222
十六进制                0xff
八进制                  0o77
二进制                  0b1111_0000
单字节字符(仅限于u8)      b'A'
// 对于整数和浮点数可以通过添加后缀来指定类型
let a = 57u8;





// 浮点型
// 默认类型是 f64
f32 f64

// 布尔类型，值为 true 或 false
bool

// 字符类型
// char 类型的大小为四个字节，代表了一个 Unicode 值，所以可以存储中文、emoji 等
char


// 复合类型
// Rust 有两个原生的复合类型：元组（tuple）和数组（array）
// 元组长度固定
// 不带任何值的元组有个特殊的名称，叫做单元（unit） 元组。这种值以及对应的类型都写作 ()，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;
let five_hundred = tup.0;

// Rust 中的数组长度是固定的，数组中的每个元素的类型必须相同
// 无效的索引会导致运行时错误，而不是允许内存访问并继续执行
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5]; // 与 let a = [3, 3, 3, 3, 3]; 效果相同
let first = a[0];
```
## 强制类型转换
```rust

```
## 条件语句
```rust
// 条件必须是 bool 值，Rust 并不会尝试自动地将非布尔值转换为布尔值
if number % 4 == 0 {
	println!("number is divisible by 4");
} else if number % 3 == 0 {
	println!("number is divisible by 3");
} else {
	println!("number is not divisible by 4, 3, or 2");
}
// 在这句代码中，if 的每个分支的可能的返回值都必须是相同类型
let number = if condition { 5 } else { 6 };
```
## 循环语句
```rust
// loop 循环
loop {
	println!("again!");
}



// 返回值的 loop 循环
let mut counter = 0;
let result = loop {
	counter += 1;

	if counter == 10 {
		break counter * 2;
	}
};



// while 循环
let mut number = 3;
while number != 0 {
	println!("{number}!");
	number -= 1;
}



// for 循环
let a = [10, 20, 30, 40, 50];
for element in a {
	println!("the value is: {element}");
}



// 左闭右开区间，即 [)
for number in (1..4) {
	println!("{number}!");
}
// 反转范围
for number in (1..4).rev() {
	println!("{number}!");
}



// 将循环的标签与 break 或 continue 一起使用
let mut count = 0;
'counting_up: loop {
	let mut remaining = 10;

	loop {
		if remaining == 9 {
			break;
		}
		if count == 2 {
			break 'counting_up;
		}
		remaining -= 1;
	}

	count += 1;
}
```
## match语句
```rust
// match 的分支必须覆盖了所有的可能性
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        }
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}




// 匹配 Option<T>
fn plus_one(x: Option<i32>) -> Option<i32> {
	match x {
		None => None,
		Some(i) => Some(i + 1),
	}
}
let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);





// 通配模式
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	other => move_player(other),
}



// _ 占位符
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => reroll(),
}




// 使用单元值作为 _ 分支的代码，表示不想运行任何代码
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => (),
}





// if let
// 可以将 if let 看作 match 的一个语法糖，当值匹配某一模式时执行代码而忽略所有其他值
let config_max = Some(3u8);
if let Some(max) = config_max {
	println!("The maximum is configured to be {max}");
}
// if let 和 else 表达式
if let Coin::Quarter(state) = coin {
	println!("State quarter from {state:?}!");
} else {
	println!("hi");
}


// let...else语法，当模式匹配时将匹配到的值绑定到外层作用域，也就相当于定义了一个新变量，如下面的 state
let Coin::Quarter(state) = coin else {
	return None;
};
if state.existed_in(1900) {
	Some(format!("{state:?} is pretty old, for America!"))
} else {
	Some(format!("{state:?} is relatively new."))
}
```
## 所有权ownership
```rust
// 所有权让 Rust 无需垃圾回收即可保障内存安全
// 所有权实际就是将 C++ 中的 RAII 和移动操作结合起来，赋值变量时进行移动，变量离开作用域时进行 drop （C++中为 delete）
// 以 C++ 进行类比，所有权就是一个指针，指向分配在堆上的内存，拷贝变量时只会复制指针的值即浅拷贝，与 C++ 不同的是，拷贝后原先的变量不再有效。这样就可以防止出现两次 free 变量，即所有权规则中的“值在任一时刻有且只有一个所有者”
// 也可以看作为 C++ 中的移动
// 字符串字面值被储存在程序的二进制输出中，因此它们也是字符串 slices

// 所有权规则：
// 1. Rust 中的每一个值都有一个所有者（owner）
// 2. 值在任一时刻有且只有一个所有者
// 3. 当所有者离开作用域，这个值将被丢弃
let mut s = String::from("hello");
println!("{s}");
// 下面代码中 s1 的所有权转移到 s2 了，所以 s1 不再有效
let s1 = String::from("hello");
let s2 = s1;
println!("{s1}, world!"); // 报错





// 借用（也称引用），把拥有所有权的变量看作指针，则 Rust 中的借用相当于指针的指针
// 引用在其生命周期内保证指向某个特定类型的有效值
// （默认）不允许修改借用的值
let s1 = String::from("hello");
let len = calculate_length(&s1);
// 可变引用
fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
let mut s = String::from("hello");
change(&mut s);
// 在任意给定时间，要么只能有一个可变引用，要么只能有多个不可变引用
// 引用必须总是有效的，防止悬垂指针
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s; // 报错

let mut s = String::from("hello");
let r1 = &s; // 没问题
let r2 = &s; // 没问题
let r3 = &mut s; // 大问题

let mut s = String::from("hello");
let r1 = &s; // 没问题
let r2 = &s; // 没问题
println!("{r1} and {r2}");
// 此位置之后 r1 和 r2 不再使用
let r3 = &mut s; // 没问题
println!("{r3}");





// 切片（slice）允许你引用集合中一段连续的元素序列，而不用引用整个集合。slice 是一种引用，所以它不拥有所有权
// 字符串 slice
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6..11];
// 相同
let slice = &s[0..2];
let slice = &s[..2];
// 相同
let len = s.len();
let slice = &s[3..len];
let slice = &s[3..];
// 相同
let len = s.len();
let slice = &s[0..len];
let slice = &s[..];
// 数组 slice
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];
```
## String和&str
String 实际存储的是指向下标为0的元素的指针和字符串长度，以及 capacity
&str 实际存储的是指向某个元素的指针和该切片的长度
```rust
let s = String::from("hello world");
let world = &s[6..];
```
![[rust_2.svg]]
## 枚举
```rust
// 定义
enum IpAddrKind {
    V4,
    V6,
}
// 使用
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
fn route(ip_kind: IpAddrKind) {}
route(IpAddrKind::V4);
route(IpAddrKind::V6);



// 
enum IpAddr {
	V4(String),
	V6(String),
}
let home = IpAddr::V4(String::from("127.0.0.1"));
let loopback = IpAddr::V6(String::from("::1"));




// 
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}
let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));




// 
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
impl Message {
	fn call(&self) {
		// 在这里定义方法体
	}
}
let m = Message::Write(String::from("hello"));
m.call();
```
## Option枚举
```rust
// Rust 并没有很多其他语言中有的空值功能
// 不过拥有一个可以编码存在或不存在概念的枚举
// 这个枚举是 Option<T>，而且它定义于标准库中
// Option<T> 枚举是如此有用以至于它甚至被包含在了 prelude 之中，无需将其显式引入作用域
// 另外，它的变体也是如此：可以不需要 Option:: 前缀来直接使用 Some 和 None

// 标准库中的定义
enum Option<T> {
    None,
    Some(T),
}
// 示例
let some_number = Some(5);
let some_char = Some('e');
let absent_number: Option<i32> = None;
// 下面代码中因为 x 和 y 的类型不同，所以 x + y 会报错
// 这样只要一个值不是 Option<T> 类型，你就可以安全的认定它的值不为空
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // 报错
```
## 函数
```rust
// 语句不返回值。表达式计算并产生一个值
// 表达式可以是语句的一部分：例如语句 let y = 6; 中的 6 是一个表达式，它计算出的值是 6。函数调用是一个表达式。宏调用是一个表达式。用大括号创建的一个新的块作用域也是一个表达式
// 如果在 five 函数中的 5 后面加上一个分号，把它从表达式变成语句，我们将看到一个错误，因为语句不返回值
// fn five() -> i32 {
//    5
//}



// 在函数签名中，必须声明每个参数的类型
fn another_function() {
    println!("Another function.");
}

fn print_labeled_measurement(value: i32, unit_label: char, s: &String) {
    println!("The measurement is: {value}{unit_label}");
	5
}



// 在 Rust 中，函数的返回值等同于函数体最后一个表达式的值
fn five() -> i32 {
    5
}
```
## 结构体
```rust
// 定义
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
// 实例化
let mut user1 = User {
	username: String::from("someusername123"),
	email: String::from("someone@example.com"),
	sign_in_count: 1,
	active: true,
};
user1.email = String::from("anotheremail@example.com");




// 字段初始化简写语法，因为 username 和 email 参数与结构体字段同名
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}


// 结构体更新语法，使用旧实例的大部分值但改变其部分值来创建一个新的结构体实例
let user2 = User {
	email: String::from("another@example.com"),
	..user1
};




// 元组结构体
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
	let Point(x, y, z) = origin;
}



// 类单元结构体
struct AlwaysEqual;
fn main() {
    let subject = AlwaysEqual;
}
```
## 方法
```rust
// 所有在 impl 块中定义的函数被称为关联函数
// 方法的第一个参数总是 self，它代表调用该方法的结构体实例
// 在 impl 中不以 self 作为参数的函数被称为不是方法的关联函数
// 每个结构体都允许拥有多个 impl 块
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}


// 泛型方法
struct Game<T> {
	name: String
}
impl<T> Game<T> {
	fn play(&self) {
		println!("I'm playing");
	}
}
```
## 不是方法的关联函数
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
	// 在 impl 中不以 self 作为参数的函数就是不是方法的关联函数
	fn say() {
		println!("Rectangle");
	}

    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```
## crates
```rust
创建一个库：cargo new --lib read
crates 分为两种：二进制 crate 和库 crate
库 crate 与其他编程语言中 “library” 概念一致
包（package）是提供一系列功能的一个或者多个 crate 的捆绑。一个包会包含一个 Cargo.toml 文件，阐述如何去构建这些 crate
一个包可以包含多个二进制 crate 和一个可选的库 crate
当编译一个 crate, 编译器首先在 crate 根文件（通常，对于一个库 crate 而言是 src/lib.rs，对于一个二进制 crate 而言是 src/main.rs）中寻找需要被编译的代码
一个包可以通过将文件放入 src/bin 目录来包含多个二进制 crate，每个文件都是一个独立的二进制 crate




声明模块: 在 crate 根文件中，你可以声明一个新模块；比如，用 mod garden; 声明了一个叫做 garden 的模块。编译器会在下列路径中寻找模块代码：
内联，用大括号替换 mod garden 后跟的分号
在文件 src/garden.rs
在文件 src/garden/mod.rs



声明子模块: 在除了 crate 根节点以外的任何文件中，你可以定义子模块。比如，你可能在 src/garden.rs 中声明 mod vegetables;。编译器会在以父模块命名的目录中寻找子模块代码：
内联，直接在 mod vegetables 后方不是一个分号而是一个大括号
在文件 src/garden/vegetables.rs
在文件 src/garden/vegetables/mod.rs
```
## modules
```rust
//  Rust 中，所有项（函数、方法、结构体、枚举、模块和常量）默认对父模块都是私有的
// 如果我们在一个结构体定义的前面使用了 pub，这个结构体会变成公有的，但是这个结构体的字段仍然是私有的
// 如果我们将枚举设为公有，则它的所有变体都将变为公有
// 父模块中的项不能使用子模块中的私有项，但是子模块中的项可以使用它们父模块中的项
// lib.rs 文件：
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() { println!("hi"); }
        fn seat_at_table() { println!("hi"); }
    }
    mod serving {
        fn take_order() { println!("hi"); }
        fn serve_order() { println!("hi"); }
        fn take_payment() { println!("hi"); }
    }
}
mod back_of_house {
	mod cooking {
		fn cook() { println!("hi"); }
	}
}
// 该库 crate 定义了 front_of_house 模块和 back_of_house 模块
// 该库 crate 的模块树
// 注意，整个模块树都植根于名为 crate 的隐式模块下
crate
 ├── front_of_house
 │  ├── hosting
 │  │   ├── add_to_waitlist
 │  │   └── seat_at_table
 │  └── serving
 │     ├── take_order
 │     ├── serve_order
 │     └── take_payment
 └── back_of_house
    └── cooking
	   └── seat_at_table
```
## 将模块拆分成多个文件
```rust
// src/lib.rs 文件原本的内容：
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() { println!("hi"); }
    }
}
// src/lib.rs
mod front_of_house;
// src/front_of_house.rs
pub mod hosting;
// src/front_of_house/hosting.rs
pub fn add_to_waitlist() { println!("hi"); }
```
## path
```rust
// path 包括绝对路径和相对路径
// lib.rs 文件：
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
pub fn eat_at_restaurant() {
    // 绝对路径
    crate::front_of_house::hosting::add_to_waitlist();
    // 相对路径
    front_of_house::hosting::add_to_waitlist();
}


// super 开始的相对路径
fn deliver_order() {}
mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::deliver_order();
    }

    fn cook_order() {}
}
```
## use
```rust
// 使用 as 关键字取别名
use std::io::Result as IoResult;


// 重导出，和 js 的 export 相似，导出一个名称
pub use std::io::Result as IoResult;



// 嵌套路径
use std::cmp::Ordering;
use std::io;
use std::{cmp::Ordering, io};
// 嵌套路径
use std::io;
use std::io::Write;
use std::io::{self, Write};
// 导入所有项
use std::collections::*;
```
## vector
```rust
// 新建一个空 vector
let v: Vec<i32> = Vec::new();
// 使用 vec! 宏创建 vector
let v = vec![1, 2, 3];
// 添加元素
v.push(5);
// 移除元素
v.pop();
// 访问元素
let third: &i32 = &v[2];
// 使用 get 访问元素，当 get 方法被传递了一个数组外的索引时，会返回 None
let third: Option<&i32> = v.get(2);


// 错误的使用
// 在 vector 的结尾增加新元素时，在没有足够空间将所有元素依次相邻存放的情况下，可能会要求分配新内存并将老的元素拷贝到新的空间中。这时，第一个元素的引用就指向了被释放的内存。借用规则阻止程序陷入这种状况
let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0];
v.push(6);
println!("The first element is: {first}"); // 报错



// 遍历
let v = vec![100, 32, 57];
for i in &v {
	println!("{i}");
}
// 遍历
let mut v = vec![100, 32, 57];
for i in &mut v {
	*i += 50;
}
```
## String
```rust
// String 是一个 Vec<u8> 的封装

// 新建一个空 String
let mut s = String::new();
// 使用 to_string 方法从字符串字面值创建 String
let data = "initial contents";
let s = data.to_string();
// 该方法也可直接用于字符串字面值：
let s = "initial contents".to_string();
// 使用 String::from 函数从字符串字面值创建 String
let s = String::from("initial contents");
// 附加字符串
s.push_str("bar");
// 使用 push 将一个字符加入 String 值中
s.push('l');
// 使用 + 运算符将两个 String 值合并到一个新的 String 值中
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // 注意 s1 被移动了，不能继续使用


// 使用 format! 宏
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{s1}-{s2}-{s3}");



// Rust 的字符串不支持索引
// 因为字符串是使用 UTF-8 编码的，不能保证一个字节存储一个字符
let hello = "вствуйте";
let s = &hello[0..1]; // 报错，因为这些字母都是两个字节长的



// 使用 chars 函数来访问每个字符
for c in "Зд".chars() {
    println!("{c}");
}
// bytes 方法返回每一个原始字节
for b in "Зд".bytes() {
    println!("{b}");
}
```
## HashMap
```rust
// 所有的键必须是相同类型，值也必须都是相同类型
// 每个唯一的键只能同时关联一个值
// 对于像 String 这样拥有所有权的值，其值将被移动而哈希 map 会成为这些值的所有者
// HashMap 默认使用一种叫做 SipHash 的哈希函数，它可以抵御涉及哈希表的拒绝服务（Denial of Service, DoS）攻击。可以指定一个不同的 hasher 来切换为其它哈希函数。hasher 是一个实现了 BuildHasher trait 的类型
// 如果我们插入了一个键值对，接着用相同的键插入一个不同的值，与这个键相关联的旧值将被替换


// 新建一个哈希 map
use std::collections::HashMap;
let mut scores = HashMap::new();

// 插入值
scores.insert(String::from("Blue"), 10);

// 获取值，get 方法返回 Option<&V>，如果某个键在哈希 map 中没有对应的值，get 会返回 None
let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);

// 遍历
for (key, value) in &scores {
	println!("{key}: {value}");
}

// 使用 entry 方法只在键没有对应值时插入
// Entry 的 or_insert 方法在键对应的值存在时就返回这个值的可变引用，如果不存在则将参数作为新值插入并返回新值的可变引用
scores.entry(String::from("Yellow")).or_insert(50);
```
## 错误处理
```rust
// Rust 将错误分为两大类：可恢复的（recoverable）和不可恢复的（unrecoverable）错误。
// Rust 没有异常。相反，它有 Result<T, E> 类型，用于处理可恢复的错误，还有 panic! 宏，在程序遇到不可恢复的错误时停止执行
// 有两种方法造成 panic：执行会造成代码 panic 的操作（比如访问超过数组结尾的内容）或者显式调用 panic! 宏
// backtrace 是一个执行到目前位置所有被调用的函数的列表


// 主动调用 panic! 宏
fn main() {
    panic!("crash and burn");
}




// Result 枚举和其变体被导入到了 prelude 中
// 标准库中的 Result 枚举
enum Result<T, E> {
    Ok(T),
    Err(E),
}




// 如果 Result 值是变体 Ok，unwrap 会返回 Ok 中的值。如果 Result 是变体 Err，unwrap 会为我们调用 panic!
let greeting_file = File::open("hello.txt").unwrap();
// expect
let greeting_file = File::open("hello.txt")
	.expect("hello.txt should be included in this project");
```
## ?运算符
```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
	// 如果成功，代码继续；若出错，直接返回错误 Err
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}


// 简化上面代码
File::open("hello.txt")?.read_to_string(&mut username)?;



// 在 Option<T> 值上使用 ? 运算符
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}




// main 函数返回错误
// 可以将 Box<dyn Error> 理解为 “任何类型的错误”
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```
## 泛型
```rust
// 函数
fn largest<T>(list: &[T]) -> &T { }

// 结构体
struct Point<T> {
    x: T,
    y: T,
}

// 枚举
enum Result<T, E> {
    Ok(T),
    Err(E),
}

// 在 Point<T> 结构体上实现方法 x，它返回 T 类型的字段 x 的引用
struct Point<T> {
    x: T,
    y: T,
}
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
// 只实现 Point<f32> 类型上的方法
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}






// 方法使用了与结构体定义中不同类型的泛型
struct Point<X1, Y1> {
    x: X1,
    y: Y1,
}

impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c' };

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```
## trait
```rust
// trait 类似于其他语言中的接口（interfaces）
trait Person {
	fn get_name(&self) -> String;
	fn say(&self) {
		println!("Hello, {}", self.get_name());
	}
}
struct Player {
	name: String
}
impl Person for Player {
	fn get_name(&self) -> String {
		self.name.clone()
	}
}



// item 参数支持任何实现了 Summary trait 的类型
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
// Trait Bound 语法，这种更冗长的写法与上面的代码等价
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}


// item 参数支持任何实现了 Summary trait 和 Display trait 的类型
pub fn notify(item: &(impl Summary + Display)) { }
pub fn notify<T: Summary + Display>(item: &T) { }



// Trait Bound 语法
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 { }
// 通过 where 简化 trait bound
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{ }



// 返回实现了 trait 的类型
fn returns_summarizable() -> impl Summary {
    SocialPost {
        username: String::from("horse_ebooks"),
    }
}
// 但不能返回不同类型，下面这段代码是错误的
fn returns_summarizable(switch: bool) -> impl Summary {
    if switch {
        NewsArticle {
            headline: String::from("Penguin"),
        }
    } else {
        SocialPost {
            username: String::from("horse_ebooks"),
    }
}





// 使用 trait bound 有条件地实现方法
use std::fmt::Display;
struct Pair<T> {
    x: T,
    y: T,
}
// 只有那些为 T 类型实现了 Display trait 和 PartialOrd trait 的 Pair<T> 才会实现 cmp_display 方法：
impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}




// 标准库为任何实现了 Display trait 的类型实现了 ToString trait
impl<T: Display> ToString for T {
    // --snip--
}
```
## 生命周期
```rust
// 生命周期是另一类泛型
// 生命周期用于保证引用在我们需要的整个期间内都是有效的
// Rust 中的每一个引用都有其生命周期（lifetime），也就是引用保持有效的作用域
// 生命周期的主要目标是避免悬垂引用
// Rust 编译器有一个借用检查器（borrow checker），它比较作用域来确保所有的借用都是有效的
// 生命周期参数名称必须以撇号（'）开头，其名称通常全是小写
// 函数或方法的参数的生命周期被称为输入生命周期（input lifetimes），而返回值的生命周期被称为输出生命周期（output lifetimes）
// 当从函数返回一个引用，返回值的生命周期参数需要与一个参数的生命周期参数相匹配


// 函数签名表达如下限制：也就是这两个参数和返回的引用存活的一样久。（两个）参数和返回的引用的生命周期是相关的
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}



// 结构体定义中的生命周期注解
struct ImportantExcerpt<'a> {
    part: &'a str,
}


// impl 块
impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}



// 有一种特殊的生命周期：'static，其生命周期能够存活于整个程序期间。所有的字符串字面值都拥有 'static 生命周期，我们也可以选择像下面这样标注出来：
let s: &'static str = "I have a static lifetime.";





// 在同一函数中指定泛型类型参数、trait bounds 和生命周期的语法
use std::fmt::Display;
fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {ann}");
    if x.len() > y.len() { x } else { y }
}
```
## 自动化测试
```rust
// Rust 中的测试就是一个带有 test 属性注解的函数。属性（attribute）是 Rust 代码片段的元数据
// cargo test 命令会运行项目中所有的测试
// 当测试函数中出现 panic 时测试就失败了。每一个测试都在一个新线程中运行，当主线程发现测试线程异常了，就将对应测试标记为失败
// assert! 宏由标准库提供，需要向 assert! 宏提供一个求值为布尔值的参数。如果值是 true，assert! 什么也不做，同时测试会通过。如果值为 false，assert! 调用 panic! 宏
// 为了将一个函数变成测试函数，需要在 fn 行之前加上 #[test]
pub fn add(left: u64, right: u64) -> u64 {
    left + right
}
#[test]
fn it_works() {
	let result = add(2, 2);
	assert_eq!(result, 4);
}



// 对于自定义的结构体和枚举，需要实现 PartialEq 才能断言它们的值是否相等。需要实现 Debug 才能在断言失败时打印它们的值。因为这两个 trait 都是派生 trait，通常可以直接在结构体或枚举上添加 #[derive(PartialEq, Debug)] 注解
// assert_eq! 宏
assert_eq!(result, 4);
// assert_ne! 宏
assert_ne!(result, 4);
// 自定义失败信息
assert!(
	result.contains("Carol"),
	"Greeting did not contain name, value was `{result}`"
);





// 在测试中使用 Result<T, E>
#[test]
fn it_works() -> Result<(), String> {
	let result = add(2, 2);

	if result == 4 {
		Ok(())
	} else {
		Err(String::from("two plus two does not equal four"))
	}
}



// 当运行多个测试时，Rust 默认使用线程来并行运行
// 这里将测试线程设置为 1，告诉程序不要使用任何并行机制
$ cargo test -- --test-threads=1
// 显示成功测试的输出
$ cargo test -- --show-output
// 测试单个函数
$ cargo test <function_name>
// 运行了所有名字中带有 add 的测试
$ cargo test add
// 忽略测试，在函数前添加 #[ignore] 属性
#[test]
#[ignore]
fn expensive_test() {
	// code that takes an hour to run
}
// 只运行被忽略的测试
$ cargo test -- --ignored
// 不管是否忽略都要运行全部测试
$ cargo test -- --include-ignored






// 单元测试
// 单元测试位于与源码相同的文件中
// 规范是在每个文件中创建包含测试函数的 tests 模块，并使用 cfg(test) 标注模块





// 集成测试
// 为了编写集成测试，需要在项目根目录创建一个 tests 目录，与 src 同级
// Cargo 会将该目录中的每一个文件当作单独的 crate 来编译
// tests 目录中的子目录不会被作为单独的 crate 编译或作为一个测试结果部分出现在测试输出中
// 如果项目是二进制 crate 并且只包含 src/main.rs 而没有 src/lib.rs，这样就不可能在 tests 目录创建集成测试
// 如果一个单元测试失败，则不会有任何集成测试和文档测试的输出，因为这些测试只会在所有单元测试都通过后才会执行

// 运行某个特定集成测试文件中的所有测试
// 这个命令只运行 tests/integration_test.rs 文件中的测试
$ cargo test --test integration_test
```
## 闭包
```rust
// Option<T> 上的 unwrap_or_else 方法由标准库定义。它接受一个无参闭包作为参数。如果 Option<T> 是 Some 变体，则 unwrap_or_else 返回 Some 中的值。如果 Option<T> 是 None 变体，则 unwrap_or_else 调用闭包并返回闭包的返回值


// 闭包语法
let add_one_v2 = |x: u32| -> u32 { x + 1 };
let add_one_v3 = |x|             { x + 1 };
let add_one_v4 = |x|               x + 1  ;



// 对于闭包定义，编译器会为每个参数和返回值推断出一个具体类型
// 第一次使用 String 值调用 example_closure 时，编译器推断出 x 的类型以及闭包的返回类型为 String。接着这些类型被锁定进闭包 example_closure 中，如果尝试对同一闭包使用不同类型则就会得到类型错误
let example_closure = |x| x;
let s = example_closure(String::from("hello"));
let n = example_closure(5); // 报错



// 使用 move 来强制闭包为线程获取 list 的所有权
let list = vec![1, 2, 3];
thread::spawn(move || println!("From thread: {list:?}"))
	.join()
	.unwrap();




// 闭包会自动、渐进地实现一个、两个或全部三个 Fn trait
// FnOnce 适用于只能被调用一次的闭包。所有闭包至少都实现了这个 trait，因为所有闭包都能被调用。一个会将捕获的值从闭包体中移出的闭包只会实现 FnOnce trait，因为它只能被调用一次
// FnMut 适用于不会将捕获的值移出闭包体，但可能会修改捕获值的闭包。这类闭包可以被调用多次
// Fn 适用于既不将捕获的值移出闭包体，也不修改捕获值的闭包，同时也包括不从环境中捕获任何值的闭包。这类闭包可以被多次调用而不会改变其环境







// 使用 sort_by_key 对长方形按宽度排序
let mut list = [
	Rectangle { width: 10, height: 1 },
	Rectangle { width: 3, height: 5 },
	Rectangle { width: 7, height: 12 },
];
list.sort_by_key(|r| r.width);
```
## 迭代器
```rust
// 迭代器都实现了名为 Iterator 的定义于标准库的 trait
// 如果我们需要一个获取所有权并返回拥有所有权的迭代器，则可以调用 into_iter 而不是 iter。类似地，如果我们希望迭代可变引用，可以调用 iter_mut 而不是 iter
// 创建一个迭代器
let v1 = vec![1, 2, 3];
let mut v1_iter = v1.iter();

v1_iter.next()




// 因为所有的迭代器都是惰性的，你必须调用一个消费适配器方法，才能从这些迭代器适配器的调用中获取结果
// 调用迭代器适配器 map 方法创建一个新迭代器，接着调用 collect 方法消费新迭代器并创建一个 vector
let v1: Vec<i32> = vec![1, 2, 3];
let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();
```