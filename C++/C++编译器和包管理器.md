- [gcc](#gcc)
	- [静态和动态链接库](#静态和动态链接库)
- [gdb](#gdb)
- [vcpkg](#vcpkg)

# gcc
```
gcc -o <filename> 指定生成的文件名
gcc -E 生成预处理后文件
gcc -S 生成编译后的文件
gcc -c 生成二进制文件
gcc -I 指定头文件搜索路径
gcc -L 指定库搜索路径
gcc -l 指定库的名称
gcc -g 生成调试信息，以便gdb调试
gcc -static 静态链接

gcc -O3 最高级优化，-O0代表不优化
gcc -w 不输出任何警告
gcc -W 输出警告信息
gcc -Wall 输出所有警告
gcc -Wunused-variable 输出unused-variable类型的警告
gcc -Werror 将警告当作错误
gcc -finput-charset=UTF-8 指定源文件编码
gcc -fexec-charset=GBK 指定运行时编码
```
## 静态和动态链接库
```
静态链接库
	命名 lib<name>.a
	生成 ar rcs lib<name>.a file1.o file2.o
	使用 gcc main.c -L<dir> -l<name>
		 或gcc main.c <dir>/lib<name>.a

动态链接库
	命名 lib<name>.so
	生成 gcc -fPIC -shared -o lib<name>.so file.c
	
		 或gcc -c -fPIC file.c
		 gcc -o lib<name>.so -shared file.o
	使用 gcc main.c -L<dir> -l<name>
		 或gcc main.c <dir>/lib<name>.a
```
# gdb
```
首先要通过gcc或g++的-g选项生成可调试的程序
b 打断点，可以b 函数名，b 行号
r 运行
n 单步执行，即next
回车 执行上一个命令
s 进入函数，即step
c 继续运行，即continue
p 查看变量的值，即print
k 结束调试
info b 查看断点信息
d 删除断点，后面加上info b看到的断点编号
bt 函数调用栈，即backtrace
watch 跟踪变量，watch 变量
info r查看寄存器的值
```
# vcpkg
```
Windows上安装：
git clone git@github.com:microsoft/vcpkg.git
执行克隆的仓库里的bootstrap-vcpkg.bat，然后目录下会出现vcpkg.exe
接着在命令行输入./vcpkg integrate install
此时在Visual Studio的属性中便可看到vcpkg
需要安装库在命令行中使用vcpkg命令即可

使用vcpkg安装库后，visual studio会自动链接动态/静态库，但是使用opencv库时头文件包含目录仍需手动在属性中设置（可能是bug）
若vcpkg所在目录为D:/Apps/vcpkg/vcpkg.exe，则安装的库（包括opencv）的头文件在D:\Apps\vcpkg\installed\x64-windows\include\opencv4
```
![](images/cpp_compiler_1.png)
```
比如vcpkg所在目录为D:/Apps/vcpkg/vcpkg.exe，则
cmake的项目要有-DCMAKE_TOOLCHAIN_FILE=D:/Apps/vcpkg/scripts/buildsystems/vcpkg.cmake

vcpkg integrate remove    移除集成
vcpkg install <package-name>:x64-windows    安装库
vcpkg install <package-name>:x86-windows    安装库
vcpkg install <package-name>:x64-windows-static    安装库
vcpkg remove <package-name>    卸载库
vcpkg list    显示所有已安装的库
vcpkg search <package-name>    搜索库
vcpkg update <package-name>    更新库
```