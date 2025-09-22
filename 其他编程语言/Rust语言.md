- [安装](#安装)
- [Cargo命令](#Cargo命令)
- [Rustup命令](#Rustup命令)
- [使用vscode开发](#使用vscode开发)
- [Cargo.toml文件](#Cargo.toml文件)
- [示例](#示例)

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
- [结构体](#结构体)
- [方法](#方法)
- [函数](#函数)
## 安装
```sh
只需安装 Rustup，其会附带包管理器 Cargo 和 Rust 构建工具
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

cargo build --release
```
## Rustup命令
```sh
rustup --version
rustup --help
更新 rust 版本
rustup update
卸载
rustup self uninstall
rustup doc
```
## 使用vscode开发
```sh
使用 vscode 开发 rust，只需要安装 rust-analyzer 插件
```
## Cargo.toml文件
```rust

```
## 示例
```rust
// 文件名包含多个单词，应当使用下划线来分隔单词。例如命名为 hello_world.rs
// rustc main.rs
// ./main
$ cargo new hello_cargo
$ cd hello_cargo
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
// 不带任何值的元组有个特殊的名称，叫做单元（unit） 元组。这种值以及对应的类型都写作 ()，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值。
let tup: (i32, f64, u8) = (500, 6.4, 1);
let five_hundred = tup.0;
let (x, y, z) = tup;
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
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
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
}



// 
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	other => move_player(other),
}



// 
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => reroll(),
}



// if let
let mut count = 0;
if let Coin::Quarter(state) = coin {
	println!("State quarter from {state:?}!");
} else {
	count += 1;
}
```
## 所有权ownership
```rust
// 所有权让 Rust 无需垃圾回收即可保障内存安全
// 以 C++ 进行类比，所有权就是一个指针，指向分配在堆上的内存，拷贝变量时只会复制指针的值即浅拷贝，与 C++ 不同的是，拷贝后原先的变量不再有效。这样就可以防止出现两次 free 变量，即所有权规则中的“值在任一时刻有且只有一个所有者”

// 所有权规则：
// 1. Rust 中的每一个值都有一个所有者（owner）
// 2. 值在任一时刻有且只有一个所有者
// 3. 当所有者离开作用域，这个值将被丢弃
let mut s = String::from("hello");
s.push_str(", world!"); // push_str() 在字符串后追加字面值
println!("{s}"); // 将打印 `hello, world!`
// Rust 中的 String 会进行浅拷贝，与 C++ 不同的是，拷贝后原先的变量不再有效，所以如果使用下面的变量 s1 会报错
let s1 = String::from("hello");
let s2 = s1;
println!("{s1}, world!"); // 报错





// 借用（也称引用），把所有权的变量看作指针，则 Rust 中的借用相当于指针的指针
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
// 可变引用有一个很大的限制：如果你有一个对该变量的可变引用，你就不能再创建对该变量的引用
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s; // 报错
// 也不能在拥有不可变引用的同时拥有可变引用
let mut s = String::from("hello");
let r1 = &s; // 没问题
let r2 = &s; // 没问题
let r3 = &mut s; // 大问题





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
assert_eq!(slice, &[2, 3]);
```
## String和&str
String 实际存储的是指向下标为0的元素的指针和字符串长度，以及 capacity
&str 实际存储的是指向某个元素的指针和该切片的长度
```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```
![[rust_2.svg]]
## 枚举
```rust
enum IpAddrKind {
    V4,
    V6,
}
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
struct Ipv4Addr {
    // --snip--
}
struct Ipv6Addr {
    // --snip--
}
enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}


// 
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```
## Option枚举
```rust
// Rust 并没有很多其他语言中有的空值功能
// 不过拥有一个可以编码存在或不存在概念的枚举
// 这个枚举是 Option<T>，而且它定义于标准库中
// Option<T> 枚举是如此有用以至于它甚至被包含在了 prelude 之中，无需将其显式引入作用域
enum Option<T> {
    None,
    Some(T),
}
// 保证安全，因为 x 和 y 的类型不同，所以 x + y 会报错
// 这样只要一个值不是 Option<T> 类型，你就可以安全的认定它的值不为空
let x: i8 = 5;
let y: Option<i8> = Some(5);

let sum = x + y; // 报错
```
## 结构体
```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
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
}

// 类单元结构体
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```
## 方法
```rust
#[derive(Debug)]
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
}
print_labeled_measurement(5, 'h');



// 在 Rust 中，函数的返回值等同于函数体最后一个表达式的值
fn five() -> i32 {
    5
}
```