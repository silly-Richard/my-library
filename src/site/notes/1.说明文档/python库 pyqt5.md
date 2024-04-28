---
{"dg-publish":true,"permalink":"/1.说明文档/python库 pyqt5/","created":"2024-04-28T21:21:43.814+08:00"}
---

# 指南

## 安装pyqt5库

注意：出于后续打包程序的需要，推荐创建虚拟环境，在虚拟环境下安装pyqt5以及编程中所需的其他支撑库。

1.创建激活虚拟环境：使用[[1.说明文档/python库 virtualenv\|virtualenv库]]创建。
2.使用pip安装pyqt5：`pip install pyqt5 -i https://pypi.python.org/simple/`

## pyqt程序的基本框架

包括QApplication对象和窗口对象

```python
import sys
from PyQt5.QtWidgets import QApplication,QWidget

if __name__ == '__main__':
    app = QApplication(sys.argv) # 创建一个程序对象app
    w = QWidget() # 创建一个窗口对象w
    ### 添加控件 ###
    w.show() # 显示窗口 
    app.exec_() # 事件循环状态
```

注意：
- 只要是qt制作的app，必须有且只有一个QApplication对象
- sys.argv当做参数的目的是将运行时的命令参数传递给QApplication对象
- 窗口对象w是其他控件的父对象

## pyqt程序的布局

在Qt中存在四种主要类型布局：QBoxLayout，QGridLayout，QFormLayout，QStackedLayout。
在一个窗口对象w中只能存在一种布局器，但是布局器之间可以嵌套。

### QBoxLayout 盒式布局

QBoxLayout一般使用它的两个子类`QHBoxLayout`(水平布局)和`QVBoxLayout`(垂直布局)

**垂直布局**示意：（水平布局类似）
```python
import sys
from PyQt5.QtWidgets import QApplication,QWidget,QVBoxLayout,QPushButton

## 创建Mywindow类，继承自QWidget
class Mywindow(QWidget):

    def __init__(self):
        super().__init__() # 继承父类，父类初始化

        self.resize(300,300)
        self.setWindowTitle("垂直布局")
        layout = QVBoxLayout() ## 创建布局器

        layout.addStretch(1) ## 在按钮1前方留一个单位空白

        btn_1 = QPushButton("no1") ## 创建btn_1，不需要申明父类
        layout.addWidget(btn_1) ## 将btn_1应用在layout上
        layout.addStretch(1)

        btn_2 = QPushButton("no2")
        layout.addWidget(btn_2)
        layout.addStretch(1)

        btn_3 = QPushButton("no3")
        layout.addWidget(btn_3)
        layout.addStretch(1)

        self.setLayout(layout) ## 将Mywindow类的layout属性修改为创建的类型

if __name__ == "__main__":
    app = QApplication(sys.argv)
    w = Mywindow() ## 创建Mywindow类的一个实例w
    w.show() ## 显示实例
    app.exec()

```


### 布局嵌套
**嵌套**垂直布局和水平布局：

```python
import sys
from PyQt5.QtWidgets import QApplication,QWidget,QVBoxLayout,QHBoxLayout,QGroupBox,QRadioButton


## 创建Mywindow类，继承自QWidget
class Mywindow(QWidget):
    def __init__(self):
        ## 继承父类 ##
        super().__init__()
        ## 将主要布局设置放置在init_ui()函数中,只留下对窗口w的设置 ##
        self.init_ui() # init_ui()初始化
        self.resize(300,300)
        self.setWindowTitle("调查")

    def init_ui(self):
        ## 嵌套式布局，最外层是垂直布局 ##
        container = QVBoxLayout()

        ### 创建第一个组,使用QGroupBox分控框组件 ###
        hobby_box = QGroupBox("爱好")
        #### hobby_box自己存在布局特点，使用布局器创建 ####
        v_layout = QVBoxLayout()
        btn1 = QRadioButton("篮球")
        btn2 = QRadioButton("羽毛球")
        btn3 = QRadioButton("足球")
        #### 将控件添加至v_layout中 ####
        v_layout.addWidget(btn1)
        v_layout.addWidget(btn2)
        v_layout.addWidget(btn3)
        #### 将hobby_box的layout属性设置为v_layout ####
        hobby_box.setLayout(v_layout)


        ### 创建第二个组 ###
        gender_box = QGroupBox("性别")
        h_layout = QHBoxLayout()
        btn4 = QRadioButton("男")
        btn5 = QRadioButton("女")
        h_layout.addWidget(btn4)
        h_layout.addWidget(btn5)
        gender_box.setLayout(h_layout)

        ## 一个窗口w只能存在一种布局，但是布局之间可以通过添加嵌套 ##
        container.addWidget(hobby_box)
        container.addWidget(gender_box)

        self.setLayout(container)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    w = Mywindow()
    w.show()
    app.exec()
```

### QGridLayout网格布局

网格布局也被称为九宫格布局，以计算器为例：

```python
import sys
from PyQt5.QtWidgets import QApplication,QWidget,QVBoxLayout,QGridLayout,QLineEdit,QPushButton


class Mywindow(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()
        self.setWindowTitle("计算器")
    
    def init_ui(self):

        # 最外层垂直布局
        layout = QVBoxLayout() # 创建全局布局器layout，纵向布局
        
        edit = QLineEdit()
        edit.setPlaceholderText("请输入内容")
        layout.addWidget(edit) # 将edit控件添加至布局器中

        grid = QGridLayout() # 创建网格布局器实例grid

        # 将按钮字符以字典形式组织
        # 也可以使用array，使用列表嵌套变为二维数组
        # 只要能传入都可以
        data = {
            0:["7","8","9","+","("],
            1:["4","5","6","-",")"],
            2:["1","2","3","*","<-"],
            3:["0",".","=","/","C"]
        }

		## 将计算器按钮数字循环输入 ##
        for line_number,line_data in data.items():
            for col_number,data in enumerate(line_data):
                btn = QPushButton(data)
                grid.addWidget(btn,line_number,col_number)
        
        layout.addLayout(grid) # 将grid嵌套进layout中
        self.setLayout(layout) # 设置全局布局属性为layout


if __name__ == "__main__":
    app = QApplication(sys.argv)
    w = Mywindow()
    w.show()
    app.exec()
```

### QFormLayout表单布局

一般用于提交form表单。

```python
import sys
from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import QApplication,QWidget,QVBoxLayout,QFormLayout,QLineEdit,QPushButton

class Mywindow(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()

    def init_ui(self):
        layout = QVBoxLayout()
        form_layout = QFormLayout()

        edit = QLineEdit()
        edit.setPlaceholderText("请输入姓名")
        form_layout.addRow("姓名",edit) # 以行方式添加，行的名称和控件

        edit2 = QLineEdit()
        edit2.setPlaceholderText("请输入年龄")
        form_layout.addRow("年龄",edit2)

        layout.addLayout(form_layout)

        btn = QPushButton("确认")
        layout.addWidget(btn,alignment=Qt.AlignRight) # 让按钮右对齐

        self.setLayout(layout)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    w = Mywindow()
    w.show()
    app.exec()
```

### QStackedLayout抽屉布局

提供了多个页面的切换，但是一次只能显示一个页面。使用了槽函数切换。

```python
import sys
from PyQt5.QtWidgets import QApplication,QWidget,QStackedLayout,QLabel,QPushButton,QVBoxLayout

class Window1(QWidget):
    def __init__(self):
        super().__init__()
        QLabel("抽屉1",self)
        self.setStyleSheet("background-color:green") # 背景色设置是css设置样式

class Window2(QWidget):
    def __init__(self):
        super().__init__()
        QLabel("抽屉2",self)
        self.setStyleSheet("background-color:red")


class Mywindow(QWidget):
    def __init__(self):
        super().__init__()
        self.create_qstacked_layout()
        self.init_ui()

    # 创建抽屉布局函数 #
    def create_qstacked_layout(self):
        # 创建全局变量 #
        self.qstacked_layout = QStackedLayout()
        # 创建窗口实例 #
        win_1 = Window1()
        win_2 = Window2()
        self.qstacked_layout.addWidget(win_1)
        self.qstacked_layout.addWidget(win_2)

    def init_ui(self):
        self.setFixedSize(300,270)

        # 创建整体垂直布局器 #
        layout = QVBoxLayout()


        # 创建一个单独的窗口嵌入create_qstacked_layout(),不然就是两个窗口 #
        win = QWidget()
        # 调用变量qstacked_layout #
        win.setLayout(self.qstacked_layout)
        win.setStyleSheet("background-color:grey")


        btn1 = QPushButton("确认1")
        btn2 = QPushButton("确认2")
        btn1.clicked.connect(self.btn1_clicked)
        btn2.clicked.connect(self.btn2_clicked)

        layout.addWidget(win)
        layout.addWidget(btn1)
        layout.addWidget(btn2)
        self.setLayout(layout)
    
    def btn1_clicked(self):
        self.qstacked_layout.setCurrentIndex(0)

    def btn2_clicked(self):
        self.qstacked_layout.setCurrentIndex(1)   


if __name__ == "__main__":
    app = QApplication(sys.argv)
    w = Mywindow()
    w.show()
    app.exec()
```


## 信号与槽

信号（signal）指事件。当程序的状态发生改变，或者发生了某种事件，就会产生一个信号。
槽（slot）指代码。当信号被系统捕捉并以此激活一段代码，与信号绑定的函数被称为槽函数。

基本格式：

```text
对象.信号.connect(槽函数)
```

---

# 问题

## 配置PyQt5-tools没有designer.exe文件

推荐阅读[^1]
使用pip安装PyQt5-tools后，根据网上教程，其中的`designer.exe`文件应当位于`C:\Users\Dell\AppData\Local\Programs\Python\Python38\Lib\site-packages\pyqt5_toolsQt\bin\designer.exe` 。但是该文件实际上并不在这里。

`designer.exe`文件位于`\site-packages\qt5_applications\Qt\bin`目录下

## 检查pyqt5版本

```python
from PyQt5.QtCore import *

print(QT_VERSION_STR)
```


# 查表

## 基础框架
```python
import sys
from PyQt5.QtWidgets import QApplication,QWidget

if __name__ == '__main__':
    app = QApplication(sys.argv) # 创建一个程序对象app
    w = QWidget() # 创建一个窗口对象w
    ### 添加控件 ###
    w.show() # 显示窗口 
    app.exec_() # 事件循环状态
```

## 窗口w/QWidget

**属性设置**
```python
w = QWidget()
w.setWindowTitle("名称") # 设置窗口名称
w.setFixedSize(300,720) # 设置固定窗口大小
w.resize(x, y) # 设置窗口大小。x为宽(weight)，y为高(height)
w.move(x, y) # 将窗口的左上角设置在以屏幕左上角为参照系的相对位置

from PyQt5.QtGui import QIcon # 设置窗口图标需要额外导入
w.setWindowIcon(QIcon('pic_file')) # 设置窗口图标，图片地址可以是相对的
```

**窗口示显在屏幕中央**
```python
from PyQt5.QtWidgets import QDesktopWidget # 首先额外导入QDesktopWidget

center_pointer = QDesktopWidget().availableGeometry().center() # 获得屏幕中央的坐标
x = center_pointer.x()
y = center_pointer.y()
old_x, old_y, width, height = w.frameGeometry().getRect() # 获得窗口w的左上角xy坐标，宽度和高度数据
w.move(x-width/2, y-height/2) # 屏幕中央的坐标减去半个高和宽长度，就可以使窗口w中央放置在屏幕中央
```

## 布局器Layout
```python
from PyQt5.QtWidgets import QHBoxLayout,QVBoxLayout,QGridLayout
'''
Qt中主要存在四种布局：
QBoxLayout:包括QHBoxLayout,QVBoxLayout
QGridLayout
'''

layout_1 = QVBoxLayout() # 创建纵向布局器实例
layout_2 = QHBoxLayout() # 创建横向布局器实例

layout_1.addWidget() # 将控件添加至布局器中
layout_1.addLayout(layout_2) # 布局器嵌套，将布局器2嵌套进布局器1中

```


## 按钮btn/QPushButton

```python
from PyQt5.QtWidgets import QPushButton # 需要额外导入QPushButton

## 创建btn对象并设置父对象
btn = QPushButton("按钮",w) # 创建button对象，在括号内传入窗口w，使窗口w成为btn父对象

btn = QPushButton("按钮")
btn.setParent(w) # 效果相同，先创建btn对象，再使用setParent()设置btn父对象

## 设置btn属性
btn.setGeometry(x,y,weight,height) # 设置btn的绝对位置，以窗口w的左上角为参照，x和y指btn左上角坐标（w左上角为坐标原点），weight指btn的宽,height指btn的高

```

## 纯文本label/QLabel

```python
from PyQt5.QtWidgets import QLabel # 需要额外导入QLabel

## 创建label对象
label = QLabel('文字内容', w) # 创建label对象，并设置其父类对象：窗口w

## 设置label属性
label.setGeometry(x1,y1,x2,y2) # 设置label的绝对位置
label.setStyleSheet("background-color:green") # 设置label文字背景色
label.setStyleSheet("color:green") # 设置label文字颜色
```

## 文本输入框edit/QLineEdit

```python
from PyQt5.QtWidgets import QLineEdit # 需要额外导入QLineEdit

## 创建edit对象
edit = QLineEdit(w) # 设置父类对象：窗口w

## 设置edit属性
edit.setPlaceholderText("请输入内容") # 当文本框为空白时默认显示内容
edit.setGeometry(x1,y1,x2,y2) # 设置edit的绝对位置

```

## 分控框组件group/QGroupBox

```python
from PyQt5.QtWidgets import QGroupBox # 需要额外导入QGroupBox

## 创建group对象
group = QGroupBox("名称")

```

[^1]: [配置PyQt5-tools没有designer.exe文件\_下的anaconda里面为什么没有designer.exe-CSDN博客](https://blog.csdn.net/qq_37713521/article/details/110395778)