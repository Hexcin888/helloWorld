# Qt学习笔记

## 注意事项

### 1.1 创建文件

![image-20200611165307035](E:\Cxx的文件夹\笔记\Qt学习笔记\Qt学习笔记.assets\image-20200611165307035.png)

 

+ 工程文件中（pro）不清楚意义的情况下可以完全不懂

```C++
QT  += core gui //Qt包含的模块，里面的模块都有很多类  ， 需要加模块你再加，可以进到类帮助文档里面看要不要加。

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets  //大于4版本就包含widgets模块

CONFIG += c++11 //使用C++11标准

DEFINES += QT_DEPRECATED_WARNINGS

TARGET = XXXXX //修改这个程序的名字

TEMPLATE = app //模板变量，告诉qmake生成哪种makefile

SOURCES += \    //帮你生成几个源文件，可以仿照格式加
    main.cpp \
    mywidget.cpp

HEADERS += \    //头文件——和源文件匹配
    mywidget.h
```





+ main.cpp

```C++
#include "mywidget.h"
#include <QApplication>//这是个包含应用程序类的头文件

int main(int argc, char *argv[])//argc是命令行变量的数量 ， argv是命令行的数组
{
    //实例化出的一个应用程序对象，在Qt中只能有一个，而且必须有一个
    QApplication a(argc, argv);
    //窗口对象 myWidget父类-> QWidget
    myWidget w;
    //窗口对象 默认不会显示，必须要调用show方法显示窗口
    w.show();
    //让应用程序对象进入消息循环,可以理解成原来的system pause
    //点右上角的×才退出，而且这后面的代码不会执行了
    return a.exec();
}
```



### 1.2 命名规范

+ 类名：首字母大写，单词和单词之间首字母大写
+ 函数名、变量名：首字母可以小写，单词和单词之间首字母大写

+++

| 功能                 | 快捷键          |
| -------------------- | --------------- |
| 注释                 | Ctrl L          |
| 运行                 | Ctrl R          |
| 编译                 | Ctrl B          |
| 字体缩放             | Ctrl 滚轮       |
| 查找关键             | Ctrl F          |
| 整行移动             | Ctrl Shift ↑或↓ |
| 同名的h和cpp之间切换 | F4              |



## 基本控件

### 2.1 QPushButton

![image-20200612211839754](E:\Cxx的文件夹\笔记\Qt学习笔记\Qt学习笔记.assets\image-20200612211839754.png)



### 2.2 信号和槽 Signal and slots

false code:

**connect(信号的发送者，signal，接收者，信号处理（槽口）)**

**优点**：松散耦合，信号发送端和接收端本身是没有关联的，通过connect连接，耦合在一起



#### 基本信号槽

**常用信号**：

1. clicked(bool checked = =false)
2. pressed()
3. released()
4. toggled(bool checked)



对于发送者和接收者的参数，在connect函数中以地址的方式呈现



#### 自定义的信号和槽

emit 激活自定义信号（信号是类内一个成员函数，用类的一个数据成员调用）

```C++
void Widgt::classIsOver()
{
    emit zt->hungry(); 
}
```



1. 可以是public slots，也可以是一个全局函数
2. 返回值是void，信号只需要声明不需要实现，槽既需要声明也需要实现
3. 可以有参数，支持重载



#### 重载自定义的信号和槽

![image-20200613210823151](E:\Cxx的文件夹\笔记\Qt学习笔记\Qt学习笔记.assets\image-20200613210823151.png)

如图所示，通过参数的不同类型，实现了信号和槽的重载



**注意**：当需要传入一个信号时，可能产生二义性，此时需要指明信号的**地址**，对于信号和槽，本质上都是**函数**，因此采用函数指针来做处理

```C++
void（Student::*studentSlot)(QString)=&Student::treat;
```

**注意**：

+ 必须写明函数指针是在哪个作用域

+ 第二个指明指明函数的参数的类型
+ 第三个是原函数的地址

#### 信号连接信号

![image-20200613211313952](E:\Cxx的文件夹\笔记\Qt学习笔记\Qt学习笔记.assets\image-20200613211313952.png)

用一个QButton连接上了另外一个信号

#### 拓展

1. 可以建立多个信号来实现多条件触发
2. 断开信号，用disconnect函数
3. 一个信号是可以连接多个槽函数的
4. 信号和槽参数必须一一对应，信号的参数可以多于槽，不对应的将会被舍弃
5. void类型的槽都可以被连接因为参数个数为0











































## 零碎的知识点

qDebug出来的是一个string类型的对象，会带有一个""，如果要解决的话，把它转化为char *

```C++
name.toUtf8.data()
```

用toUtf8 转化为QByteArray类型

再用data函数转化为char *（实际上，帮助文档中可以查询返回类型）



**信号和槽函数有重载版本的话，要用函数指针接收，来确认类型**



询问是否要确认关闭窗口

```c
void QWidget::closeEvent(QCloseEvent *event)
{
    int closeApp = QMessageBox::question(this, "Note", "Do you really want to exit?");
    if(closeApp == QMessageBox::Yes)
    {
        //如果有多线程操作，要记得关闭
        event->accept();  //关闭
    }
    else
    {
        event->ignore();  //不关闭
    }
}

```

