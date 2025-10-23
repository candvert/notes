- [创建并运行项目](#创建并运行项目)
- [运行单个文件](#运行单个文件)
- [添加和删除包](#添加和删除包)
- [顶级语句](#顶级语句)
- [示例](#示例)
- [dotnet命令](#dotnet命令)
- [数据类型](#数据类型)
- [注释](#注释)
- [变量和常量](#变量和常量)
- [类型转换](#类型转换)
- [格式化输出](#格式化输出)
- [定义对象](#定义对象)
- [main方法](#main方法)

c#不能在类外定义函数和变量
c#有垃圾回收
## 创建并运行项目
```sh
# dotnet new console 创建控制台项目，-n 选项用于指定项目名称
dotnet new console -n my_project
cd my_project
dotnet run
```
## 运行单个文件
```sh
# .NET 10 及之后的版本才可以使用
dotnet run file.cs
```
## 添加和删除包
```sh
dotnet add package <PACKAGE_NAME>
dotnet remove package <PACKAGE_NAME>

dotnet list package
```
## 顶级语句
使用顶级语句使得无需将代码放在类或方法中，在这种情况下，编译器会为应用程序生成一个带有入口点方法的 Program 类

一个项目只能有一个带有顶级语句的文件

对于包含顶级语句的文件：
- 可以显式编写 Main 方法，但它不能用作入口点，编译器会发出警告
- using 指令必须位于文件顶部
- 顶级语句隐含在全局命名空间中
- 可以包含命名空间和类型定义，但它们必须位于顶级语句之后
- 可以使用 await 调用异步方法
```c#
using System;
using System.Threading.Tasks;

Console.Write("Hello ");
await Task.Delay(5000);
Console.WriteLine("World!");

return 0;

// public class MyClass { }
// namespace MyNamespace { }
```
## 示例
C# 程序由一个或多个文件组成。每个文件包含零个或多个命名空间。命名空间包含类、结构、接口、枚举和委托等类型或其他命名空间。
下面的示例是包含所有这些元素的 C# 程序的骨架
```c#
// A skeleton of a C# program
using System;

namespace YourNamespace
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello world!");
        }
    }
	
    class YourClass
    {
    }

    struct YourStruct
    {
    }

    interface IYourInterface
    {
    }

    delegate int YourDelegate();

    enum YourEnum
    {
    }

    namespace YourNestedNamespace
    {
        struct YourStruct
        {
        }
    }
}
```
## dotnet命令
```sh
# 构建项目
dotnet build
# 运行项目
dotnet run
# 运行单个文件，.NET 10 及之后的版本才可以使用
dotnet run file.cs
```
## 数据类型
```c#
// 值类型
short, int, long, float, double
bool, char, byte
ushort, uint, ulong, decimal, nint, nuint, sbyte
// 引用类型
object, string, delegate, dynamic



// C# 类型关键字都是相应 .NET 类型的别名
// int a = 10; 和 System.Int32 a = 10; 是完全相同的



// 对应的 .NET 值类型：
System.Int16, System.Int32, System.Int64, System.Single, System.Double
System.Boolean, System.Char, System.Byte
System.UInt16, System.UInt32, System.UInt64, System.Decimal, System.IntPtr, System.UIntPtr, System.SByte
// 对应的 .NET 引用类型：
System.Object, System.String, System.Delegate, System.Object



// 为何采用这种设计：
// .NET 框架支持多种编程语言（如C#、VB、F#），它们都使用同一套基础类型系统。在 C# 中叫 int，在 VB 中可能叫 Integer，但它们最终都对应到 System.Int32
```
## 注释
```c#
// this is comment

/* this is
comment */
```
## 变量和常量
```c#
int i = 10;
// c#中可以使用var关键字进行类型自动推断
var i = 10;
// 常量
const int i = 10;
```
## 类型转换
```c#
int i = 10;
byte b = (byte)i;

string s = "1";
int i = Convert.ToInt32(s);
int j = int.Parse(s);
```
## 格式化输出
```c#
int i = 4, j = 8;
Console.WriteLine("i={0}, j={1}", i, j);
```
## 定义对象
```c#
class Person {
    public int Age;
}
var person = new Person() {Age = 20};
```
## main方法
Main 必须在类或结构中进行声明。 该类可以是 static
Main 必须为 static
Main 可以具有任何访问修饰符（file 除外）
Main 的返回类型可以是 void、int、Task 或 `Task<int>`
当且仅当Main返回Task或 `Task<int>` 时，声明Main可以包含async修饰符。 此规则专门排除方法 `async void Main`
Main 方法可以使用或不使用包含命令行参数的 string[] 参数来声明，与 C 和 C++ 不同，程序名称不被视为第一个命令行参数

以下列表显示了最常见的 Main 声明：
```c#
static void Main() { }
static int Main() { }
static void Main(string[] args) { }
static int Main(string[] args) { }
static async Task Main() { }
static async Task<int> Main() { }
static async Task Main(string[] args) { }
static async Task<int> Main(string[] args) { }
```
