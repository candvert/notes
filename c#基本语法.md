# c#基本语法

c#中所有都是类，不能像c++一样在类外定义函数和变量

## main函数

```c#
using System;

namespace HelloWorld
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("Hello World");
        }
    }
}
```

## 基本数据类型

```c#
byte, short, int, long, float, double, decimal, char, bool
// 对应的.NET类型
Byte, Int16, Int32, Int64, Single, Double, Decimal, Char, Boolean
```

## 注释

```c#
// this is comment

/* this is comment */
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

