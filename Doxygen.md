- [示例](#示例)
- [创建配置文件](#创建配置文件)
- [配置文件中的标签](#配置文件中的标签)
- [生成文档](#生成文档)
- [注释块](#注释块)
- [结构命令](#结构命令)

## 示例
```cpp
/** 
 * @file		main.cpp
 * @brief		The main.cpp file
 * @details		This file contains serveral functions and
 * a class.
 * @author		John Doe
 * @version		4.0
 * @date		1990-2025
 * @copyright	GNU Public License.
 */
 
/**
 * @brief	Calculate the sum of
 * two numbers.
 *
 * The detailed description of add function. It starts
 * here.
 *
 * @param	a	first number
 * @param 	b	second number
 * @return	the sum of two numbers
 * @see		sub()
 */
int add(int a, int b) {
	return a + b;
}
 
/// @todo	need to write more
void ok() {}
 
/**
 * @brief	A brief description.
 *
 * This is the detailed description. Start
 * here.
 * @param	a,b		the two number needed
 */
int sub(int a, int b) {
	return a - b;
}

/**
 * @class	ExampleA
 * @brief	A brief description of ExampleA.
 * @details	A detailed description of ExampleA.
 */
class ExampleA {
private:
	/// the first variable of class ExampleA
	int num;
	int num2;	///< the second variable of class ExampleA
public:
	void hello() {}	///< the hello function of class ExampleA
};

int main() {
	int a = 2;
	int b = 10;
	add(a, b);
}
```
## 创建配置文件
```
Doxygen使用配置文件Doxyfile设置所有选项。Doxyfile需位于根目录。

创建一个模板配置文件：
doxygen -g <config-file>
如果省略文件名，一个叫Doxyfile的文件会被创建

要记录全局对象（函数、typedef、枚举、宏等），必须记录定义它们的文件。即必须有@file结构命令在文件中
```
## 配置文件中的标签
```
配置文件中都是形如"EXTRACT_ALL = YES"的格式，其中EXTRACT_ALL被称为标签

如果EXTRACT_ALL设为YES，即使没有注释块也会生成文档，doxygen会认为所有实体（doxygen中的实体即为函数，类等）都有注释

递归解析文件树必须将RECURSIVE标签设为YES。

对于几个文件的小项目，可以将INPUT标签留为空，Doxygen会在当前目录搜索源文件。
对于一个包含文件树的大项目，应该将根目录赋予INPUT标签，并且赋予FILE_PATTERNS标签一个或多个文件模式（例如*.cpp，*.h），只有匹配这些模式的文件会被解析。进一步调整被解析的文件可以使用EXCLUDE和EXCLUDE_PATTERNS标签。
例如忽略文件树中所有test文件目录，可以使用：
EXCLUDE_PATTERNS = */test/*

SOURCE_BROWER为YES，会生成交叉引用
INLINE_SOURCES为YES，会将源文件直接包含进文档（例如方便代码审查）
EXTRACT_PRIVAT为YES，类中所有私有成员会包含进文档
EXTRACT_STATIC为YES，类中所有静态成员会包含进文档
SHOW_USED_FILES为NO，不会在类和结构体文档的底部生成该类从哪些文件生成的信息
DISABLE_INDEX为NO，不会生成文档头部的导航栏
GENERATE_TREEVIEW为NO，不会在文档左侧生成树状索引

应该使用的默认设置：
RECURSIVE = YES
INPUT = D:\kkkk
FILE_PATTERNS = .h .hpp .c .cpp .cxx
EXTRACT_PRIVAT = YES
EXTRACT_STATIC = YES
SHOW_USED_FILES = NO


Doxygen使用文件扩展名决定如何解析一个文件，文件扩展名见下表
```
![](images/doxygen_1.png)
## 生成文档
```
生成文档（默认会生成一个html目录和一个latex目录）：
doxygen <config-file>
如果省略文件名，会查找当前目录下的Doxyfile文件
默认输出目录为Doxyfile所在目录
输出目录可以通过OUTPUT_DIRECTORY标签改变
```
## 注释块
```
对代码中的每个实体来说有两种（某些情况下三种）类型的描述：简短描述和详细描述。
对成员方法和函数来说有第三种描述，叫做in body描述，由成员方法或函数内部的所有注释块组成。

如果描述中的单词和某个实体相同，则会生成链接

详细描述：
第一种：
/**
 * ... text ...
 */
第二种：
/*!
 * ... text ...
 */
中间的*号可以省略
/*!
 ... text ...
*/
第三种（至少两行注释）：
///
/// ... text ...
///
或
//!
//!... text ...
//!
请注意，在这种情况下，空白行会结束文档块。
第四种（需要JAVADOC_BANNER标签设为YES）：
/////////////////////////////////////////////////
/// ... text ...
/////////////////////////////////////////////////
或
/***********************************************
 *  ... text
 ***********************************************/




简短描述：
第一种：
/*! @brief Brief description.
 *         Brief description continued.
 *
 *  Detailed description starts here.
 */
第二种：
/// Brief description.
/** Detailed description. */
或
//! Brief description.

//! Detailed description
//! starts here.



doxygen允许将注释块放在定义之前，以保持头文件紧凑

如果想记录文件、结构、联合、类或枚举的成员，可以将注释块放在它们之后。（这些块只能用于记录成员和参数。它们不能用于记录文件、类、联合、结构、组、命名空间、宏和枚举本身。结构命令（如 \class）不允许出现在这些注释块中。）例如：
int var; //!< Brief description after the member
int var; ///< Brief description after the member
int var; /*!< Detailed description after the member */
int var; /**< Detailed description after the member */
int var; //!< Detailed description after the member
         //!<
int var; ///< Detailed description after the member
         ///<

对于函数，可以使用@param命令来记录参数，然后使用[in]、[out]、[in,out]来记录方向。例如：
void foo(int v /**< [in] docs for input parameter v. */);

要记录C++类的成员，还必须记录该类本身。命名空间也是如此。要记录全局C函数、typedef、枚举或预处理器定义，您必须首先记录包含它的文件（通常这将是一个头文件，因为该文件包含导出到其他源文件的信息）。
```
## 结构命令
```
结构命令以一个\或@开头（\和@都可以使用），后跟命令名和一个或多个参数
@class to ducument a class
@struct to document a C-struct.
@union to document a union.
@enum to document an enumeration type.
@fn to document a function.
@var to document a variable or typedef or enum value.
@def to document a #define.
@typedef to document a type definition.
@file to document a file.
@namespace to document a namespace.
@package to document a Java package.
@interface to document an IDL interface.

@brief 简要说明元素的功能
@details 提供详细说明
@return 描述返回值或具体返回值含义
@throw或@exception 描述函数可能抛出的异常
@author和@date 作者和日期信息
@todo 标记待办事项
@deprecated 标记已过时的元素
@warning 添加警告
@note 添加注释
@see 引用其他相关元素


使用@param描述函数参数：
@param[<dir>] <parameter-name> { parameter description }
例子：
/*!
 * Copies bytes from a source memory area to a destination memory area,
 * where both areas may not overlap.
 * @param[out] dest The memory area to copy to.
 * @param[in]  src  The memory area to copy from.
 * @param[in]  n    The number of bytes to copy
 */
void memcpy(void *dest, const void *src, size_t n);
也可以用逗号分隔参数：
/** Sets the position.
 *  @param x,y,z Coordinates of the position in 3D space.
 */
void setPosition(double x,double y,double z,double t)
{
}
```