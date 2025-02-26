- [示例](#示例)
- [信号与槽](#信号与槽)
- [Qt中的枚举](#Qt中的枚举)
- [字体](#字体)

## 示例
```python
from PySide6.QtWidgets import (
    QPushButton,
    QApplication,
    QMainWindow,
)

class MyWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        button = QPushButton("hello", self)    

if __name__ == "__main__":
    app = QApplication([])
    window = MyWindow()
    window.show()
    app.exec()
```
## 信号与槽
```python
# 信号：使用Signal类定义，通常在类的顶部定义。
# 槽：可以是任何方法，使用@Slot装饰器标记（可选）。
# 连接：使用connect()方法将信号连接到槽。
# 发射信号：使用 emit()方法发射信号。

# 连接信号
from PySide6.QtCore import Qt, Slot
from PySide6.QtWidgets import QWidget, QApplication, QPushButton

@Slot()
def say():
    print('hi')

app = QApplication()
button = QPushButton('click')
button.clicked.connect(say)
button.show()
app.exec()


# 释放信号
class A:
	sig = Signal(str)
    def say():
		self.sig.emit("hello")
		self.sig.connect(函数名)
```
## Qt中的枚举
```python
from PySide6.QtCore import Qt
Qt.AlignmentFlag.AlignCenter
```
## 字体
```python
label = QLabel('d')
font = QFont()
font.setFamily('微软雅黑')
font.setPixelSize(50)
label.setFont(font)
```

