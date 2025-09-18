- [基本语法](#基本语法)
- [属性书写顺序](#属性书写顺序)
- [基本概念](#基本概念)
	- [信号与槽](#信号与槽)
	- [导入](#导入)
	- [属性类型](#属性类型)
		- [id属性](#id属性)
		- [property属性](#property属性)
		- [signal属性](#signal属性)
		- [method属性](#method属性)
		- [enumeration属性](#enumeration属性)
	- [属性绑定](#属性绑定)
	- [自定义类型](#自定义类型)
	- [动态创建和删除对象](#动态创建和删除对象)
	- [视图](#视图)
	- [javascript资源](#javascript资源)
	- [模块](#模块)
	- [设置全局变量](#设置全局变量)
	- [与c++交互](#与c++交互)
	- [设置数据Settings](#设置数据Settings)
- [注释](#注释)
- [布局](#布局)
	- [行](#行)
	- [网格](#网格)
	- [生成多个相同元素Repeater](#生成多个相同元素Repeater)
	- [StackLayout](#StackLayout)
- [全局对象](#全局对象)
- [视图基础元素Item](#视图基础元素Item)
- [获取鼠标事件MouseArea](#获取鼠标事件MouseArea)
- [图片Image](#图片Image)
- [对话框Dialog](#对话框Dialog)
- [滚筒Tumbler](#滚筒Tumbler)
- [输入一行文字TextInput](#输入一行文字TextInput)
- [输入多行文字TextEdit](#输入多行文字TextEdit)
- [焦点区域FocusScope](#焦点区域FocusScope)
- [动画](#动画)
- [状态](#状态)
- [组件component](#组件component)
- [Loader](#Loader)
- [Button](#Button)
- [CheckBox](#CheckBox)
- [Text](#Text)
- [Timer](#Timer)

## 基本语法
```css
/* 和css不同的是属性值后可以加分号，也可以不加，通常不加分号 */
Window {
	id: root;
    width: 640;
    height: 480;
    visible: true;
	title: qsTr("quickApp");
	
	objectName: "the_window";
	
    x: 50;
    y: 50;
    z: 1;	/* 显示顺序 */
    
    width: 200;
    height: 200;
    minimumWidth: 200;
    minimumHeight: 200;
    maximumWidth: 600;
    maximunHeight: 600;
    
    border.color: "black";
    border.width: 5;
    
    font.family: "Ubuntu";
    font.bold: true;
    font.pointSize: 20;
    font.pixelSize: 20;
    font { pointSize: 20; bold: true }
    
    horizontalAlignment: Text.AlignHCenter;
    verticalAlignment: Text.AlignVCenter;
    
    KeyNavigation.tab: first;    /* first是一个id */
    
    Keys.onSpacePressed: Qt.quit();
    Keys.onPressed: function (event) { 
	    switch(event.key) { case Qt.Key_Plus: scale += 0.2 }
    }
    
    gradient: Gradient {
	    GradientStop { position: 0.0; color: "red" }
	    GradientStop { position: 1.0; color: "blue" }
    }
    
    color: "red";
    opacity: 0.5;
    
    transformOrigin: Item.BottomLeft;    /* 设置旋转点，有TopLeft，Top，TopRight
    ，Left，Center，Right，BottomLeft，Bottom，BottomLeft可以设置 */
    rotation: 30;
    scale: 2;
    antialiasing: true;
    
    contentItem: Text { text: "a" }
    
    onWidthChanged: console.log("width:", width);
    onHeightChanged: { console.log("height:", height) }
    
    function eat() {}
    
    
    Rectangle {
	    id: first;
        width: parent.width;	/* 绑定父组件的宽度 */
        height: parent.height;
    	focus: true;
    	radius: 20;
    	
		anchors.centerIn: parent;
    	anchors.fill: parent;
    	
    	anchors.margins: 8;
    	anchors.leftMargins: 8;
    	anchors.rightMargins: 8;
    	anchors.topMargin: 8;
    	anchors.bottomMargin: 8;
    	
    	anchors.left: parent.left;
    	anchor.right: parent.right;
    	anchors.top: parent.top;
    	anchor.bottom: parent.bottom;
    	
    	anchors.horizontalCenter: parent.horizontalCenter;
    	anchor.verticalCenter: parent.verticalCenter;
    	anchors.horizontalCenterOffset: -12;
    	anchors.verticalCenterOffset: -12;
    	
    	MouseArea {
	    	anchors.fill: parent;
	    	onClicked: parent.color = "blue";
    	}
	}
	
	Label {
		text: "ok";
	}
}

ApplicationWindow {
    visible: true;
    width: 800;
    height: 600;
    title: "app";
}
```
## 属性书写顺序
```css
id
x
y
width
height
anchors
```
## 基本概念
## 信号与槽
```css
Button {
	id: button;
	width: 200;
	height: 200;
	
	/* 自定义信号 */
    signal first;
    signal second();		/* 没有参数时()是可选的 */
    signal third(message: string, line: int, column: int);
    
    /* 槽函数 */
    onFirst: console.log("hello");
    /* 参数名不需要和信号中的相同 */
    onThird: (mgs, line, col) => console.log("hello");
    /* 可以省略尾置参数 */
    onThird: message => console.log("hello");
    /* 可以用_代替前面的参数 */
    onThird: (_, _, col) => console.log("hello");
    
    /* 发出信号 */
    onClicked: first();
    
    /* connect函数可以将信号连接到另一个信号或函数 */
    Component.onCompleted: {
        button.first.connect(second);
        button.first.connect(eat);
    }
    function eat() { }
    /* disconnect函数 */
    Component.onDestruction: {
        button.first.disconnect(second);
        button.first.disconnect(eat);  
    }
    
    
    /* 属性改变的信号处理函数，格式为on<属性>Changed */
    /* 一个属性隐式的对应一个信号处理函数 */
    onWidthChanged: console.log("hello");
}

/* 通过Connections收到目标组件的信号 */
/* 下面的代码即Button的clicked信号触发后，由Connections收到并通过onClicked函数处理 */
Window {
    id: root;
    visible: true;
    width: 800;
    height: 600;
    title: "音乐播放器";

    Button {
        id: button;
        anchors.bottom: parent.bottom;
        anchors.horizontalCenter: parent.horizontalCenter;
        text: "Change color!";
    }

    Connections {
        target: button;
        function onClicked() {
            root.color = Qt.rgba(Math.random(), Math.random(), Math.random(), 1);
        }
    }
}
```
## 导入
```css
/* 导入有三种类型，分别为模块、文件夹、javascript文件 */
/* 示例 */
import QtQuick
import QtQuick 2.0
import QtQuick.LocalStorage 2.0 as Database
import "../privateComponents"
import "somefile.js" as Script    /* 导入js文件必须要有关键字as，且名字以大写字母开头 */

import QtQuick			/* Qt Quick模块是QML应用程序的标准库 */
import QtQuick.Controls	/* 控制控件 */
import QtQuick.Layouts    /* 布局 */

/* 在javascript文件中导入另一个javascript文件 */
import * as MathFunctions from "factorial.mjs";
```
## 注释
```sh
和javascript的注释一样，用//或/**/
```
## 属性类型
```css
有下面几种
	id属性
	property属性
	signal属性
	signal handler属性
	method属性
	attached properties and attached signal handler属性
	enumeration属性
```
## id属性
```sh
每个QML对象类型只有一个id属性
id值只能以小写字母或下划线开头，并且只能包含字母，数字，下划线
```
## property属性
```css
/* property的名字只能以小写字母开头，并且只能包含字母，数字，下划线 */
/* 一个自定义的property隐式创建了一个随该property值改变的signal，和一个signal handler叫on<PropertyName>Changed，
<PropertyName>即该property的名字，首字母大写 */
Rectangle {
    property color nextColor
    onNextColorChanged: console.log("The next color will be: " + nextColor.toString())
}
/* 除enumeration type以外的任何QML Value Types类型可以作为自定义property的类型 */
/* 常用内置属性包括string、int、real、double、bool、list、var、date */
property int someNumber: 15
property string someString: "hello"
property url someUrl: "https://baidu.com"
/* var类型是通用类型 */                                                     
property var someNumber: 1.5
property var someString: "abc"
/* 任何QML object type可以作为自定义property的类型 */
property Rectangle someRectangle
property Component myComponent


/* 别名 */
property alias a_color: rectangle.border.color
/* 不能超过三层，下面这种就不行 */
property alias b_color: myItem.myRect.border.color


/* 列表属性 */
Item {
    property list<Rectangle> siblingRects
    
    states: [
        State { name: "loading" },
        State { name: "running" },
        State { name: "stopped" }
    ]
}
/* 如果列表只包含一个，则可以省略[] */
Item {
    states: State { name: "running" }
}


/* 组合属性，比如下面的font属性，font属性有一组子属性，子属性有dot notation和group notation两种赋值方式 */
Text {
    /* dot notation */
    font.pixelSize: 12
    font.b: true
}
Text {
    /* group notation */
    font { pixelSize: 12; b: true }
}


/* 对象定义可以有一个默认属性 */
Text {
    default property var someText
    text: `Hello, ${someText.text}`
}

/* 只读属性 */
readonly property int someNumber: 10

/* 必须赋值的属性 */
required property int someNumber
```
## signal属性
```css
/* 信号处理程序必须在发出信号的对象的定义内声明，并且处理程序应包含要执行的JavaScript代码块 */
Item {
    width: 100; height: 100

    MouseArea {
        anchors.fill: parent
        onClicked: {
            console.log("Click!")
        }
    }
}

/* 信号声明的方式 */
Item {
    signal clicked
    signal hovered()		/* 没有参数时()是可选的 */
    signal actionPerformed(action: string, actionResult: int)
    signal actionCanceled(string action)	/* 不推荐这种，推荐使用上一行的声明 */
}
```
## method属性
```css
import QtQuick
Rectangle {
    id: rect

    function calculateHeight(a: real): real {
        return rect.width / 2;
    }

    width: 100
    height: calculateHeight(10)
}
```
## enumeration属性
```css
/* 枚举类型和值都需要首字母大写，比如下面的TextType，Normal */
Text {
    enum TextType {
        Normal,
        Heading
    }

    property int textType: MyText.TextType.Normal

    font.bold: textType === MyText.TextType.Heading
    font.pixelSize: textType === MyText.TextType.Heading ? 24 : 12
}
```
## 属性绑定
```css
/* 即绑定到另一个属性，并随着它的改变而改变，比如下面的height: parent.height */
Rectangle {
    width: 200; height: 200

    Rectangle {
        width: 100
        height: parent.height
        color: "blue"
    }
}
/* 如果将值重新赋值为一个静态值，则会破坏绑定 */
/* 点击id为second的Rectangle则不会破坏first.width的绑定，而点击id为third的Rectangle则会
破坏绑定 */
Window {
    width: 500
    height: 300
    visible: true
    title: qsTr("quickApp")

    Rectangle {
        id: first;
        width: 200;
        height: width/2;
        color: "red";
    }

    Rectangle {
        id: second;
        anchors.bottom: parent.bottom;
        width: 40;
        height: 40;
        color: "yellow"
        MouseArea {
            anchors.fill: parent;
            onClicked: first.width = first.width + 10;
        }
    }

    Rectangle {
	    id: third;
        anchors.bottom: parent.bottom;
        anchors.left: second.right
        width: 40;
        height: 40;
        color: "blue";
        MouseArea {
            anchors.fill: parent;
            onClicked: first.height = 20;
            /* 使用Qt.binding()来创建新的绑定 */
            /* onClicked: first.height = Qt.binding(function(){
	            return first.width + 20
            }) */
        }
    }
}

/* 可以包含任何有效的javascript语句 */
height: parent.height / 2
height: Math.min(parent.width, parent.height)
height: parent.height > 100 ? parent.height : parent.height/2
height: {
    if (parent.height > 100)
        return parent.height
    else
        return parent.height / 2
}
height: someMethodThatReturnsHeight()
```
## 布局
## 行
```css
Row {
	id: row;
	spacing: 20;
	/* 只有Row有layoutDirection，Column没有 */
	layoutDirection: Qt.RightToLeft;
	leftPadding: 20;
	topPadding: 20;
	padding: 20;
	Rectangle { width: 200; height: 200 }
	Rectangle { width: 200; height: 200 }
}

Button {
	onClicked: {
		for (var i = 0; i < row.children.length; i++) {
			console.log(row.children[i] instanceof Button); 
		}
	}
}
```
## Flow
```css
/* 基本作用和Row差不多，但如果一行放不下，Flow会自动换行 */
Flow {
	flow: Flow.TopToBottom;
	
	width: 500;

	spacing: 20;
	leftPadding: 20;
	topPadding: 20;
	padding: 20;
	Rectangle { width: 200; height: 200 }
	Rectangle { width: 200; height: 200 }
}
```
## 网格
```css
Grid {
	rows: 2;
	columns: 1;
	rowSpacing: 5;
	columnSpacing: 5;
	Rectangle { width: 200; height: 200 }
	Rectangle { width: 200; height: 200 }
}
```
## 生成多个相同元素Repeater
```css
Row {
	spacing: 10;
	Repeater {
		model: 3;
		Rectangle { y: index*50; width: 200; height: 200; color: "red" }
	}
}

Row {
	spacing: 10;
	Repeater {
		id: rep;
		model: ["first", "second", "third"];
		Rectangle { y: index*50; width: 200; height: 200; text: modelData }
	}
	
	Button {
		onClicked: console..log(rep.itemAt(1));
	}
}
```
## StackLayout
```css
 StackLayout {
     id: layout;
     anchors.fill: parent;
     currentIndex: 1;
     Rectangle {
         color: StackLayout.isCurrentItem ? "blue" : "red";
         implicitWidth: 200;
         implicitHeight: 200;
     }
     Rectangle {
         color: 'plum';
         implicitWidth: 300;
         implicitHeight: 200;
     }
 }
```
## 全局对象
```css
/* qml提供一个全局对象，其包含函数，枚举等 */
/* 比如下面使用Qt.rgba()函数 */
Window {
    width: 500;
    height: 300;
    visible: true;
    title: qsTr("quickApp");
    
    Text {
	    anchor.centerIn: parent;
	    color: Qt.rgba(1, 0, 0, 1);
    }
}

/*
Qt.quit()
Qt.fontFamilies()
Qt.openUrlExternally("https://baidu.com")
Qt.openUrlExternally("file:///D:/a.png")
Qt.platform.os
*/
```
## 视图基础元素Item
```css
/* 其他视图元素都继承自Item，但Item本身并不绘制任何图形 */
Item {
	Component.onCompleted: {}
}
```
## 获取鼠标事件MouseArea
```css
MouseArea {
	anchors.fill: parent;
	cursorShape: Qt.CrossCursor;
	
	acceptedButtons: Qt.LeftButton | Qt.RightButton;
	onClicked: console.log("");
	onPressed: {
		var ret = pressedButtons & Qt.LeftButton;
		console.log(ret ? "left" : "right")
	}
	onReleased: console.log("");
	onDoubleClicked: console.log("");
	
	pressAndHoldInterval: 300;
	onPressAndHold: console.log("");
	
	hoverEnabled: true;
	onContainMouseChanged: console.log("");
}

/* propagateComposedEvents事件冒泡 */
 import QtQuick 2.0

 Rectangle {
     color: "yellow"
     width: 100; height: 100

     MouseArea {
         anchors.fill: parent
         onClicked: console.log("clicked yellow")
     }

     Rectangle {
         color: "blue"
         width: 50; height: 50

         MouseArea {
             anchors.fill: parent
             propagateComposedEvents: true
             onClicked: (mouse)=> {
                 console.log("clicked blue")
                 mouse.accepted = false
             }
         }
     }
 }
```
## 图片Image
```css
/* 不使用qt资源文件 */
/* 在CMakeLists.txt中添加下面内容，images/a.png和Main.qml位于同一目录 */
qt_add_qml_module(appQuick
    URI Quick
    VERSION 1.0
    QML_FILES
        Main.qml
        
    RESOURCES                 /* 新添加的 */
        images/a.jpg          /* 新添加的 */
)

/* 使用qt资源文件 */
/* 需要在CMakeLists.txt中添加set(CMAKE_AUTORCC ON)后才能使用qt资源文件 */
Image {
	id: img;
	source: "qrc:/images/a.png";
	/* source: "qrc:/images/a.png"; */
	/* source: "https://baidu.com"; */
	
	clip: true;
	fillMode: Image.PreserveAspectFit;
	
	width: 200;
	height: 200;
}

/* gif图片 */
AnimatedImage {
	id: img;
	source: "qrc:/images/a.gif";
	speed: 10;
}
```
## 自定义类型
```css
/* 自定义类型最简单的形式是基于文件的类型 */
/* 文件名必须以大写字母开头，并且由字母数字字符或下划线组成 */
/* 以这种方式定义的类型会自动提供给同一本地目录下的其它QML文件 */
/* 根对象的所有属性、信号和方法，都可以被外部访问 */
/* MyButton.qml */
Item {
	width: first.width;
	height: first.height;
	
	Rectangel {
		id: first;
		width: 200;
		height: 200;
	}
}
/* main.qml */
Window {
	width: 400;
	height: 300;
	visible: true;

	MyButton {}
}


/* 如果不想在一个文件中定义一个新类型，则可以使用component定义内联组件 */
import QtQuick

Window {
    id: root;
    width: 800;
    height: 600;
    visible: true;
    title: "hello";

    component OkRectangle: Rectangle {
            width: 50;
            height: 50;
            color: "red";
    }

    OkRectangle {}
}

```
## 动态创建和删除对象
```css
/* 动态创建的对象没有id */
/* 使用Qt.createComponent()函数 */
/* Sprite.qml */
import QtQuick
Rectangle { width: 80; height: 50; color: "red" }

/* main.qml */
import QtQuick
import "componentCreation.js" as MyScript
Rectangle {
    id: appWindow
    width: 300; height: 300
    Component.onCompleted: MyScript.createSpriteObjects();
}

/* componentCreation.js */
var component;
var sprite;
function createSpriteObjects() {
	/* 创建component */
    component = Qt.createComponent("Sprite.qml");
    /* createObject函数第一个参数是父控件，第二个参数为初始化的属性 */
    sprite = component.createObject(appWindow, {x: 100, y: 100});

    if (sprite == null) {
        // Error Handling
        console.log("Error creating object");
    }
}


/* 还可以使用Qt.createQmlObject()函数，但不推荐，因为很耗时 */
const newObject = Qt.createQmlObject(`
    import QtQuick

    Rectangle {
        color: "red"
        width: 20
        height: 20
    }
    `,
    parentItem,
    "myDynamicSnippet"
);


/* 使用destroy()函数删除动态创建的对象 */
/* MyButton.qml */
import QtQuick.Controls
Button {
    id: rect
    width: 300;
    height: 200;
    text: "abc"
    onClicked: rect.destroy()
}
/* main.qml */
import QtQuick
Window {
    id: root
    visible: true
    width: 800
    height: 600
    title: "音乐播放器"
    Component.onCompleted: {
        var component = Qt.createComponent("MyButton.qml")
        component.createObject(root)
    }
}

```
## 视图
```css
/* 视图需要被展示的数据和定义数据如何展示 */
/* ContactModel.qml */
 import QtQuick

 ListModel {
     ListElement {
         name: "Bill Smith"
         number: "555 3264"
     }
     ListElement {
         name: "John Brown"
         number: "555 8426"
     }
     ListElement {
         name: "Sam Wise"
         number: "555 0473"
     }
 }
/* main.qml */
 import QtQuick

 ListView {
     width: 180; height: 200

     model: ContactModel {}
     delegate: Text {
         text: name + ": " + number
     }
 }




/* 可以通过list.contentItem.children[0]来访问ListView的元素 */
ListView {
	id: list;
}
```
## javascript资源
```css
/* javascript文件称为javascript资源，分为两种，一种是共享的，另一种是每个实例都是独立的 */
/* 独立的 */
/* MyButton.qml */
import QtQuick 2.0
import "my_button_impl.js" as Logic /* 生成一个javascript资源的实例 */

Rectangle {
    id: rect
    width: 200
    height: 100
    color: "red"

    MouseArea {
        id: mousearea
        anchors.fill: parent
        onClicked: Logic.onClicked(rect)
    }
}
/* my_button_impl.js */
var clickCount = 0;   /* 每个实例拥有一个该变量 */
function onClicked(button) {
    clickCount += 1;
    if ((clickCount % 5) == 0) {
        button.color = Qt.rgba(1,0,0,1);
    } else {
        button.color = Qt.rgba(0,1,0,1);
    }
}


/* 共享的 */
/* 需要在代码前添加.pragma library */
/* factorial.js */
.pragma library

var factorialCount = 0;

function factorial(a) {
    a = parseInt(a);

    if (a > 0)
        return a * factorial(a - 1);

    factorialCount += 1;

    return 1;
}

function factorialCallCount() {
    return factorialCount;
}
/* Calculator.qml */
import QtQuick 2.0
import "factorial.js" as FactorialCalculator /* 即使多个Calculator.qml被创建，该javascript资源也只加载一次 */

Text {
    width: 500;
    height: 100;
    property int input: 17;
    text: "The factorial of " + input + " is: " + FactorialCalculator.factorial(input);
}
```
## 模块
```css
/* 编写qmldir文件，文件中的每条命令都必须单独成行，注释以#开头 */
/* c++插件加载是一项相对昂贵的操作，建议最多指定一个插件 */
/* 示例 */
/* Style.qml */
pragma Singleton
import QtQuick 2.0

QtObject {
    property int textSize: 20
    property color textColor: "green"
}

/* qmldir */
module CustomStyles
singleton Style 1.0 Style.qml

/* 使用 */
import QtQuick 2.0
import CustomStyles 1.0

Text {
    font.pixelSize: Style.textSize
    color: Style.textColor
    text: "Hello World"
}




/* qmldir文件示例 */
module ExampleModule
CustomButton 2.0 CustomButton20.qml
CustomButton 2.1 CustomButton21.qml
plugin examplemodule
MathFunctions 2.0 mathfuncs.js
```
## 设置全局变量
```css
/* main.cpp */
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>

int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);
    QQmlApplicationEngine engine;
    engine.loadFromModule("Quick", "Main");
    
    /* 使用setContextProperty函数设置全局变量 */
    QQmlContext *context = engine.rootContext();
    context->setContextProperty("SCREEN_WIDTH", 200);

    return app.exec();
}


/* Main.qml */
import QtQuick

Window {
    visible: true;
    /* 使用全局变量 */
    width: SCREEN_WIDTH;
    height: 400;
    title: qsTr("Hello World");
}
```
## 与c++交互
```css
/* myobject.h */
#include <QObject>
#include <QDebug>

class MyObject : public QObject
{
    Q_OBJECT
    Q_PROPERTY(QString name READ name WRITE setName NOTIFY nameChanged)
    /* 上面这句也可以用下面这句代替 */
    /* Q_PROPERTY(QString name MEMBER m_name NOTIFY nameChanged) */

public:
    explicit MyObject(QObject *parent = nullptr) : QObject(parent) {}
    QString name() const { return m_name; }
    void setName(const QString &name) { m_name = name; emit nameChanged(); }
    Q_INVOKABLE void printName() { qDebug() << "Name:" << m_name; }

signals:
    void nameChanged();

public slots:
    void ok() { qDebug() << "slot function"; }

private:
    QString m_name;
};




/* main.cpp */
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include "myobject.h"

int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);
    QQmlApplicationEngine engine;

    /* 注册c++模块 */
    /* 参数分别为模块名，主版本号，次版本号，组件名 */
    /* 一定要放在engine.loadFromModule("Quick", "Main");这句前面 */
    qmlRegisterType<MyObject>("com.example", 1, 0, "MyObject");

    engine.loadFromModule("Quick", "Main");
    return app.exec();
}




/* Main.qml */
import QtQuick.Window 2.12
import QtQuick.Controls 2.5
import com.example 1.0

Window {
	visible: true
	width: 480
	height: 400
	title: qsTr("Hello World")

	MyObject {
		id: myObject
		name: "John"
	}

	Text {
		text: myObject.name
	}

	Button {
		y: 20
		text: "Print Name"
        onClicked: {
	        /* 普通函数需要在前面加Q_INVOKABLE，而槽函数可以直接调用 */
            myObject.printName()
            myObject.ok()
        }
	}
}
```
## 设置数据Settings
```css
Window {
    id: root;
    width: 800;
    height: 600;
    visible: true;
    title: "hello";
    
    Settings {
	    id: settings;
	    fileName: "D:\\settings.ini";
	    category: "General"
	    property int i: 10;
	    property alias x: root.x;
    }
    
    Button {
	    text: "click";
	    /* 访问Settings中的值，参数分别为访问的变量名和找不到变量时返回的默认值 */
	    onClicked: console.log(settings.value("i", 0));
	    /* 设置Settings中的变量 */
	    /* onClicked: settings.setValue("j", 5); */
    }
}
```
## 对话框Dialog
```css
/* 弹出窗口需调用open()函数，对下面的代码则为dialog.open() */
 Dialog {
     id: dialog
     title: "Title"
     /* modal即鼠标是否可以点击其他窗口 */
     modal: false
     /* standardButtons: DialogButtonBox.Ok | DialogButtonBox.Cancel */
     standardButtons: Dialog.Ok | Dialog.Cancel
     onAccepted: console.log("Ok clicked")
     onRejected: dialog.close()
 }
```
## 滚筒Tumbler
```css
Tumbler {
    id: hoursTumbler
    model: 24
    delegate: TumblerDelegate {
        text: modelData
    }
}
```
滚筒如下图所示
![](/images/qml_1.png)
## 输入一行文字TextInput
```css
TextInput {
	
}
```
## 输入多行文字TextEdit
```css

```
## 动画
```css
/* 动画类型
PropertyAnimation
NumberAnimation
ColorAnimation
RotationAnimation
SequentialAnimation
ParallelAnimation
*/
Window {
    id: root;
    width: 800;
    height: 600;
    visible: true;
    title: "hello";
    
    Rectangle {
	    id: rect;
	    width: 200;
	    height: 200;
	    color: "blue";
	    
	    MouseArea {
		    anchors.fill: parent;
		    onClicked: first_ani.start()
	    }
	    
	    NumberAnimation {
		    id: first_ani;
		    target: rect;
		    properties: "opacity";
		    from: 0.1;
		    to: 1.0;
		    duration: 2000;
	    }
    }
}


Image {
	id: root;
	x: 100;
	
	PropertyAnimation on x {
		target: root;
		/* from: 0; */
		to: 200;
		duration: 5000;
		running: true;
	}
	
	NumberAnimation on x {
		to: 200;
		duration: 5000;
		running: true;
	}
	
	RotationAnimation on rotation {
		to: 360;
		duration: 5000;
		running: true;
	}

	/* y值改变便会触发动画 */
	Behavior on y {
		NumberAnimation { duration: 5000 }
	}
}
```
## 状态
```css
Item {
	width: 400; height: 400;
	
	state: "stop";
	states: [
		State {
			name: "stop";
			PropertyChanges { target: light1; color: "red" }
			PropertyChanges { target: light2; color: "black" }
		},
		State {
			name: "go";
			PropertyChanges { target: light1; color: "black" }
			PropertyChanges { target: light2; color: "green" }
		}
	]
	
	Rectangle {
		id: light1;
		x: 0; y: 0;
		width: 200; height: 200;
		radius: width/2;
	}
	
	Rectangle {
		id: light2;
		x: 0; y: 240;
		width: 200; height: 200;
		radius: width/2;
	}
	
	MouseArea {
		anchors.fill: parent;
		onClicked: parent.state = (parent.state === "stop" ? "go" : "stop");
	}
}

/* states和transitions一起使用 */
Item {
	width: 400;
	height: 400;
	
	Rectangle {
		id: rect;
		width: 200;
		height: 200;
		color: "red";
	}
	
	state: "stop";
	states: [
		State {
			name: "stop";
			PropertyChanges { target: rect; color: "blue" }
		},
		State {
			name: "go";
			PropertyChanges { target: rect; color: "black" }
		}
	]
	
	transitions: [
		Transition {
			from: "stop";
			to: "go";
			ColorAnimation { target: rect; duration: 1000 }
		},
		Transition {
			from: "go";
			to: "stop";
			ColorAnimation { target: rect; duration: 1000 }
		}
	]
	
	MouseArea {
		anchors.fill: parent;
		onPressed: button.state = "go";
		onReleased: button.state = "stop";
	}
}
```
## 组件component
```css
Window {
    id: root;
    width: 800;
    height: 600;
    visible: true;
    title: "hello";
    
    Component.onCompleted: console.log("start");
    Component.onDestruction: console.log("end");
}
```
## Loader
```css
Window {
    id: root;
    width: 800;
    height: 600;
    visible: true;
    title: "hello";
    
    Component {
	    id: com;
	    Rectangle {
		    width: 200;
		    height: 200;
		    color: "black";
	    }
    }
    
    Loader {
	    id: load;
	    sourceComponent: com;
	    asynchronous: true;
	    
	    onStatusChanged: console.log("status: ", status);
    }
    
    Button {
	    width: 50;
	    height: 50;
	    x: 200;
	    onClicked: {
		    load.item.width = 50;
		    load.item.height = 50;
		    load.item.color = "red";
		    /* 删除动态加载的Component */
		    /* load.sourceComponent = null; */
	    }
    }
}
```
## Button
```css
Button {
	id: btn;
	width: 100;
	height: 100;
	background: Rectangle {
		color: "red";
	}
	contentItem: Text {
		text: "hello";
		font.pointSize: 20;
	}
	text: "+";
	icon.source: "./images/a.png";
	icon.width: 50;
	icon.height: 50;
	icon.color: "blue";
}
```
## CheckBox
```css
CheckBox {
	checked: true;
	text: "male";
	tristate: true;    /* 三种状态 */
}
```
## Text
```css
Text {
	text: "hello";
	color: "red";
	
	font.family: "Ubuntu";
    font.bold: true;
    font.italic: true;
    font.underline: true;
    font.letterSpacing: 10;
    font.pointSize: 20;
    font.pixelSize: 20;
	
	elide: Text.ElideMiddle;    /* 当文本超过宽度时，将省略符设置在文本左，中，右侧 */
	wrapMode: Text.WordWrap;
	style: Text.SunKen;
	styleColor: "#000000";
}

Text {
	font.pointSize: 24;
	textFormat: Text.MarkdownText;
	text: "**Hello** *World!*";
}

Text {
	text: "<a href=\"https://baidu.com\">百度</a>";
	onLinkActivated: console.log("");
	onLinkHovered: console.log("");
}
```
## Timer
```css
Window {
    id: root;
    width: 800;
    height: 600;
    visible: true;
    title: "hello";

	property int time: 0;
    Timer {
	    id: timer;
	    interval: 1000;
	    /* running: true */
	    repeat: true;
	    triggeredOnStart: true;
	    onTriggered: time += 1;
    }
    
    Button {
	    text: "start";
	    onClicked: timer.start();
	}
}
```
