- [示例](#示例)
- [基础](#基础)
	- [基本语句](#基本语句)
	- [变量](#变量)
	- [指定c++标准](#指定c++标准)
	- [指定头文件目录](#指定头文件目录)
	- [生成一个静态库并添加到项目](#生成一个静态库并添加到项目)
	- [生成动态库](#生成动态库)
	- [安装库或文件](#安装库或文件)
	- [链接第三方库](#链接第三方库)
- [常用指令](#常用指令)
	- [find_package](#find_package)
	- [add_library](#add_library)
	- [target_link_libraries](#target_link_libraries)
	- [include](#include)
	- [set_target_properties](#set_target_properties)

配置文件为 `CMakeLists.txt`
```sh
mkdir build
cd build
cmake ..
cmake --build .
```
如果是在 windows 上并使用mingw，则：
```sh
mkdir build
cd build
cmake -G "MinGW Makefiles" ..
cmake --build .
```
## 使用shell脚本简化命令
在项目根目录创建 run.sh 文件：
```sh
#!/bin/bash

# -S 指定源目录
# -B 指定构建目录
cmake -S . -B build

cmake --build build

./build/helloworld
```
然后运行该脚本：
```sh
./run.sh
```
## 示例
目录结构：
```go
content/
├── CMakeLists.txt
├── main.cpp
├── mainwindow.h
├── mainwindow.cpp
└── redblockwidget/
        ├── redblockwidget.h
		└── redblockwidget.cpp
```
CMakeLists.txt文件：
```cmake
cmake_minimum_required(VERSION 3.16)

project(helloworld VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 REQUIRED COMPONENTS Widgets Network)

qt_standard_project_setup()

qt_add_executable(helloworld
	mainwindow.cpp
	main.cpp
	# 使用相对路径包含 redblockwidget.cpp
	redblockwidget/redblockwidget.cpp
)
# redblockwidget 的头文件目录
target_include_directories(helloworld PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/redblockwidget
)

target_link_libraries(helloworld PRIVATE Qt6::Widgets Qt6::Network)

set_target_properties(helloworld PROPERTIES
	# 阻止在 Windows 上创建控制台窗口
	WIN32_EXECUTABLE ON
	# 在 macOS 上创建应用程序包
	MACOSX_BUNDLE ON
)
```
## 基础
## 基本语句
```cmake
cmake_minimum_required(VERSION 3.10)
project(ScreenShot)
add_executable(ScreenShot main.cpp)
```
## 项目
```cmake
project(ScreenShot VERSION 0.1 LANGUAGES CXX)

# 形式：
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [DESCRIPTION <project-description-string>]
        [HOMEPAGE_URL <url-string>]
        [LANGUAGES <language-name>...])
```
## 变量
```cmake
# 格式为set(变量名 变量值)
set(cxx 11)
set(PROJECT_SOURCES main.cpp a.h a.cpp)
# 使用变量的语法为${variable}
set(CMAKE_CXX_STANDARD ${cxx})
add_executable(myproject ${PROJECT_SOURCES})
```
## 指定c++标准
```cmake
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```
## 指定头文件目录
```cmake
# 下面这句需要放在add_executable()之后
target_include_directories(ScreenShot PUBLIC "${PROJECT_BINARY_DIR})")
```
## 生成一个静态库并添加到项目
```cmake
# 比如生成静态库的文件在与CMakeLists.txt同级的MathFunctions目录中
# 那么先在MathFunctions中添加一个CMakeLists.txt文件，然后写入下面内容
add_library(MathFunctions mysqrt.cpp)
target_include_directories(MathFunctions INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}")	
# 接着在顶层CMakeLists.txt添加下面内容
add_subdirectory(MathFunctions)
target_link_libraries(ScreenShot PUBLIC MathFunctions)
```
## 生成动态库
```cmake
# 比如生成动态库的文件在与CMakeLists.txt同级的MathFunctions目录中
# 那么先在MathFunctions中添加一个CMakeLists.txt文件，然后写入下面内容
add_library(MathFunctions SHARED mysqrt.cpp)
target_include_directories(MathFunctions INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}")
# 接着在顶层CMakeLists.txt添加下面内容
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARAY_DIR}") # .a .lib
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARAY_DIR}") # .dll .exe
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PROJECT_BINARAY_DIR}") # .so
```
## 安装库或文件
```cmake
# 格式为install(TARGETS 库或生成的项目文件 DESTINATION 目录)
# 如果是文件，则install(FILES MathFunctions.h DESTINATION 目录)
# 下面都是安装到了系统目录下的lib目录和include目录
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)


# Qt中的使用
install(TARGETS mytarget
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
```
## 链接第三方库
```cmake
# 以opencv库为例
find_package(OpenCV REQUIRED)
target_link_libraries(ScreenShot ${OpenCV_LIBS})
```
# 常用指令
## find_package
find_package 命令会搜索 `<PackageName>Config.cmake` 或 `<lowercasePackageName>-config.cmake` 配置文件，这些文件被称为包配置文件，通常随包一起安装
```cmake
# 当缺乏该包项目不能配置成功时应该使用REQUIRED
# 有REQUIRED时COMPONENTS关键字可以直接省略
find_package(<PackageName> [<version>] [REQUIRED] [COMPONENTS <components>...])


# 比如Qt中的使用
find_package(Qt6 REQUIRED COMPONENTS Widgets Network)
target_link_libraries(mytarget PRIVATE Qt6::Widgets Qt6::Network)
```
## add_library
```cmake
add_library(<name> [<type>] [EXCLUDE_FROM_ALL] <sources>...)

# 使用
add_library(MathFunctions SHARED mysqrt.cpp)
```
## target_link_libraries
```cmake
target_link_libraries(<target>
                      <PRIVATE|PUBLIC|INTERFACE> <item>...
                     [<PRIVATE|PUBLIC|INTERFACE> <item>...]...)

# 使用
target_link_libraries(mytarget PRIVATE Qt6::Widgets Qt6::Network)
```
## include
```cmake
# 从给定文件加载并运行cmake代码。如果存在OPTIONAL，则文件不存在时不会引发错误。如果给出了RESULT_VARIABLE，则变量<var>将设置为已包含的完整文件名，如果失败则设置为NOTFOUND
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>]
                      [NO_POLICY_SCOPE])
                      
```
## set_target_properties
```cmake
set_target_properties(<targets> ...
                      PROPERTIES <prop1> <value1>
                      [<prop2> <value2>] ...)

# 使用
set_target_properties(mytarget PROPERTIES
    OUTPUT_NAME "CustomApp"
    WIN32_EXECUTABLE TRUE
)
```
