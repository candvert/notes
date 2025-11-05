- [创建并运行项目](#创建并运行项目)
- [运行单个文件](#运行单个文件)
- [添加和删除包](#添加和删除包)
- [顶级语句](#顶级语句)
- [示例](#示例)
- [dotnet命令](#dotnet命令)
- [访问限制](#访问限制)
- [数据类型](#数据类型)
- [注释](#注释)
- [变量](#变量)
- [常量](#常量)
- [using指令](#using指令)
- [类型转换](#类型转换)
- [格式化输出](#格式化输出)
- [定义对象](#定义对象)
- [main方法](#main方法)
- [原始字符串](#原始字符串)
- [控制流](#控制流)
	- [if](#if)

c# 中的每个项目在编译后都会生成一个独立的程序集（即编译后的 .dll 或 .exe 文件）

c# 有垃圾回收

在 c# 中，如果源文件分布在不同的目录，但只要它们属于同一个项目，可以直接使用 using 指令导入其他源文件中定义的命名空间

c# 要求所有的变量和函数（方法）都必须属于某个类或结构体

任何未显式包裹在 namespace 内的类、结构等类型，默认都属于​​全局命名空间

直接在命名空间级别定义的类、结构、接口等类型，其访问权限只能设置为 public 或 internal

internal 限定的类型只能在同一个程序集内部被访问

struct 不支持继承

在 c# 中 bool 不会隐式转换为 int

## 程序入口
程序入口是某个类中的 Main 函数（类的名称无所谓）
```c#
using System;

class Program
{
	static void Main(string[] args)
	{
		Console.WriteLine("Hello world!");
	}
}
```
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

// 可以通过 args 访问命令行参数
foreach (var arg in args)
{
    Console.Write("Hello ");
}

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
## 访问限制
```c#
internal 限定的类型只能在同一个程序集内部被访问

类默认权限为 ​​internal。其成员默认为 private
结构默认访问权限为 internal。其成员默认为 private

接口默认访问权限为 internal。其成员默认为 public​​，且不允许显式添加访问修饰符
枚举默认访问权限为 internal。其成员默认为 public，且不允许显式添加访问修饰符
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
// 例如 int a = 10; 和 System.Int32 a = 10; 是完全相同的



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
## 变量
```c#
// 所有引用类型的默认值为 null
// 数值类型默认值为 0
// 布尔类型默认值为 false
// 字符串默认值为 ""（空字符串）

// 局部变量必须显式初始化


// 明确指定类型
int i = 10;
// 自动推断类型
var i = 10;
```
## 常量
```c#
const int i = 10;
```
## using指令
```c#
using System;


// 别名
using MyAlias = Some.Very.Long.Namespace.MyClass;
using IO = System.IO;


// 使用 using static 可以引入一个类的静态成员，从而直接使用静态方法或属性，无需类名限定
using static System.Math; // 引入Math类的静态成员
using static System.Console; // 引入Console类的静态成员
PI * Pow(radius, 2);


// 从 C# 10.0 开始，使用 global using 指令可以在一个项目中全局引入命名空间或静态成员，这样项目中的所有文件都不需要再单独引入
global using System;
global using static System.Math;
```
## if
```c#
// C# 中 if 语句的条件表达式必须严格评估为 true 或 false。整数、指针或其他非布尔类型不能直接用作条件

int score = 85;
if (score >= 90)
{
    Console.WriteLine("优秀");
}
else if (score >= 60)
{
    Console.WriteLine("及格");
}
else
{
    Console.WriteLine("不及格");
}
```
## switch
```c#
// case 值必须是常量

int day = 3;
switch (day)
{
    case 1:
        Console.WriteLine("星期一");
        break;
	// 多个 case 可共享同一代码块
    case 2:
    case 3:
        Console.WriteLine("星期三");
        break;
    default:
        Console.WriteLine("无效输入");
        break;
}
```
## 条件运算符
```c#
bool result = (20 > 10) ? true : false;
```
## for
```c#
int sum = 0;
for (int i = 1; i <= 100; i++)
{
    sum += i;
	continue;
	break;
}
```
## while
```c#
int count = 5;
while (count > 0)
{
    Console.WriteLine($"倒计时: {count}");
    count--;
}




int count = 5;
do
{
    count--;
} while (count > 0);
```
## foreach
```c#
// 遍历数组
int[] numbers = { 1, 2, 3, 4, 5 };
foreach (int num in numbers)
{
    Console.WriteLine(num);
}



Dictionary<int, string> students = new Dictionary<int, string> { { 1, "Alice" }, { 2, "Bob" } };
foreach (var (id, name) in students)
{
    Console.WriteLine($"学号: {id}, 姓名: {name}");
}
```
## 数组
```c#
int[] numbers = new int[5];
int[] numbers = {1, 2, 3, 4, 5};

int[] numbers = new int[] {1, 2, 3, 4, 5}; // 编译器推断大小为5
int[] numbers = new int[5] {1, 2, 3, 4, 5}; // 显式指定大小为5，元素个数必须匹配

int firstNumber = numbers[0]; // 读取第一个元素，值为10
numbers[1] = 25; // 修改第二个元素的值为25

foreach (int number in numbers)
{
    Console.WriteLine(number);
}




// 声明并初始化一个2行3列的二维数组
int[,] matrix = new int[2, 3] { {1, 2, 3}, {4, 5, 6} };
// 访问第二行第三列的元素
int element = matrix[1, 2]; // 值为6



// 声明并初始化一个交错数组
int[][] jaggedArray = new int[3][];
jaggedArray[0] = new int[] {1, 2, 3};
jaggedArray[1] = new int[] {4, 5};
jaggedArray[2] = new int[] {6, 7, 8, 9};
// 访问第一个子数组的第二个元素
int element = jaggedArray[0][1]; // 值为2
```
## 枚举
```c#
public enum Weekday
{
    Monday,    // 0
    Tuesday,   // 1
    Wednesday // 2
}


// 可以显式指定值或基础类型（如 byte、long）
public enum ErrorCode : ushort
{
    Success = 200,
    NotFound = 404,
    ServerError = 500
}


public enum LoopType
{
    None,   // 0
    Daily,  // 1
    Weekly = 7,
    Monthly // 8
}
```
## 类型转换
```c#
// 隐式转换由编译器自动完成，它发生在转换安全、不会导致数据丢失的情况下
int numberInt = 100;
long numberLong = numberInt; // 编译器自动完成隐式转换


// 显式类型转换
int i = 10;
byte b = (byte)i;


// 基类引用转换为派生类引用（向下转型）
Animal someAnimal = new Dog();
Dog myDogAgain = (Dog)someAnimal; // 向下转型，需要显式转换


// 引用类型转换：as 和 is 运算符
// ​​as 运算符​​：它尝试将对象转换为指定类型，如果转换失败（类型不兼容或对象为 null），则返回 null，而不会抛出异常
// as运算符只能用于引用类型或可空值类型
object obj = "这是一个字符串";
string str = obj as string; // 转换成功，str 不为 null
object notAString = 123;
string failedConversion = notAString as string; // 转换失败，failedConversion 为 null


// ​​is 运算符
if (someObject is string specificString)
{
    // 检查通过后，可以直接使用 specificString
    Console.WriteLine(specificString.Length);
}



// System.Convert类提供了一系列静态方法（如 ToInt32, ToString）
string numberString = "123";
int convertedNumber = Convert.ToInt32(numberString); // 字符串转整数


// Parse 与 TryParse 方法​​：专门用于将字符串转换为特定的值类型（如整数、浮点数、日期等）
string input = "true";
bool result = bool.Parse(input); // 使用 Parse 转换
```
## 自定义类型
```c#
// 可以使用 struct、class、interface、enum 和 record 创建自定义类型
// 使用 struct 关键字定义的类型是值类型;所有内置数值类型都是 structs。另一类值类型是 enum
// 定义为class、record、delegate、数组或interface的类型是reference type。
```
## 格式化输出
```c#
int i = 4, j = 8;
Console.WriteLine("i={0}, j={1}", i, j);
```
## 定义对象
```c#
class Person
{
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
## 原始字符串
```c#
// 单行原始字符串
string singleLine = """Friends say "hello" as they pass by.""";


// 多行原始字符串
// 起始和结束引号需独占一行
// 编译器会自动删除结束引号左侧的所有空格
string multiLine = """
    This line is indented in the code. // 输出时左侧无缩进空格
    This line too. // 输出时左侧无缩进空格
    """;
```
## 控制流
## if
```c#
if (score >= 90)
{
    Console.WriteLine("优秀");
}
else if (score >= 60)
{
    Console.WriteLine("及格");
}
else
{
    Console.WriteLine("不及格");
}
```
## switch
```c#
int dayOfWeek = 3;
switch (dayOfWeek)
{
    case 1:
    case 2:
    case 3:
        Console.WriteLine("工作日");
        break;
    default:
        Console.WriteLine("无效的日期");
        break;
}


// 可以匹配字符串
string command = "start";
switch (command)
{
    case "start":
        Console.WriteLine("程序启动...");
        break;
    default:
        Console.WriteLine("未知命令");
        break;
}


// 匹配类型
switch (obj)
{
	case int i:
		return $"这是一个整数: {i}";
	case string s:
		return $"这是一个字符串: {s}";
	case double d:
		return $"这是一个双精度浮点数: {d}";
	default:
		return "未知类型";
}


// when 子句
int score = 85;
switch (score)
{
    case int n when n >= 90:
        Console.WriteLine("优秀");
        break;
    default:
        Console.WriteLine("不及格");
        break;
}


// 简洁的 switch 表达式语法
int number = 2;
string result = number switch
{
    1 => "数字是1",
    2 => "数字是2", // result将被赋值为"数字是2"
    3 => "数字是3",
    _ => "未知数字" // _ 代表默认情况
};
```
## for
```c#
int sum = 0;
for (int i = 0; i < 5; i++)
{
    Console.WriteLine($"{i}");
	break;
}

for (;;)
{
	Console.WriteLine("hi");
}
```
## while
```c#
int num = 5;
while (num > 0)
{
    Console.WriteLine($"{num}");
	num--;
}




int num = 5;
do
{
	Console.WriteLine($"{num}");
	num--;
} while (num > 0);
```
## foreach
```c#
int[] numbers = { 1, 2, 3, 4, 5 };
foreach (var num in numbers)
{
    Console.WriteLine(num * 2); // 输出：2, 4, 6, 8, 10
}



// 遍历字典
Dictionary<string, string> capitals = new Dictionary<string, string>
{
    { "France", "Paris" }, 
    { "Japan", "Tokyo" }
};
foreach (KeyValuePair<string, string> pair in capitals)
{
    Console.WriteLine($"国家：{pair.Key}，首都：{pair.Value}");
}
```
## 构造函数
```c#

```