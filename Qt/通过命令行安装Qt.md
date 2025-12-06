官方教程：[https://doc.qt.io/qt-6/get-and-install-qt-cli.html](https://doc.qt.io/qt-6/get-and-install-qt-cli.html)
## Linux
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
## 测试项目
CMakeLists.txt：
```cpp
cmake_minimum_required(VERSION 3.16)

project(MyQtApp VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 COMPONENTS Widgets REQUIRED)

add_executable(my_app
    main.cpp
    mainwindow.cpp
    mainwindow.h
)

target_link_libraries(my_app PRIVATE Qt6::Widgets)
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