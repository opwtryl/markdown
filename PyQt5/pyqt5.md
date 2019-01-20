# PyQT5 QtWidgets基本概念

简单总结下Qt Widgets Module 的一些基本概念。
[详细资料：Qt Documentation](http://doc.qt.io/qt-5/qtwidgets-index.html)

## 一、Widget

[详细资料：QWidget:Detailed Description](http://doc.qt.io/qt-5/qwidget.html#details)

> All UI elements that Qt provides are either subclasses of QWidget, or are used in connection with a QWidget subclass.

Qt里面的所有UI元素都是Widget。其中按照归属关系分为 parent widget 和 child widget，见下图。

![parent-child-widgets](https://github.com/opwtryl/photos/blob/master/pyqt5/parent-child-widgets.png?raw=true)

使用方式：

```python
from PyQt5.QtWidgets import *
import sys

if __name__ == "__main__":
    app=QApplication(sys.argv) # 创建一个application对象，sys.argv是为了接收命令行参数，比如 python example.py -style windows 会以windows风格显示app。如果不需要命令行参数，也可以去掉 sys.argv。
    p=QWidget()
    p.resize(300,300)
    c=QPushButton(text="按钮",parent=p) # 创建 p 的 child widget，其中QPushButton是QWidget的子类
    c.move(100,100)
    p.show()
    sys.exit(app.exec_())# 下划线是为了和python内置函数exec区分，exec_()启动app直到app退出,返回 0 值。执行 sys.exit(0) 退出python程序。
```

![widget-example](https://github.com/opwtryl/photos/blob/master/pyqt5/widget-example.png?raw=true)

## 二、Window

[详细资料：Qt Documentation:Window and Dialog Widgets](http://doc.qt.io/qt-5/application-windows.html)

>A widget that is not embedded in a parent widget is called a window. (Usually, windows have a frame and a title bar, although it is also possible to create windows without such decoration using suitable window flags). In Qt, QMainWindow and the various subclasses of QDialog are the most common window types.

window就是指那些没有parent 的顶层widget(top-level widget),也可以设置window flag使有parent 的widget成为window。常见的window类型是QMainWindow和QDialog。

### 1.QMainWindow

[详细资料：QMainWindow:Detailed Description](http://doc.qt.io/qt-5/qmainwindow.html#details)

QMainWindow 可以很方便地添加QToolBars(工具栏),QDockWidget,QMenuBar（菜单栏）,QStatusBar（状态栏）,布局见下图。

![QMainWindow](https://github.com/opwtryl/photos/blob/master/pyqt5/mainwindowlayout.png?raw=true)

```python
from PyQt5.QtWidgets import *
from PyQt5.QtCore import Qt
import sys

class Example(QMainWindow):

    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.resize(300,300)
        self.setWindowTitle("Main Window")

        self.statusbar=self.statusBar() # 创建状态栏
        self.statusbar.showMessage("准备就绪")

        self.menubar=self.menuBar() # 创建菜单栏
        self.fileMenu=self.menubar.addMenu("文件")

        self.saveMenu=self.fileMenu.addMenu("保存") #创建带子菜单的菜单
        self.saveAction=self.saveMenu.addAction("保存文件")
        self.saveasAction=self.saveMenu.addAction("另存为")

        self.exitAction=self.fileMenu.addAction("退出")
        self.exitAction.setShortcut("Ctrl+E") # 设置快捷方式
        self.exitAction.setStatusTip("退出程序") # 设置状态栏消息
        self.exitAction.triggered.connect(qApp.quit) # qApp 等价于  QCoreApplication::instance()，可以理解为当前 app。点击 exitAction 触发app退出，这里涉及到 signal and slot 知识。

        self.toolbar=self.addToolBar("工具栏") # 创建工具栏
        self.fileTool=self.toolbar.addAction("新建")

        dock=QDockWidget("浮窗") # dock是可以拖拽的
        dock.setWidget(QTextEdit()) # QTextEdit 是文本输入框
        self.dockwidget=self.addDockWidget(Qt.LeftDockWidgetArea,dock) # 参数分别是 浮窗位置，浮窗对象

        self.centralwidget=self.setCentralWidget(QCalendarWidget())# QCalendarWidget 是日历组件

if __name__ == "__main__":
    app=QApplication(sys.argv)
    ex=Example()
    ex.show()
    sys.exit(app.exec_())
```

效果图如下：

![Main Window](https://github.com/opwtryl/photos/blob/master/pyqt5/mainwindow-example.gif?raw=true)

## 2.QDialog

[详细资料：QDialog:Detailed Description](http://doc.qt.io/qt-5/qdialog.html#details)

>A dialog is always a top-level widget, but if it has a parent, its default location is centered on top of the parent's top-level widget (if it is not top-level itself). It will also share the parent's taskbar entry.

Dialog 本身是top-level widget ，但是也有parent widget。Dialog可以分为，Modal （模态，需要关闭当前窗口才可以操作其他窗口）和 Modeless（非模态）的。

```python
# 只修改了这部分内容，其他的 import,init,main和上面一模一样
def initUI(self):
    self.resize(300,300)
    self.setWindowTitle("Dialog")
    self.menubar=self.menuBar()
    self.fileMenu=self.menubar.addMenu("文件")
    self.openAction=self.fileMenu.addAction("打开")
    self.textedit=QTextEdit("filename",parent=self)
    self.textedit.resize(100,100)
    self.textedit.move(100,100)

    self.filedialog=QFileDialog(self,directory=r"D:\post\python\pyqt5") # directory是窗口打开时默认路径
    self.openAction.triggered.connect(self.openfile)

def openfile(self):
    filename=self.filedialog.getOpenFileName() #打开 filedialog 窗口，选择文件，返回文件路径和文件过滤规则
    self.textedit.setText("filename:"+str(filename))
```

效果图：
![FileDialog](https://github.com/opwtryl/photos/blob/master/pyqt5/filedialog.gif?raw=true)

## 三、Layout

[详细资料：Qt Documetation:Layout Management](http://doc.qt.io/qt-5/layout.html)

>The Qt layout system provides a simple and powerful way of automatically arranging child widgets within a widget to ensure that they make good use of the available space.

PyQT提供各种方便排列 child widget 的layout（布局）类，主要有 Horizontal, Vertical, Grid, Form 和 Stacked Layouts，见下图。

![Horizontal](https://github.com/opwtryl/photos/blob/master/pyqt5/qhboxlayout-with-5-children.png?raw=true)
![Vertical](https://github.com/opwtryl/photos/blob/master/pyqt5/qvboxlayout-with-5-children.png?raw=true)
![Grid](https://github.com/opwtryl/photos/blob/master/pyqt5/qgridlayout-with-5-children.png?raw=true)
![Form](https://github.com/opwtryl/photos/blob/master/pyqt5/qformlayout-with-6-children.png?raw=true)

```python
def initUI(self):
    #  Creating a main window without a central widget is not supported. You must have a central widget even if it is just a placeholder.
    self.setCentralWidget(QWidget()) # MainWindow 必须有一个 central widget，即使只是个空白的 widget
    self.central=self.centralWidget()

    self.setWindowTitle("Layout")
    self.menubar=self.menuBar()
    self.windowMenu=self.menubar.addMenu("菜单")

    hlayout=QHBoxLayout() # 水平布局
    hlayout.addWidget(QPushButton("按钮1"))
    hlayout.addWidget(QPushButton("按钮2"))
    hlayout.addWidget(QPushButton("按钮3"))

    vlayout=QVBoxLayout() # 垂直布局
    vlayout.addWidget(QPushButton("按钮4"))
    vlayout.addWidget(QPushButton("按钮5"))

    flayout=QFormLayout() # 表单布局
    flayout.addRow("账号",QLineEdit())
    flayout.addRow("密码",QLineEdit())

    glayout=QGridLayout() # 网格布局
    glayout.addLayout(hlayout,0,0,1,3) #后面4个参数分别是 行索引，列索引，行跨度，列跨度
    glayout.addLayout(vlayout,1,0,1,1)
    glayout.addLayout(flayout,1,1,2,2)

    page1=QWidget()
    page1.setLayout(glayout)

    page2=QWidget()
    lcd=QLCDNumber() #LCD显示屏
    slider=QSlider(Qt.Horizontal) # 滑块
    slider.valueChanged.connect(lcd.display) # 移动滑块改变LCD显示的数字
    hlayout2=QVBoxLayout()
    hlayout2.addWidget(lcd)
    hlayout2.addWidget(slider)
    page2.setLayout(hlayout2)

    slayout=QStackedLayout() # 堆叠布局
    slayout.addWidget(page1)
    slayout.addWidget(page2)

    pageComboBox=QComboBox() # 用来选择页面
    pageComboBox.addItems(["page1","page2"])
    pageComboBox.activated.connect(slayout.setCurrentIndex)

    mainlayout=QVBoxLayout()
    mainlayout.addWidget(pageComboBox)
    mainlayout.addLayout(slayout)
    self.central.setLayout(mainlayout)
```

效果图：
![Layout](https://github.com/opwtryl/photos/blob/master/pyqt5/layout.gif?raw=true)

## 四、Model/View

[详细资料：Model/View Tutorial](http://doc.qt.io/qt-5/modelview.html)
[详细资料：Model/View Programming](http://doc.qt.io/qt-5/model-view-programming.html)

> Model/View is a technology used to separate data from views in widgets that handle data sets.Standard widgets use data that is part of the widget.View classes operate on external data (the model)

Standardwidget
![standardwidget](https://github.com/opwtryl/photos/blob/master/pyqt5/standardwidget.png?raw=true)

Model/View
![Model/View](https://github.com/opwtryl/photos/blob/master/pyqt5/modelview.png?raw=true)

简单理解，就是Model负责处理数据，View负责展示数据。其中，Model/View 主要有以下3种形式，List Model, Table Model, Tree Model。

![list_table_tree](https://github.com/opwtryl/photos/blob/master/pyqt5/list_table_tree.png?raw=true)

对应的实例分别是:
![LIST](https://github.com/opwtryl/photos/blob/master/pyqt5/listview.png?raw=true)![TABLE](https://github.com/opwtryl/photos/blob/master/pyqt5/tableview.png?raw=true)![TREE](https://github.com/opwtryl/photos/blob/master/pyqt5/treeview.png?raw=true)

```python
# QTableView 和 QTableWidget 例子
class Example(QMainWindow):

    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setCentralWidget(QWidget())
        self.central=self.centralWidget()
        self.resize(550,300)

        self.setWindowTitle("Model/View TableView")
        self.menubar=self.menuBar()
        self.windowMenu=self.menubar.addMenu("菜单")

        self.tableview=QTableView(parent=self.central) # 新建一个视图 (View)
        self.tableview.setModel(myModel()) # 设置视图使用哪个模型 (Model)
        self.tableview.setGeometry(0,0,550,100)
        self.tableview.horizontalHeader().setSectionResizeMode(QHeaderView.ResizeToContents) # 使列宽等于单元格内容长度


        self.tablewidget=QTableWidget(2,3,parent=self.central) # 通过 QTableWidget 创建一个表格
        for row in range(2):
            for column in range(3):
                item=QTableWidgetItem(f"WidgetRow{row+1},WidgetColumn{column+1}")
                self.tablewidget.setItem(row,column,item)
        self.tablewidget.setGeometry(0,150,550,100)
        self.tablewidget.horizontalHeader().setSectionResizeMode(QHeaderView.ResizeToContents)

# QAbstractTableModel 的子类,必须实现 rowCount(),columnCount(),data() 3个方法
class myModel(QAbstractTableModel):

    def __init__(self):
        super().__init__()

    def rowCount(self,parent=QModelIndex()):
        return 2

    def columnCount(self,parent=QModelIndex()):
        return 3

    def data(self,index,role):
        if role==Qt.DisplayRole:
            return f"ViewRow{index.row()+1},ViewColumn{index.column()+1}"

        return QVariant() # QVariant 表示 Qt 常见的数据类型的联合。返回无效数据时推荐使用 QVariant(),而不是 0。
```

效果图：
![TableView](https://github.com/opwtryl/photos/blob/master/pyqt5/Tableview&TableWidget.png?raw=true)

## 五、Graphics View

[详细资料：Graphics View Framework](http://doc.qt.io/qt-5/graphicsview.html)

>Graphics View provides a surface for managing and interacting with a large number of custom-made 2D graphical items, and a view widget for visualizing the items, with support for zooming and rotation.

Graphics View 用于创建二维图形，主要由 Scene,View,Item构成。其中，Item就是Scene里的各种元素，Scene用于管理Item，View用于展示Scene，跟Model/View类似。

```python
from PyQt5.QtGui import *


def initUI(self):
    self.setCentralWidget(QWidget())
    self.central=self.centralWidget()
    self.resize(200,200)

    self.setWindowTitle("Model/View GraphicView")
    self.menubar=self.menuBar()
    self.windowMenu=self.menubar.addMenu("菜单")

    scene=QGraphicsScene() # 创建一个Scene
    scene.addEllipse(0,0,100,100,QPen(Qt.black),QBrush(Qt.blue)) # 添加一个圆形 Item 到Scene
    self.graphView=QGraphicsView(scene,self.central) # 展示Scene
```

效果图：
![graphics view](https://github.com/opwtryl/photos/blob/master/pyqt5/GraphicsView.png?raw=true)

## 六、Styles

[详细资料：Styles and Style Aware Widgets](http://doc.qt.io/qt-5/style-reference.html)

>Styles draw on behalf of widgets and encapsulate the look and feel of a GUI.
Qt Style Sheets are a powerful mechanism that allows you to customize the appearance of widgets, in addition to what is already possible by subclassing QStyle.

在PyQt5中可以通过Style和 Style Sheets设计程序外观，其中Style Sheets的语法类似于CSS。

```python
    def initUI(self):
        self.setCentralWidget(QWidget())
        self.central=self.centralWidget()
        self.resize(200,200)

        self.setWindowTitle("Style & Style Sheet")
        self.menubar=self.menuBar()
        self.windowMenu=self.menubar.addMenu("菜单")

        btn1=QPushButton("按钮1",self.central)

        btn2=QPushButton("按钮2",self.central)
        btn2.move(0,50)
        btn2.setFont(QFont('楷体', 10))

        btn3=QPushButton("按钮3",self.central)
        btn3.move(0,100)
        btn3.setStyleSheet("*{ background-color: red;}")
```

效果图：
![Style & Style Sheet](https://github.com/opwtryl/photos/blob/master/pyqt5/Style%20&%20Style%20Sheet.png?raw=true)

## 七、Signals & Slots

[详细资料：Signals & Slots](http://doc.qt.io/qt-5/signalsandslots.html)

>Signals and slots are used for communication between objects.
A signal is emitted when a particular event occurs.
A slot is a function that is called in response to a particular signal.

![Signals & Slots](https://github.com/opwtryl/photos/blob/master/pyqt5/abstract-connections.png?raw=true)