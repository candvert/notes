- [安装](#安装)
- [Cargo命令](#Cargo命令)
- [Rustup命令](#Rustup命令)
- [使用vscode开发](#使用vscode开发)
- [Cargo.toml文件](#Cargo.toml文件)
- [示例](#示例)

- [变量](#变量)
- [常量](#常量)
- [数据类型](#数据类型)
- [条件语句](#条件语句)
- [循环语句](#循环语句)
- [所有权ownership](#所有权ownership)
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
// 在 Rust 中，变量默认是不可变的
// 不可变
let apples = 5;
// 可变
let mut bananas = 5;


// 遮蔽
// mut 与遮蔽的一个区别是，当再次使用 let 时，实际上创建了一个新变量，我们可以改变值的类型，并且复用这个名字
let spaces = " ";
let spaces = 5;
// 重复使用 let 关键字进行遮蔽
let x = 5;
let x = x + 1;
// 
let mut guess = " ";
let guess: u32 = 5;
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
// 不带任何值的元组有个特殊的名称，叫做 单元（unit） 元组。这种值以及对应的类型都写作 ()，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值
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
## 所有权ownership
```rust
// 所有权让 Rust 无需垃圾回收即可保障内存安全
```
## 函数
```rust
// 语句不返回值。表达式计算并产生一个值
// 表达式可以是语句的一部分：例如语句 let y = 6; 中的 6 是一个表达式，它计算出的值是 6。函数调用是一个表达式。宏调用是一个表达式。用大括号创建的一个新的块作用域也是一个表达式
// 如果在 five 函数中的 5 后面加上一个分号，把它从表达式变成语句，我们将看到一个错误，因为语句不返回值
// fn five() -> i32 {
//    5
//}


// Rust 代码中的函数和变量名使用 snake case 规范风格，即所有字母都是小写并使用下划线分隔单词
// 在函数签名中，必须声明每个参数的类型
fn another_function() {
    println!("Another function.");
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
print_labeled_measurement(5, 'h');



// 在 Rust 中，函数的返回值等同于函数体最后一个表达式的值
fn five() -> i32 {
    5
}
```