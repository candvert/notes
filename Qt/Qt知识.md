- [Qt](#Qt)
	- [关键概念](#关键概念)
	- [QStringLiteral](#QStringLiteral)
	- [QStackedWidget实现局部界面变换](#QStackedWidget实现局部界面变换)
	- [QSplitter分隔左右或上下界面](#QSplitter分隔左右或上下界面)
	- [QScrollArea创建可以滚动的区域](#QScrollArea创建可以滚动的区域)
	- [QWidget的constMenuEvent函数创建右键菜单](#QWidget的constMenuEvent函数创建右键菜单)
	- [QTreeWidget实现展开折叠效果](#QTreeWidget实现展开折叠效果)
	- [悬停显示图片等复杂信息](#悬停显示图片等复杂信息)
	- [设置界面固定大小和去掉界面标题栏](#设置界面固定大小和去掉界面标题栏)
	- [下拉框](#下拉框)
	- [更改应用标题和应用图标](#更改应用标题和应用图标)
	- [设置应用的作者和应用显示的名字](#设置应用的作者和应用显示的名字)
	- [设置应用主题](#设置应用主题)
	- [设置鼠标事件追踪](#设置鼠标事件追踪)
	- [返回qt内置icon](#返回qt内置icon)
	- [设置文件](#设置文件)
	- [系统托盘](#系统托盘)
	- [预定义宏](#预定义宏)
	- [智能指针](#智能指针)
	- [容器类](#容器类)
	- [QVariant类](#QVariant类)
	- [QRandomGenerator生成随机数](#QRandomGenerator生成随机数)
	- [获取当前时间](#获取当前时间)
	- [QChar](#QChar)
	- [设置互斥QAction](#设置互斥QAction)
	- [动态属性](#动态属性)
	- [创建默认右键菜单](#创建默认右键菜单)
	- [鼠标左键拖动](#鼠标左键拖动)
	- [QString用来表示c++中的String](#QString用来表示c++中的String)
	- [qDebug()打印消息](#qDebug()打印消息)
	- [QWidget](#QWidget)
	- [QObject](#QObject)
	- [信号与槽](#信号与槽)
	- [connect和disconnect](#connect和disconnect)
	- [事件](#事件)
	- [自定义控件](#自定义控件)
	- [QSS(Qt Style Sheet)](#QSS(Qt Style Sheet))
	- [布局](#布局)
	- [Json处理](#Json处理)
	- [网络](#网络)
	- [QFile操作文件](#QFile操作文件)
	- [数据库](#数据库)
	- [QObject定时器](#QObject定时器)
	- [QTimer定时器](#QTimer定时器)
	- [QElapsedTimer](#QElapsedTimer)
	- [QPixmap保存图片](#QPixmap保存图片)
	- [QFileDialog打开资源管理器](#QFileDialog打开资源管理器)
	- [设置命令行输出的格式](#设置命令行输出的格式)
	- [QMessageBox对话框](#QMessageBox对话框)
  
  
  
  
  
	- [QTcpSocket](#QTcpSocket)
	- [QTcpServer](#QTcpServer)
	- [QThread线程](#QThread线程)
	- [QMenuBar](#QMenuBar)
	- [QToolBar](#QToolBar)
	- [QStatusBar](#QStatusBar)
	- [QDockWidget](#QDockWidget)
	- [QDialog对话框](#QDialog对话框)
	- [QColorDialog::getColor()](#QColorDialog::getColor())
	- [QFileDialog::getOpenFileName()](#QFileDialog::getOpenFileName())
	- [QFontDialog::getFont()](#QFontDialog::getFont())
	- [QPainter绘图](#QPainter绘图)
	- [QPen](#QPen)
	- [QBrush](#QBrush)
	- [QPalette](#QPalette)
	- [QFileInfo](#QFileInfo)
	- [QFont](#QFont)
	- [QPlainTextEdit](#QPlainTextEdit)
	- [QProgressBar](#QProgressBar)
	- [QSlider](#QSlider)
	- [QButtonGroup用于单选按钮](#QButtonGroup用于单选按钮)
	- [QComboBox](#QComboBox)
	- [QTimeEdit](#QTimeEdit)
	- [QDateEdit](#QDateEdit)
	- [QDateTimeEdit](#QDateTimeEdit)
	- [打包成exe](#打包成exe)

# Qt
## 关键概念
```cpp
// 1. 对象树
// 当一个控件删除时，它包含的所有子控件被删除

// 2. 信号与槽
// 使用信号与槽必须是QObject的子类并在类定义时包含Q_OBJECT宏
// 使用connect函数连接信号和槽，使用disconnect函数解除连接
// 使用qOverload明确参数类型
connect(ui->checkBox, &QCheckBox::clicked, this, qOverload<bool>(&Widget::do_click));
// 3. qobject_cast<>进行动态类型转换
// 使用sender()函数获取信号发射者
QPushButton *btn = qobject_cast<QPushButton*>(sender());

// QMetaObject::propertyCount()返回元对象描述的类中定义的属性个数，但是其中不包括对象的动态属性
```
## QStringLiteral
```cpp
// 这样需要在运行时分配内存
this->setWindowIcon(QIcon(":/background-1.png"));

// 这样在编译时就分配了内存，可以提高效率
this->setWindowIcon(QIcon(QStringLiteral(":/background-1.png"));
```
## QStackedWidget实现局部界面变换
```cpp
// 最好将QStackWidget放在一个布局里，不然内容会显示不全
QStackedWidget *s = new QStackedWidget();
s->addWidget(widget);

s->count();
s->currentIndex();
s->currentWidget();
s->setCurrentIndex(0);
s->setCurrentWidget(widget);
```
## QSplitter分隔左右或上下界面
```cpp
// QSplitter可以分隔任何数量的控件
QSplitter *splitter = new QSplitter(Qt::Horizontal); // 水平分隔
splitter->addWidget(leftWidget);
splitter->addWidget(rightWidget);

// setStretchFactor(int index, int stretch)函数可以设置拉伸因子
splitter->setStretchFactor(0, 1);
splitter->setStretchFactor(1, 2);

leftWidget->setMinimumWidth(100);
rightWidget->setMinimumWidth(100);

// setCollapsible(int index, bool)函数可以使不论如何拖动分隔条都不会让控件消失，如果不设置则控件可能随分割条的拖动而消失
splitter->setCollapsible(0, false);
splitter->setCollapsible(1, false);

// setHandleWidth(int)函数设置分割条的宽度
splitter->setHandleWidth(20);
```
## QScrollArea创建可以滚动的区域
```cpp
QScrollArea *scroll = new QScrollArea(this);
// 关闭水平滚动条
// 参数有三个值，Qt::ScrollBarAlwaysOff、Qt::ScrollBarAlwaysOn、Qt::ScrollBarAsNeeded
scrollArea->setHorizontalScrollBarPolicy(Qt::ScrollBarAlwaysOff);

// qss美化
// QScrollBar:vertical { background: lightgray; }是必须的，不然后面的样式不会生效
QScrollBar:vertical {
    background: lightgray; /* 背景色，可以根据需要调整 */
    width: 10px; /* 滚动条的宽度 */
}

QScrollBar::handle:vertical {
    background: darkgray; /* 滚动条的滑块颜色 */
}

QScrollBar::handle:vertical:hover {
    background: gray; /* 鼠标悬停时的滑块颜色 */
}

QScrollBar::add-line:vertical,
QScrollBar::sub-line:vertical {
    background: none; /* 去掉上下箭头的背景 */
}

QScrollBar::sub-page:vertical {
    background: red; /* 去掉上下页的背景 */
}

QScrollBar::add-page:vertical {
    background: blue; /* 去掉上下页的背景 */
}
```

用上面的qss后的样式

![用上面的qss后的样式](/images/qt_1.png)
## QWidget的constMenuEvent函数创建右键菜单
```cpp
void contextMenuEvent(QContextMenuEvent *event) override {
	QMenu menu(this);
        
	// 添加菜单项
	QAction *addSongAction = menu.addAction("加入歌曲");
	QAction *removeSongAction = menu.addAction("从歌单中删除");
	QAction *viewPlaylistAction = menu.addAction("查看播放列表");
	menu.addSeparator(); // 添加分隔线
	QAction *shareAction = menu.addAction("分享");

	// 连接信号和槽
	connect(addSongAction, &QAction::triggered, this, &MainWindow::addSong);
	connect(removeSongAction, &QAction::triggered, this, &MainWindow::removeSong);
	connect(viewPlaylistAction, &QAction::triggered, this, &MainWindow::viewPlaylist);
	connect(shareAction, &QAction::triggered, this, &MainWindow::share);

	// 显示菜单
	menu.exec(event->globalPos());
}
```
## QTreeWidget实现展开折叠效果
```cpp
// 创建 QTreeWidget
QTreeWidget treeWidget;
treeWidget.setHeaderHidden(true); // 隐藏表头

// 添加父节点 "Top Artists"
QTreeWidgetItem *topArtists = new QTreeWidgetItem(&treeWidget);
topArtists->setText(0, "Top Artists");

// 添加子节点
QStringList artists = { "Candy_Wind", "Pianoboy高至豪", "The Chainsmokers", "四季音色", "闫东炜", "周杰伦" };
for (const QString &artist : artists) {
    QTreeWidgetItem *item = new QTreeWidgetItem(topArtists);
    item->setText(0, artist);
}

// 展开父节点（默认显示为展开状态）
topArtists->setExpanded(true);

// 美化树控件的样式（可选）
treeWidget.setStyleSheet(
    "QTreeView::branch:closed:has-children {"
    "    image: url(:/icons/arrow_right.png); /* 关闭状态的图标 */"
    "}"
    "QTreeView::branch:open:has-children {"
    "    image: url(:/icons/arrow_down.png); /* 打开状态的图标 */"
    "}"
    "QTreeWidget::item {"
    "    font-size: 14px;"
    "    padding: 5px;"
    "}"
    );

// 设置窗口大小
treeWidget.setWindowTitle("Top Artists");
treeWidget.resize(300, 200);
treeWidget.show();
```
## 悬停显示图片等复杂信息
```cpp
#include <QApplication>
#include <QListWidget>
#include <QLabel>
#include <QVBoxLayout>
#include <QWidget>
#include <QEvent>
#include <QHelpEvent>
#include <QPoint>

class HoverInfo : public QWidget {
public:
    HoverInfo(QWidget *parent = nullptr) : QWidget(parent) {
        setWindowFlags(Qt::ToolTip); // 让窗口表现为工具提示

        QVBoxLayout *layout = new QVBoxLayout(this);

        // 添加图片和文本
        QLabel *imageLabel = new QLabel;
        imageLabel->setPixmap(QPixmap(":/images/album_cover.png").scaled(50, 50));
        QLabel *textLabel = new QLabel("🎵 happy\n👤 duheyu");

        layout->addWidget(imageLabel);
        layout->addWidget(textLabel);
    }
};

class CustomListWidget : public QListWidget {
public:
    CustomListWidget(QWidget *parent = nullptr) : QListWidget(parent) {
        hoverWidget = new HoverInfo(this);
        hoverWidget->hide();
    }

protected:
    bool event(QEvent *event) override {
        if (event->type() == QEvent::ToolTip) {
            QHelpEvent *helpEvent = static_cast<QHelpEvent *>(event);
            QListWidgetItem *item = itemAt(helpEvent->pos());
            if (item && item->text() == "happy") {
                hoverWidget->move(helpEvent->globalPos() + QPoint(10, 10));
                hoverWidget->show();
            } else {
                hoverWidget->hide();
            }
            return true;
        }
        return QListWidget::event(event);
    }

private:
    HoverInfo *hoverWidget;
};

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);

    // 自定义 QListWidget
    CustomListWidget listWidget;
    listWidget.setWindowTitle("Custom Hover Example");
    listWidget.resize(300, 200);

    // 添加列表项
    listWidget.addItem("happy");
    listWidget.addItem("new one");
    listWidget.addItem("Instant fade");
    listWidget.addItem("我喜欢");
    listWidget.addItem("日语歌");

    listWidget.show();
    return app.exec();
}
```
## 设置界面固定大小和去掉界面标题栏
```cpp
// 设置界面固定大小
this->setFixedSize(width, height);
this->setFixedSize(QSize(width, height));

// 去掉界面标题栏
this->setWindowFlag(Qt::FramelessWindowHint);
```
## 下拉框
```cpp
comboBox = new QComboBox;
// 获得当前index
comboBox->currentIndex();
// 添加item
void QComboBox::addItem(const QIcon &icon, const QString &text, const QVariant &userData = QVariant());
// 获得当前item的data
QVariant QComboBox::itemData(int index, int role = Qt::UserRole) const;
```
## 更改应用标题和应用图标
```cpp
// 更改应用标题
this->setWindowTitle("cc");

// 更改应用图标
this->setWindowIcon(QIcon(":/background-1.png"));
```
## 设置应用的作者和应用显示的名字
```cpp
// 设置应用的作者
QCoreApplication::setOrganizationName("NotepadNext");

// 设置应用显示的名字
QGuiApplication::setApplicationDisplayName("Notepad Next");

// 
QCoreApplication::setApplicationName(QStringLiteral("Zeal"));
QCoreApplication::setApplicationVersion(ZEAL_VERSION);
QCoreApplication::setOrganizationDomain(QStringLiteral("zealdocs.org"));
QCoreApplication::setOrganizationName(QStringLiteral("Zeal"));
```
## 输出操作系统和版本号
```cpp
// 输出例如"Windows 11 Version 23H2"
QSysInfo::prettyProductName();	// 返回QString
```
## 设置应用主题
```cpp
// 应用中的所有控件都为黑色
QApplication app(argc, argv);
app.setPalette(QPalette(QColor("#000000")));
```
## 设置鼠标事件追踪
```cpp
// 如果不设置，则控件只会收到至少按下一个键并移动鼠标的事件，设置后没有按键按下也会收到事件
this->setMouseTracking(true);
```
## 返回qt内置icon
```cpp
// style()是QWidget的一个函数，返回QStyle，下面代码返回QIcon
style()->standardIcon(QStyle::SP_MessageBoxInformation);
```
## 设置文件
```cpp
// 设置默认的配置文件格式为 INI 格式
QSettings::setDefaultFormat(QSettings::IniFormat);

// ini文件格式
[General]
username=admin
password=1234

// 创建 QSettings 对象，读取或写入配置
// 这段代码会创建一个名为 config.ini 的文件，并且将 username 存储在 General 部分下
QSettings settings("config.ini", QSettings::IniFormat);
settings.setValue("General/username", "admin");
```
## 系统托盘
```cpp
QSystemTrayIcon::isSystemTrayAvailable();	// 系统托盘是否可用
QApplication::setQuitOnLastWindowClosed(false);		// 设置窗口都关闭后应用在后台运行
trayIcon = new QSystemTrayIcon(this);
trayIcon->setContextMenu(trayIconMenu);				// 设置托盘右击后显示的菜单
trayIcon->showMessage("title", "message", icon, 1000);	// 在桌面右下角弹出系统消息，第三个参数为图标，第四个为时间
QSystemTrayIcon::setVisible(false);		// 隐藏系统托盘
trayIcon->setIcon(icon);		// 设置托盘图标
trayIcon->setToolTip("hello");	// 设置托盘图标的toolTip

// 当左键或中键单击托盘图标，则会触发QSystemTrayIcon::activated
connect(trayIcon, &QSystemTrayIcon::activated, this, &Window::iconActivated);
void Window::iconActivated(QSystemTrayIcon::ActivationReason reason)
{
    switch (reason) {
        case QSystemTrayIcon::Trigger:
            trayIcon->setIcon(icon);
            break;
    }
}

// 系统托盘图标，有四种
enum MessageIcon { NoIcon, Information, Warning, Critical };
QSystemTrayIcon::MessageIcon(1)
```
## 预定义宏
```cpp
// 命令行输出当前函数的声明
qInfo(Q_FUNC_INFO);
```
## 智能指针
```cpp
QScopedPointer<QPushButton> qapp(new QPushButton("hello"));
```
## 容器类
```cpp
// 顺序容器类有QList、QVector、QStack、QQueue
// 在Qt6中QVector是QList的别名，QList底层和c++的vector相似，QStack和QQueue是QList的子类
// 可以使用<<运算符或append函数向列表添加数据
QList<QString> list;
list << "hello";
list.append("world");

// 关联容器类QSet、QMap、QMultiMap、QHash、QMultiHash
```
## QVariant类
```cpp
QFont font;
QVariant var = font;						// 赋值给一个QVariant变量
// 使用setValue给QVariant类型变量赋值
// var.setValue(QFont());
QFont font2 = var.value<QFont>();			// 转换为QFont类型
```
## QRandomGenerator生成随机数
## 获取当前时间
```cpp
QDateTime::currentDateTime();
```
## QChar
```cpp
// QChar使用的是UTF-16编码，一个字符包含2字节数据
// QString存储的是QChar类型
```
## 设置互斥QAction
```cpp
QActionGroup *actionGroup = new QActionGroup(this);
actionGroup->addAction(ui->action1);
actionGroup->addAction(ui->action2);
actionGroup->setExclusive(true);
```
## 动态属性
```cpp
// 函数QObject::setProperty()设置属性值时，如果属性名称不存在，就会为对象定义一个新的属性并设置属性值，这时定义的属性称为动
// 态属性
Q_CLASSINFO("author", "ano")
Q_PROPERTY(int age READ age WRITE setAge NOTIFY ageChanged)
Q_PROPERTY(int score MEMBER m_score)
    
// const QMetaObject *meta=obj->metaObject();
// meta->propertyOffset();
// meta->propertyCount();
// meta->property(i).name();
// obj->property(propName).toString();
// meta->classInfoOffset();
// meta->classInfoCount();
// QMetaClassInfo classInfo=meta->classInfo(i);
// classInfo.name();
// classInfo.value();
```
## 创建默认右键菜单
```cpp
// 创建有复制粘贴的功能的菜单，在QLineEdit、QTextEdit和QPlainTextEdit等可以输入文字的控件上生效
this->createPopupMenu();
```
## 鼠标左键拖动
```cpp
// 重写mousePressEvent、mouseMoveEvent、mouseReleaseEvent三种事件
void Widget::mousePressEvent(QMouseEvent *event)
{
    if (event->button() == Qt::LeftButton)
    {
        mousePress = true;
    }
    movePoint = event->globalPos()->pos();
}
void Widget::mouseMoveEvent(QMouseEvent *event)
{
    if (mousePress)
    {
        QPoint movepos = event->globalPos();
        move(movepos - movePoint);
    }
}
void Widget::mouseMoveEvent(QMouseEvent *event)
{
    Q_UNUSED(event)
    mousePress = false;
}
```
## QString用来表示c++中的String
```cpp
QString s = "hello";
// 将整数类型转换为字符串
QString::number(235);
// 格式化字符串
QString("%1,%2").arg(23).arg("ok");
QString::asprintf("value=%i", value);
```
## qDebug()打印消息
```cpp
qDebug() << "hello";
// 可以直接打印QObject对象
QWidget widget;
qDebug() << widget;
```
## QWidget
```cpp
// 调整窗口大小
void resize(const QSize&);
void resize(int w, int h);
// 设置固定窗口大小
void setFixedSize(const QSize &s);
void setFixedSize(int w, int h);
// 设置widget相对于其parent的位置
void setGeometry(const QRect &);
void setGeometry(int x, int y, int w, int h);
// 将窗口移到某个坐标
void move(const QSize &);
void move(int x, int y);
```
## QObject
```cpp
QObject* parent() const;
QString objectName() const;
void setParent(QObject* parent);
void setObjectName(const QString &name);
connect();
disconnect();
// 启动定时器，返回一个id
int startTimer(int interval, Qt::TimerType timerType = Qt::CoarseTimer);
int startTimer(std::chrono::milliseconds interval, Qt::TimerType timerType = Qt::CoarseTimer);
// 关闭定时器
void killTimer(int id);
// 事件
virtual bool event(QEvent *e);
virtual bool eventFilter(QObject *watched, QEvent *event);
protected:
	virtual void timerEvent(QTimerEvent *event);
```
## 信号与槽
```cpp
// 信号可以连接到槽，也可以连接到信号
// 信号与槽可以一对一，一对多，多对一，多对多
// 信号的参数个数不能少于槽函数的参数个数
// 只有当信号关联的所有槽函数运行完毕后，才运行发射信号处后面的代码，比如emit后的代码

// 自定义信号
signal:
	void hide(int n);		// 返回值是void。只需声明，无需实现。可以有参数，可以重载


// 槽函数
// 可以写在public、protected、private下
// 返回值是void，可以有参数，可以重载
private slots:
	void slotHide();

// 使用emit关键字释放信号
connect(ui->LineEdit, &Widget::hide, &Widget::slotHide);
emit hide(5);

// 在槽函数里可以调用sender()函数，返回发出信号的QObject
QObject *QObject::sender() const;
```
## connect和disconnect
```cpp
// 不要连接多次，最好在构造函数中调用，保证只连接一次
// 示例，返回值是QMetaObject::Connection，可以用于调用后续的disconnect函数
connect(ui->LineEdit, &QPushButton::clicked, this, &Widget::on_cancelButton_clicked);
connect(ui->LineEdit, &QPushButton::clicked, &Widget::on_cancelButton_clicked);
connect(ui->LineEdit, &QPushButton::clicked, [this](){  });
connect(ui->LineEdit, SIGNAL(returnPressed(int)), this, SLOT(on_commitButton_clicked(int)));		// 不推荐
    
    
// 通过qOverload明确参数类型
connect(ui->checkBox, &QCheckBox::clicked, this, qOverload<bool>(&Widget::do_click));
connect(ui->checkBox, &QCheckBox::clicked, this, qOverload<>(&Widget::do_click));


// disconnect
// 1.解除一个发射者所有信号的连接
disconnect(myObject, nullptr, nullptr, nullptr);			// 静态函数形式
myObject->disconnect();									//成员函数形式
// 2.解除与一个特定信号的所有连接
disconnect(myObject, SIGNAL(mySignal()), nullptr, nullptr);	// 静态函数形式
myObject->disconnect(SIGNAL(mySignal()));					//成员函数形式
// 3.解除与一个特定接收者的所有连接
disconnect(myObject, nullptr, myReceiver, nullptr);			// 静态函数形式
myObject->disconnect(myReceiver);						//成员函数形式
// 4.解除特定的一个信号与槽的连接
disconnect(lineEdit, &QLineEdit::textChanged, label, &QLabel::setText);			// 静态函数形式
```
## 事件
```cpp
// 当事件发生，Qt会创建一个event对象代表它，并调用QObject的event函数
// 常见事件：QMouseEvent, QKeyEvent, QPaintEvent, QResizeEvent, QCloseEvent
// event函数会进行事件的分发，通过调用相应的事件处理函数来处理不同的事件
// 自己发送事件需要创建一个event对象，再通过QCoreApplication::sendEvent()和QCoreApplication::postEvent()来发送
// 要拦截相应事件，需要重写event函数，下面代码拦截了鼠标按下事件
bool Widget::event(QEvent *event)
{
    if (event->type() == QEvent::MouseButtonPress)
    {
        qDebug() << "good";
        return true;
    }
    return QWidget::event(event);
}

// 重载keyPressEvent
void Widget::keyPressEvent(QKeyEvent *event)
{
    if (event->key() == Qt::Key_Enter || event->key() == Qt::Key_Return) {
        qDebug() << "Pressed";
    }
    QWidget::keyPressEvent(event);
}

// 重载paintEvent
void Widget::paintEvent(QPaintEvent *event)
{
    QPainter painter(this);
    
    QPen pen(QColor(255, 0, 0));
    pen.setWidth(1);
    
    QBrush brush(Qt::cyan);
    
    painter.setPen(pen);
    painter.setBrush(brush);
    painter.drawLine(QPoint(0, 0), QPoint(100, 100));
	
    // 保存目前设置
    painter.save();
    painter.setRenderHint(QPainter::Antialiasing);
    // 还原设置
    painter.restore();
    
    // 画图片
    painter.drawPixmap(0, 0, QPixmap(":/ok.png"));
}
```
## 自定义控件
```cpp
// task.h
#ifndef TASK_H
#define TASK_H

#include <QWidget>
#include <QLabel>
#include <QPushButton>
#include <QHBoxLayout>
#include <QCheckBox>
#include <QMouseEvent>
#include <QDockWidget>
#include <QMenu>
#include <QAction>

class Task : public QWidget
{
    Q_OBJECT
        public:
    explicit Task(QWidget *parent = nullptr);
    QAction *action;
    QLabel *label;

    signals:
    void actionSignal();

    protected:
    virtual void mousePressEvent(QMouseEvent *e) override;

    private:
    QCheckBox *checkBox;
    QPushButton *button;
    QMenu *contextMenu;
};

#endif // TASK_H

// task.cpp
#include "task.h"

Task::Task(QWidget *parent)
    : QWidget{parent}
{
    label = new QLabel("learning", this);
    checkBox = new QCheckBox(this);
    button = new QPushButton("删除", this);

    // 添加组件
    QHBoxLayout *layout = new QHBoxLayout(this);
    label->setAlignment(Qt::AlignCenter);
    layout->addWidget(label);
    layout->addWidget(checkBox);
    layout->addWidget(button);

    // 设置布局
    layout->setAlignment(label, Qt::AlignLeft);
    layout->setAlignment(checkBox, Qt::AlignRight);
    setLayout(layout);

    // 设置信号与槽
    connect(button, &QPushButton::clicked, [this](){delete this;});
}

void Task::mousePressEvent(QMouseEvent *event)
{
    QPoint pos = event->pos();
    QWidget *child = childAt(pos);

    // 鼠标左击，跳转详情页；鼠标右击，跳出菜单
    if (child) {
        if (child->isWidgetType()) {
            if (qobject_cast<QLabel*>(child)) {
                if (event->button() == Qt::LeftButton) {
                    qDebug() << "left click";
                } else if (event->button() == Qt::RightButton) {
                    contextMenu = new QMenu(this);

                    // 添加菜单项
                    action = contextMenu->addAction("成为当前任务");
                    contextMenu->move(event->globalPos());
                    contextMenu->show();
                    connect(action, &QAction::triggered, this, &Task::actionSignal);
                }
            }
        }
    }
}
```
## QSS(Qt Style Sheet)
```cpp
// 语法和css相似，组件也是盒子模型，即padding，border，margin
// 当和除QSS设置的样式冲突时，会优先使用QSS，即保证QSS是有效的
// 全局使用
QFile file(":/style.qss");
file.open(QFile::ReadOnly);
QString styleSheet = QString::fromLatin1(file.readAll());
qApp.setStyleSheet(styleSheet);
// 应用于某个组件
button->setStyleSheet("color: red; background-color: white");
button->setStyleSheet("QPushButton { color: red; background-color: white }");

// 选中所有组件
* { color: red; background-color: white }
// 选中的类及其子类都会生效，比如继承自QPushButton的MyPushButton也会生效
QPushButton { color: red; background-color: white }
// 选中color属性为red的QPushButton类及其子类
QPushButton[color="red"] { color: red; background-color: white }
// 只有选中的类生效，子类不受影响
.QPushButton { color: red; background-color: white }
// 选中对象名为okButton的组件
#okButton { color: red; background-color: white }
// 选中对象名为okButton的QPushButton
QPushButton#okButton { color: red; background-color: white }
// 选中所有从属于QDialog的QPushButton类的实例，即QDialog对话框里的所有QPushButton按钮
QDialog QPushButton { color: red; background-color: white }
// 所有直接从属于QDialog的QPushButton类的实例
QDialog > QPushButton { color: red; background-color: white }
// 可以同时选中多个选择器，用逗号隔开
QDialog,
QPushButton { color: red; background-color: white }

// 选中QSpinBox组件的向上，向下按钮
QSpinBox::up-button { image: url(:/images/up.png) }
QSpinBox::down-button { image: url(:/images/down.png) }

// 可以对状态取反，即:!hover
QPushButton:hover { color: red }
QPushButton:!hover { color: red }
// 状态可以串联使用
QPushButton:hover:checked { color: red }
// 子控件也可以使用伪状态
QCheckBox::indicator:checked { image: url(:/images/checked.png) }


// 属性
QPushButton {
	color: blue;	// 改变字体颜色
    color: #ff00ff;
    color: rgb(0, 0, 255);
    color: rgba(0, 0, 255, 0.2);
    color: rgba(0, 0, 255, 255);
    
	background-color: yellow;	// 改变背景颜色
    background-color: #ff00ff;
    background-color: rgb(0, 0, 255);
    background-color: rgba(0, 0, 255, 0.2);
    background-color: rgba(0, 0, 255, 255);
    
    background-image: url(:/image/one.png);
    background-position: center;
    background-repeat: repeat-y;
    background-repeat: no-repeat;
    
    border-top: 20px;
    border-right: 20px;
    border-bottom: 20px;
    border-left: 20px;
    border-top-color: red;
    border-color: red;
    border-radius: 4px;
    border-top-left-radius: 4px;
    border: none;
    border: transparent;
    border: 1px solid;
    border: 1px solid #696969;
    
    padding: 20px;
    
    font: 12pt "Times New Roman";
    font-family: "Times New Roman";
    font-size: 14px;
    text-align: center;
    
    width: 64px;
    height: 64px;
    min-width: 64px;
    min-height: 64px;
}
```
![](/images/qss_1.png)
![](/images/qss_2.png)
## 布局
```cpp
QVBoxLayout *todayLayout = new QVBoxLayout();
QLabel *todayLabel1 = new QLabel("learning");
QLabel *todayLabel2 = new QLabel("fucking");
todayLayout->addWidget(todayLabel1);
todayLayout->addWidget(todayLabel2);
todayLayout->setSpacing(0);
todayLayout->setContentsMargins(0, 0, 0, 0);
todayLayout->setAlignment(Qt::AlignHCenter);
todayLayout->setAlignment(Qt::AlignTop);
todayLayout->setAlignment(todayLabel1, Qt::AlignLeft);
```
## Json处理
```cpp
// 创建一个JSON对象
QJsonObject jsonObj;
jsonObj["name"] = "John";
jsonObj["age"] = 30;

// 创建一个JSON数组
QJsonArray jsonArr;
jsonArr.append("c++");
jsonArr.append("python");

// 将数组添加到JSON对象
jsonObj["programming languages"] = jsonArr;

// 将JSON对象添加到JSON对象
QJsonObject jsonObj2;
obj2["today"] = "sunny";
obj2["tomorrow"] = "rainny";
jsonObj["obj2"] = obj2;

// 将JSON对象转换为JSON文档
QJsonDocument jsonDoc(jsonObj);

// 将JSON文档转换为数组
QByteArray jsonData = jsonDoc.toJson(QJsonDocument::Indented);

// 将Json数据写入文件
QFile file("D:\\json");
if (!file.open(QIODevice::WriteOnly)) {
    qDebug() << "Error";
}
file.write(jsonData);
file.close();

// 从文件读入Json数据
QFile file("D:\\json");
if (!file.open(QIODevice::ReadOnly)) {
    qDebug() << "Error";
}
QByteArray arr = file.readAll();
file.close();
QJsonDocument doc = QJsonDocument::fromJson(arr);
if (jsonDoc.isNull() || !jsonDoc.isObject()) {
    qDebug() << "Failed to parse JSON data.";
}
QJsonObject obj = doc.object();
QJsonValue value = obj["name"];
QString s = value.toString();
qDebug() << s;				// 简便写法qDebug() << QJsonDocument::fromJson(arr).object()["name"].toString();
```
## 网络
```cpp
QNetworkAccessManager *manager = new QNetworkAccessManager(this);
connect(manager, &QNetworkAccessManager::finished, [](QNetworkReply *reply){
    if (reply->error() == QNetworkReply::NoError) {
        QByteArray responseData = reply->readAll();
        qDebug() << "Response:" << responseData;
    } else {
        qDebug() << "Error:" << reply->errorString();
    }
    reply->deleteLater();
});
QUrl url("https://baidu.com");
QNetworkRequest request(url);
manager->get(request);
```
## QFile操作文件
```cpp
QFile file("D:\\qt");
file.open(QIODevice::ReadWrite);
file.write(responseData);
file.close();

file.open(QIODevice::ReadOnly);
QByteArray arr = file.readAll();
```
## 数据库
```cpp
// 示例
void Widget::deleteTask(QString title)
{
    // 将QSqlDatabase和QSqlQuery放在一个单独的作用域，目的是这样调用QSqlDatabase::removeDatabase()时不会出错
    {
        QSqlDatabase todayDB = QSqlDatabase::addDatabase("QSQLITE", "today");
        todayDB.setDatabaseName("record.db");
        if (!todayDB.open()) {
            qDebug() << "Error: unalbe to open database";
            return;
        }

        QSqlQuery todayQuery(todayDB);
        todayQuery.prepare("DELETE FROM today WHERE title = ?");
        todayQuery.addBindValue(title);
        todayQuery.exec();
    }
    QSqlDatabase::removeDatabase("today");
}

// 需要先创建连接在打开后才可使用
QSqlDatabase db = QSqlDatabase::addDatabase("QSQLITE");
db.open();
// mysql
QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
db.setHostName("bigblue");
db.setDatabaseName("flightdb");
db.setUserName("acarlson");
db.setPassword("1uTbSbAs");
bool ok = db.open();
// 如果没有给addDatabase传递第二个参数，则创建的是默认连接，下面创建first, second两条连接
QSqlDatabase firstDB = QSqlDatabase::addDatabase("QMYSQL", "first");
QSqlDatabase secondDB = QSqlDatabase::addDatabase("QMYSQL", "second");
firstDB.open();
secondDB.open();
// 创建连接后可以通过database获得连接，不传递连接名称，则返回默认连接
QSqlDatabase defaultDB = QSqlDatabase::database();
QSqlDatabase firstDB = QSqlDatabase::database("first");
// 关闭连接
firstDB.close() 

// 执行sql语句，如果构造函数没有传递参数，则使用的是默认连接
QSqlQuery query(firstDB);
QSqlQuery query;
query.exec("SELECT name, salary FROM employee WHERE salary > 50000");
while (query.next()) {
    QString name = query.value(0).toString();
    int salary = query.value(1).toInt();
    qDebug() << name << salary;
}
// 插入
query.exec("INSERT INTO employee (id, name, salary) "
           "VALUES (1001, 'Thad Beaumont', 65000)");
// 使用占位符插入
query.prepare("INSERT INTO employee (id, name, salary) "
              "VALUES (?, ?, ?)");
query.addBindValue(1001);
query.addBindValue("Thad Beaumont");
query.addBindValue(65000);
query.exec();
// 更新
query.exec("UPDATE employee SET salary = 70000 WHERE id = 1003");
// 删除
query.exec("DELETE FROM employee WHERE id = 1007");
// QSqlQuery每次提供对结果集一条记录的访问
while (query.next()) {
    QString name = query.value(0).toString();
    int salary = query.value(1).toInt();
    qDebug() << name << salary;
}
```
## QProcess打开其他程序
```cpp
QProcess *myProcess = new QProcess(this);
myProcess->start(program);
```
## QObject定时器
```cpp
// 启动定时器
timerid = this->startTimer(1000);
// 需要重写timerEvent函数
virtual void timerEvent(QTimerEvent *event) override {
    if(event->timerId() == timerId) {				// 用event->timerId()来判断是哪个定时器
        this->killTimer(timerid);					// 用killTimer来关闭定时器
    }
}
```
## QTimer定时器
```cpp
QTimer::singleShot(1000, []{qDebug() << "hello";});		// 只触发一次

QTimer timer;
timer.start();				// 时间到了会发出信号
connect(timer, &QTimer::timeout, this, &Widget::timeroutSlot);
timer.stop();

timer->setTimerType(Qt::CoarseTimer);
timer->setInterval(1000);
timer->setSingleShot(true);
timer->isSingleShot(true);
```
## QElapsedTimer
```cpp
QElapsedTimer timer;
timer.start();
int time = timer.elapsed();
```
## QPixmap保存图片
```cpp
QPixmap pix("D:\\a.png");
```
## QFileDialog打开资源管理器
```cpp
QString fileName = QFileDialog::getOpenFileName(this, "Open File", "C:\\home", "*.txt");
QString fileName = QFileDialog::getSaveFileName(this, "文件", QCoreApplication::applicationFilePath());
fileName.isEmpty();
```
## 设置命令行输出的格式
```cpp
qSetMessagePattern();
// 下面的可能输出[     1.998] D: 
// 对qDebug(), qInfo(), qWarning(), qCritical()和qFatal()生效
// %{time process}代表进程启动后经过的时间
// %{if-debug}D%{endif}如果输出的信息为debug类型，则输出中有D，否则无D
// %{message}代表输出的信息
qSetMessagePattern("[%{time process}] %{if-debug}D%{endif}%{if-info}I%{endif}%{if-warning}W%{endif}%{if-critical}C%{endif}%{if-fatal}F%{endif}: %{message}");
```









## QTcpSocket
```cpp
QTcpSocket socket;
QString ip = "", port = "";
socket->connectToHost(QHostAddress(ip), port.toShort());
// 连接成功，会发出信号
connect(socket, &QTcpSocket::connected, [this](){});
// 连接断开，会发出信号
connect(socket, QTcpSocket::disconnected, [this](){});
```
## QTcpServer
```cpp
QTcpServer server;
server.listen(QHostAddress::AnyIPv4, 8000);
// 客户端发起连接，会发出信号
connect(server, &QTcpServer::newConnection, this, [](){
    // 建立TCP连接
    QTcpSocket *socket = server->nextPendingConnection();
    socket->peerAddress();
    socket->peerPort();
});
```
## QThread线程
```cpp
myThread *t = new myThread;
t->start();			// 需要重写run函数
```
## QMenuBar
```cpp
// 最多一个
QMenuBar *bar = new QMenuBar(this);
setMenuBar(bar);
QMenu *fileMenu = bar->addMenu("文件");
fileMenu->addAction("新建");
fileMenu->addSeparator();
```
## QToolBar
```cpp
// 可以有多个
QToolBar *bar = new QToolBar(this);
addToolBar(bar);
```
## QStatusBar
```cpp
// 最多一个
QStatusBar *bar = new QStatusBar(this);
setStatusBar(bar);
bar->addWidget(label);
```
## QDockWidget
```cpp
// 可以有多个
```
## QDialog对话框
```cpp
// 模态对话框，打开后不能对其他窗口进行操作
QDialog d(this);
d.exec();
// 非模态对话框，打开后也能对其他窗口进行操作
QDialog *d = new QDialog(this);
d.show();
d->setAttribute(Qt::WA_DeleteOnClose);
```
## QMessageBox对话框
```cpp
// 有四种类型，分别为错误，信息，提问，警告
QMessageBox::about(this, "ok");
```
## QColorDialog::getColor()
## QFileDialog::getOpenFileName()
## QFontDialog::getFont()
## QPainter绘图
```cpp
// 需要重写绘图事件 paintEvent
QPainter *p = new QPainter(this);
p.setRenderHint(QPainter::Antialiasing);	// 抗锯齿
p.tranlate(100, 100);					  // 移动坐标系
p.drawPixmap(x, y, QPixmap("D:\\a.png"));
p.drawPixmap(x, y, width, height, QPixmap("D:\\a.png"));
```
## QPen
## QBrush
## QPalette
```cpp
QPalette plet;
plet.setColor(QPalette::Text, Qt::blue);
```
## QFileInfo
## QFont
```cpp
QFont font;
font.setUnderline(true);
font.setItalic(true);
font.setBold(true);
font.setPointSize(20);
```
## QPlainTextEdit
## QProgressBar
```cpp
QProgressBar *bar = new QProgressBar();
bar->setValue(20);
bar->setTextVisible(true);
bar->setInvertedAppearance(true);
bar->setFormat("%p");
```
## QSlider
## QButtonGroup用于单选按钮
## QComboBox
```cpp
QComboBox *box;
box->addItem(QIcon().addFile("a.ico"), "hello");
box->clear();
box->setEditable();
box->currentText();
box->currentData().toString();
```
## 打包成exe
```sh
windeployqt exe文件
需要先把exe移到一个空文件夹下，运行命令后会在当前目录下生成exe启动所需的所有文件
```
