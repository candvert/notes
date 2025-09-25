```rust
// 对于集合可以将参数写成这种类型 args: &[String]
fn main() {
    let args: Vec<String> = env::args().collect();
    let (query, file_path) = parse_config(&args);
}

fn parse_config(args: &[String]) -> (&str, &str) {
    let query = &args[1];
    let file_path = &args[2];
    (query, file_path)
}







// 退出程序
use std::process;
process::exit(1);








// 多行文本
// 双引号之后的反斜杠告诉 Rust 不要在字符串字面值内容的开头加入换行符
let contents = "\
Rust:
safe, fast, productive.
Pick three.";




// std::env 模块包含了处理环境变量的实用功能




// eprintln! 将错误信息写入标准错误而不是标准输出
eprintln!("Application error!");





// 接收迭代器参数
let config = build(env::args()).unwrap_or_else(|err| {
	eprintln!("Problem parsing arguments: {err}");
	process::exit(1);
});
pub fn build(
	mut args: impl Iterator<Item = String>,
) -> i32 { }
```
## 自定义配置
```rust
// Cargo 有两个主要的配置：运行 cargo build 时采用的 dev 配置和运行 cargo build --release 的 release 配置


// 默认的优化等级
[profile.dev]
opt-level = 0
[profile.release]
opt-level = 3





// 文档注释使用三斜杠 /// 而不是双斜杠以支持 Markdown 注解来格式化文本

/// Adds one to the number given.
///
/// # Examples
///
/// ```
/// let arg = 5;
/// let answer = my_crate::add_one(arg);
///
/// assert_eq!(6, answer);
/// ```
pub fn add_one(x: i32) -> i32 {
    x + 1
}









// 文档注释//!

//! # My Crate
//!
//! `my_crate` is a collection of utilities to make performing certain
//! calculations more convenient.

```
## 工作空间
```rust
// 工作空间可以组织多个库
// 是工作空间只在根目录有一个 Cargo.lock，而不是在每一个 crate 目录都有 Cargo.lock
$ mkdir add
$ cd add
// 在 add 目录中，创建 Cargo.toml 文件，用于配置整个整个工作空间
[workspace]
resolver = "3"
// 接下来，在 add 目录运行 cargo new 新建 adder 二进制 crate
// 到此为止，可以运行 cargo build 来构建工作空间
// add 目录中的文件应该看起来像这样
├── Cargo.lock
├── Cargo.toml
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
// 工作空间在顶级目录只有一个 target 目录，用于存放编译生成的产物；adder 包并没有自己的 target 目录。即使进入 adder 目录运行 cargo build，构建结果也位于 add/target 而不是 add/adder/target






// 指定依赖的本地库
[dependencies]
add_one = { path = "../add_one" }


// 指定工作空间中我们希望运行的包
cargo run -p adder












// 所有来自 cargo install 的二进制文件都安装到 Rust 安装根目录的 bin 文件夹中
```