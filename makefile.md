- [makefile简介](#makefile简介)
	- [基本语法](#基本语法)
	- [执行逻辑和两个阶段](#执行逻辑和两个阶段)
	- [默认目标](#默认目标)
	- [伪目标](#伪目标)
	- [变量与赋值](#变量与赋值)
	- [自动变量](#自动变量)
	- [预定义变量](#预定义变量)
	- [include指令和嵌套make](#include指令和嵌套make)
	- [if语句](#if语句)
	- [函数](#函数)
	- [其他](#其他)



# makefile简介

## 基本语法

最简单的makefile由像如下形式的规则组成

```makefile
target ... : prerequisites ...
	recipe
	...
	...
```

target为生成的程序（即目标），prerequisites为生成程序所需的文件（即依赖），recipe为生成程序所需执行的命令，基本为shell语句

比如由hello.cpp生成hello程序的makefile如下所示

```makefile
hello : hello.cpp
	g++ -o hello hello.cpp
```

需要注意的是命令前面是一个tab，而不能是四个空格

## 执行逻辑和两个阶段

1. make的执行逻辑

make可以缩短编译时间，因为make只会更新需要更新的文件。

如果你执行上述makefile生成了hello程序，那么接着第二次执行make便会提示”hello已是最新“。make是如何知道hello不需要重新生成的呢，其实是通过比较hello和hello.cpp两个文件的修改时间来实现的。如果hello的修改时间晚于hello.cpp的，那么就说明hello.cpp在上一次make执行后未做修改，因此也就不需要重新生成hello。

2. make执行的两个阶段

在实践上，上述的makefile会写成如下形式

```makefile
hello : hello.o
	g++ -o hello hello.o
	
hello.o : hello.cpp
	g++ -c hello.cpp
```

make执行时，在第一个阶段，make会读取makefile的所有内容，生成目标和依赖之间的关系图，比如上述的关系图为hello.cpp->hello.o->hello，在第二个阶段make会决定那些目标需要更新并执行相应的命令。

## 默认目标

默认make会执行第一个目标，如果将makefile写成如下形式，那么只会生成hello.o而不会生成hello

```makefile
hello.o : hello.cpp
	g++ -c hello.cpp

hello : hello.o
	g++ -o hello hello.o
```

当然也可以指定目标来执行，使用make hello便可以生成hello程序

## 伪目标

实践上，经常需要删除中间文件和程序使目录保持”干净“，则可以添加一个目标clean。

```makefile
hello : hello.o
	g++ -o hello hello.o
	
hello.o : hello.cpp
	g++ -c hello.cpp
	
clean :
	rm hello.o hello
```

但如果当前目录有一个为clean的文件，则执行make clean时由于目标clean没有依赖，所以会始终判断clean是最新的，导致不会执行相应的命令。

此时，便可以使用伪目标.PHONY

```makefile
hello : hello.o
	g++ -o hello hello.o
	
hello.o : hello.cpp
	g++ -c hello.cpp
	
.PHONY: clean
clean :
	rm hello.o hello
# .PHONY: clean 也可写在此处，由于make解析分为两个阶段所以顺序没有影响
```

.PHONY告诉make不管有没有clean文件都执行相关的命令

## 变量与赋值

定义变量的语句如下

```makefile
obj = hello
```

使用变量

```makefile
$(obj) : hello.cpp
	g++ -o $(obj) hello.cpp
```

变量有多种赋值方式，主要使用的为=，:=，+=，区别为=为延迟赋值，:=为立即赋值，+=将值添加到变量上

```makefile
obj = hello
all = $(obj)
obj = world
clean:
	echo $(all)
# 使用=赋值echo打印出的值为world

obj = hello
all := $(obj)
obj = world
clean:
	echo $(all)
# 使用:=赋值echo打印出的值为hello

obj = hello
obj += world
clean:
	echo $(obj)
# 使用+=赋值echo打印出的值为hello world
```

## 自动变量

自动变量可以简化规则的书写，常用的有$@，$<，$^，$@的值为目标，$<的值为第一个依赖，$^的值为所有依赖。

以下几个表达的是同一个意思

```makefile
# example
hello: hello.cpp world.cpp
	g++ -o hello hello.cpp world.cpp
	
hello: hello.cpp world.cpp
	g++ -o $@ hello.cpp world.cpp
	
hello: hello.cpp world.cpp
	g++ -o hello $< world.cpp

hello: hello.cpp world.cpp
	g++ -o hello $^
	
hello: hello.cpp world.cpp
	g++ -o $@ $^
```

## 预定义变量

常用的预定义变量有如下这些。预定义变量赋值时应使用+=。

```makefile
CC          # 相当于gcc
CPP         # 相当于gcc -E
CXX         # 相当于g++
CFLAGS      # 给gcc指定的选项
CPPFLAGS    # 给gcc预处理时指定的选项
CXXFLAGS    # 给g++指定的选项
LDFLAGS     # 给编译器链接时指定的选项
LDLIBS      # 给编译器链接时指定的选项
```

通过下述makefile演示一下几个预定义变量，以下几个表达的是同一个意思。

```makefile
# example 假设使用了opencv库
hello: hello.cpp
	g++ -o hello hello.cpp -I/usr/include/opencv4 -L/usr/lib -lopencv_core
	
# CXX
CXX += g++
hello: hello.cpp
	$(CXX) -o hello hello.cpp -I/usr/include/opencv4 -L/usr/lib -lopencv_core

# CXXFLAGS
CXXFLAGS += -I/usr/include/opencv4
hello: hello.cpp
	g++ -o hello hello.cpp $(CXXFLAGS) -L/usr/lib -lopencv_core
	
# LDFLAGS
LDFLAGS += -L/usr/lib
hello: hello.cpp
	g++ -o hello hello.cpp -I/usr/include/opencv4 $(LDFLAGS) -lopencv_core
	
# LDLIBS
LDLIBS += -lopencv_core
hello: hello.cpp
	g++ -o hello hello.cpp -I/usr/include/opencv4 -L/usr/lib $(LDLIBS)
```

## include指令和嵌套make

1. include指令

include指令和c/c++包含头文件的指令类似，会将include的文件展开。

```makefile
# 比如在当前makefile目录下有一个子目录为subdir，其中有一个makefile，则可以通过include指令将其内容添加进来

include subdir/makefile

hello : hello.o
	g++ -o hello hello.o
```

2. 嵌套make

大型项目目录结构比较复杂，一般每一层都会有一个makefile，这种情况下就需要嵌套make来执行子目录下的makefile。

```makefile
hello : hello.o
	$(MAKE) -C subdir
	g++ -o hello hello.o
```

上述的`$(MAKE) -C subdir`便相当于`make -C subdir`的shell命令，其中-C选项便是指定make执行的目录

## if语句

```makefile
# ifeq判断两个字符串是否相等
hello : hello.o
ifeq ($(CC),gcc)
	g++ -o hello hello.o
else ifeq ($(CC),gcc)
	g++ -o hello hello.o
else
	g++ -o hello hello.o
endif

# ifneq判断两个字符串是否不等
hello : hello.o
ifneq ($(CC),gcc)
	g++ -o hello hello.o
else ifneq ($(CC),gcc)
	g++ -o hello hello.o
else
	g++ -o hello hello.o
endif
```

```makefile
# ifdef判断变量是否已定义
hello : hello.o
ifdef CC
	g++ -o hello hello.o
else ifdef CC
	g++ -o hello hello.o
else
	g++ -o hello hello.o
endif

# ifndef判断变量是否未定义
hello : hello.o
ifndef CC
	g++ -o hello hello.o
else ifndef CC
	g++ -o hello hello.o
else
	g++ -o hello hello.o
endif
```

## 函数

函数的用法如下

```makefile
$(function arguments)
# function为函数名，arguments为参数，可以有多个，以,隔开
```

比如使用info函数

```makefile
$(info hello)

hello : hello.o
	g++ -o hello hello.o
```

上述makefile在运行make时会在命令行打印hello

其他函数有`let，foreach，call`等，本文不再赘述。

## 其他

1. 命令前加@或-

```makefile
# 可以在命令前加@，则运行make时不再打印相应命令
clean:
	@echo hello

# 可以在命令前加-，则运行命令时出错也不会中断，而是继续运行
clean:
	-rm hello.x
```

2. 多行变量

当想变量的值书写成多行时，可以使用\

```makefile
# 和obj = hello world相等
obj = hello \
	  world
```

当使用\书写多行变量时，\符号前的空格和其后面的换行符及空格都会转变为一个空格。

```makefile
# 和obj = hello world相等
obj = hello    	\
	      world
```

3. 每条命令都是在单独的终端中运行的

```makefile
# echo语句打印的结果为a=
hello : hello.o
	@a = 2
	@echo a=$$a
	@g++ -o hello hello.o
```

如果要使命令在同一个终端运行，则需要加上.ONESHELL

```makefile
# echo语句打印的结果为a=2
.ONESHELL:
hello : hello.o
	@a = 2
	@echo $$(a)
	@g++ -o hello hello.o
```