- [Linux上安装](#Linux上安装)
- [打开Qt文档和编写.ui文件](#打开Qt文档和编写.ui文件)
- [测试安装成功](#测试安装成功)

官方教程：[https://doc.qt.io/qt-6/get-and-install-qt-cli.html](https://doc.qt.io/qt-6/get-and-install-qt-cli.html)
## Linux上安装
下载安装脚本：
```sh
curl -L -O https://download.qt.io/official_releases/online_installers/qt-online-installer-linux-x64-online.run
```
赋予运行权限：
```sh
chmod +x qt-online-installer-linux-x64-online.run
```
安装 package：
```sh
./qt-online-installer-linux-x64-online.run install qt6.10.1-essentials
```
安装 opengl 库：
```sh
sudo apt install build-essential \
                 libgl1-mesa-dev \
                 libglu1-mesa-dev \
                 mesa-common-dev
```
安装后的 moc，rcc，uic 等位于 ~/Qt/6.10.1/gcc_64/libexec/
## 打开Qt文档和编写.ui文件
将该路径添加到 PATH 中
```sh
export PATH="~/Qt/6.10.1/gcc_64/bin:$PATH"
```
在命令行输入 assistant 便可打开 Qt 文档：
```sh
assistant
```
在命令行输入 designer 便可打开 Qt Designer 来创建并修改 .ui 文件
## 测试安装成功
CMakeLists.txt：
```cmake
cmake_minimum_required(VERSION 3.16)

project(helloworld VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 REQUIRED COMPONENTS Widgets)

qt_standard_project_setup()

qt_add_executable(helloworld
	mainwindow.cpp
	main.cpp
)

target_link_libraries(helloworld PRIVATE Qt6::Widgets)

set_target_properties(helloworld PROPERTIES
	WIN32_EXECUTABLE ON
	MACOSX_BUNDLE ON
)
```
main.cpp：
```cpp
#include <QApplication>
#include "mainwindow.h"

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}
```
mainwindow.h：
```cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>

class MainWindow : public QMainWindow
{
    Q_OBJECT // 宏，必须用于所有定义信号/槽/属性的 Qt 类

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();
};

#endif // MAINWINDOW_H
```
mainwindow.cpp
```cpp
#include "mainwindow.h"
#include <QPushButton>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    setWindowTitle("Hello Qt CMake");
    QPushButton *button = new QPushButton("Click Me", this);
    setCentralWidget(button);
}

MainWindow::~MainWindow()
{
}
```