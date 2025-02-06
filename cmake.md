- [cmake](#cmake)
	- [基本语句](#基本语句)
	- [命令行](#命令行)
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

# cmake

## 基本语句

```cmake
cmake_minimum_required(VERSION 3.10)
project(ScreenShot)
add_executable(ScreenShot main.cpp)
```
## 项目
```cmake
project(ScreenShot VERSION 0.1 LANGUAGES CXX)

形式：
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [DESCRIPTION <project-description-string>]
        [HOMEPAGE_URL <url-string>]
        [LANGUAGES <language-name>...])
```
## 命令行

```shell
# 使用cmake编译应先在CMakeLists.txt的同级目录创建一个build文件夹，再cd build进入文件夹
cmake ..
cmake --build .

# 指定生成Makefile
cmake -G "MinGW Makefiles" ..
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
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```
## 指定头文件目录

```cmake
# include_directories可以使用相对路径
include_directories(ok)

# 对于单个目标使用target_include_directories()
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
```cmake
形式：
find_package(<PackageName> [<version>] [REQUIRED] [COMPONENTS <components>...])
当缺乏该包项目不能配置成功时应该使用REQUIRED
有REQUIRED时COMPONENTS关键字可以直接省略

Qt中的使用
find_package(Qt6 REQUIRED COMPONENTS Widgets Network)
target_link_libraries(mytarget PRIVATE Qt6::Widgets Qt6::Network)

opencv包的cmake配置文件有：
OpenCVConfig.cmake
OpenCVConfig-version.cmake
OpenCVModules.cmake
OpenCVModules-debug.cmake
OpenCVModules-release.cmake

使用cmake的包支持<PackageName>Config.cmake和<LowercasePackageName>-config.cmake两种配置文件名，只有该文件是必须的以使用find_package()
可能还含有<PackageName>ConfigVersion.cmake文件，其被用来检查包的版本是否满足find_package()的版本参数限制，但find_package()指定版本是可选的

CMAKE_PREFIX_PATH变量指定搜索的cmake配置文件目录
```
## add_library
```cmake
形式：
add_library(<name> [<type>] [EXCLUDE_FROM_ALL] <sources>...)

使用：
add_library(MathFunctions SHARED mysqrt.cpp)
```
## target_link_libraries
```cmake
常用形式：
target_link_libraries(<target>
                      <PRIVATE|PUBLIC|INTERFACE> <item>...
                     [<PRIVATE|PUBLIC|INTERFACE> <item>...]...)

使用：
target_link_libraries(mytarget PRIVATE Qt6::Widgets Qt6::Network)
```
## include
```cmake
形式：
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>]
                      [NO_POLICY_SCOPE])
                      
从给定文件加载并运行cmake代码。如果存在OPTIONAL，则文件不存在时不会引发错误。如果给出了RESULT_VARIABLE，则变量<var>将设置为已包含的完整文件名，如果失败则设置为NOTFOUND。
```
## set_target_properties
```cmake
形式：
set_target_properties(<targets> ...
                      PROPERTIES <prop1> <value1>
                      [<prop2> <value2>] ...)

使用：
set_target_properties(mytarget PROPERTIES
    OUTPUT_NAME "CustomApp"
    WIN32_EXECUTABLE TRUE
)
```
