- [Java基本语法](#Java基本语法)
	- [main函数](#main函数)
	- [基本数据类型](#基本数据类型)
	- [注释](#注释)
	- [数组](#数组)
	- [try catch和throws](#try catch和throws)
	- [final](#final)
	- [继承](#继承)
	- [重写Override](#重写Override)
	- [包和导入](#包和导入)
	- [构造代码块](#构造代码块)
	- [静态代码块](#静态代码块)
	- [抽象类](#抽象类)
	- [接口](#接口)
	- [实现接口](#实现接口)
	- [匿名类](#匿名类)
	- [包装类](#包装类)
	- [lambda表达式](#lambda表达式)
	- [泛型](#泛型)
	- [泛型通配符](#泛型通配符)
	- [反射](#反射)
	- [注解](#注解)
	- [JUnit测试框架](#JUnit测试框架)



# Java基本语法

java中数组和new出来的对象都是指针

java中有构造函数，无析构函数

java中所有都是类，不能像c++一样在类外定义函数和变量

java中只能单继承，但可以实现多个接口，也可以单继承的同时实现多个接口

## main函数

```java
import java.lang.System;	// 默认导入

public class Ok {
    public static void main(String[] args) {
    	  System.out.println("hello");
    }
}
```

## 基本数据类型

```java
char, byte, short, int, long, boolean, float, double
```

## 注释

```java
// this is comment

/* this is comment */
```

## 数组

```java
// 第一种定义方式
int[] arr = new int[10];

// 第二种定义方式
int[] arr = new int[]{3, 4};

// 第三种定义方式
int[] arr = {3, 4};
```

## try catch和throws

```java
// try catch语法
try {
    
} catch (Exception e) {
    
}

// 方法可能抛出异常使用关键字throws
public class A {
    public void method() throws Exception, RuntimeException {}
}

// finally关键字表示无论抛不抛出异常都会执行
try {
    
} catch (Exception e) {
    
} finally {
    System.out.println("hello");  //此句无论抛不抛出异常都会执行
}
```

## final

```java
// 修饰类表示类不能被继承
public final class Ok {}

// 修饰成员方法表示不能被重写
public class Ok {
    public final void method() {}
}

// 修饰成员变量表示该变量为常量（相当于c++中的const），必须初始化
public class Ok {
    public final int name = 89;
}
```

## 继承

```java
public class A {}

// 继承使用extends关键字
public class B extends A {}
```

## 重写Override

```java
public class A {
    public void method() {}
}

// 重写方法需要加上@Override注解
public class B extends A {
    @Override
    public void method() {}
}
```

## 包和导入

```java
// 包就是文件夹
package com.example;

import com.example.OK;			// 导入一个类
import static java.lang.Math.*; // 导入Math类的所有静态成员
```

## 构造代码块

```java
public class A {
    // 构造代码块每次创建对象都会执行，先于构造函数执行
    {
        System.out.println("hello");
    }
}
```

## 静态代码块

```java
public class A {
    // 无论创建多少对象，只会执行一次，先于构造函数执行
    static {
        System.out.println("hello");
    }
}
```

## 抽象类

```java
// 抽象类不能实例化
// 抽象类中不一定有抽象方法，有抽象方法的类一定是抽象类
// 可以有构造方法
public abstract class A {
    String name;
    public A() {
        name = "hello";
    }
    
    public abstract void method();
}
```

## 接口

```java
interface Eat {
    int a = 8;
    public void method();
}
```

## 实现接口

```java
interface Eat {}
public class A implements Eat {}
```

## 匿名类

```java
interface Eat {
    public void method();
}

public class A {
    // 匿名类格式new 类名或接口名() {}
    // 类名代表匿名类继承了他，接口名代表匿名类实现了他
    Eat e = new Eat() {
        @Override
        public void method() {}
    };
}
```

## 包装类

```java
char		Character
byte		Byte
short		Short
int			Integer
long		Long
boolean		Boolean
float		Float
double		Double
```

## lambda表达式

```java
// 格式
(int a, int b) -> { return a + b; }
// 当只有一个参数并且语句只有一条时，可以简化成下述格式
a -> return a
```

## 泛型

```java
// 泛型类
public class A<E, T> {}

// 泛型方法
public class A {
    public <E> void method(E e) {}
}

// 泛型类
public class A<E> {
    public A<E> method(E e) {
        return new A<E>();
    }
}

// 泛型接口
interface Eat<E> {}

// 类实现泛型接口
public class A<E> implements Eat<E> {}
```

## 泛型通配符

```java
public class A {
	// ArrayList<? extends B> arr表示arr中的元素都是B的子类，一般在List、ArrayList等容器类中使用
    public void method(ArrayList<? extends B> arr) {}
}
```

## 反射

```java
public class A {}

// 获取Class对象（Class对象具有获取到的类的所有信息，比如类名等）
// 实际反射是将类的信息加载到内存中
// 第一种方法
Class c = A.class;

// 第二种方法
Class c = Class.forName("A");

// 第三种方法
A a = new A();
Class c = a.getClass();


// 实际使用
Class c = A.class;
Constructor[] constructors = c.getDeclaredConstructors();  // 得到类中所有的构造函数
constructors[0].newInstance;  // 通过类的构造函数实例化一个对象

Field[] fields = c.getField();  //得到类中所有的成员变量

Method[] methods = c.getMethods();  //得到类中所有的成员方法
```

## 注解

```java
// 定义注解
@Target(ElementType.Type)  // ElementType.Type表示该注解只能使用在类上
@Rentention(RetentionPolicy.RUNTIME)  // RetentionPolicy.RUNTIME表示该注解保留到运行时
@interface Eat {
    String name() default "hello";  // 实际编译成字节码是abstract String name() {};
}
```

## JUnit测试框架

```java
public class TestClass {
    // 测试方法上要加上@Test注解，测试方法必须是公共的，无参，无返回值
    @Test
    public void TestMethod() {}
}
```

