---
layout:         post
title:          Python-逆向学习问题总结
category:       course
description:    逆向法学习 Python，一网打尽那些关于 Python 你需要能回答出的问题
---


## 问题分类

- 核心知识
    - **基础**: 开始，开发环境搭建，基本对象和变量，流程控制，文件，函数式编程
    - **进阶**: 模块和包，类和实例，面向对象和设计模式，异常处理，正则表达式，装饰器，生成器
- 常见应用
    - **系统运维**: Bash，Python 与 Bash 联合编程，文件和目录，系统监控，网络协议，并行处理
    - **DevOps**: 版本控制，测试，日志处理，部署，自动化管理，容器，APM
    - **Web开发**: ORM模型，Web架构和接口，MVC，WebService，Flask，Django，微信
    - **数据采集**: 爬虫，Scrapy 框架
    - **数据挖掘**: 基本概念，常见类库，案例分析
- 其它应用
    - **云计算OpenStack**: 架构，安装，使用，排错
    - **PaaS云平台**: GAE, SAE
    - **Office文档处理**: Word，Excel
    - **Python-C**: Python 调用 C-Lib，C 实现 Python-Module
    - **PVM内存分析和调试**：pdb，dump，windbg，gdb
    - **IoT**: 树莓板，GPIO，Azure IoT DevKit
    - **MineCraft**: MineCraft, 绘制三维图形，捕捉玩家位置，虚拟连接现实
    - **GUI**: TK
    - **区块链**: BitCoin, IoT via BitCoin

---------------------------------------
---------------------------------------


## 核心知识

### 基础

1. [开始] **Python 在编程语言世界中的处于什么样的位置？**[TIOBE](https://tiobe.com/tiobe-index/)

1. [开始] **Python 有什么样的特质和设计哲学？**简单优于繁复，明确优于晦涩，解决一个问题只用一种方法。

1. [开始] **作为一个初入门者，我们应该怎样学习编程？**[如何成为一名黑客-Eric-S-Raymond](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/%E5%A6%82%E4%BD%95%E6%88%90%E4%B8%BA%E4%B8%80%E5%90%8D%E9%BB%91%E5%AE%A2-Eric-S-Raymond.pdf)

1. [开始] **应该选择 Python2 还是 Python3 开始学习，以及进行开发？**[参考](https://wiki.python.org/moin/Python2orPython3)，用 Python3

1. [开始] 如果你已经掌握了 Python2 或者 Python3 其中一门，[Python2和3有什么区别](https://docs.python.org/3/whatsnew/3.0.html)？[如何迁移](https://docs.python.org/2/library/2to3.html)？

1. [开始] Python2 和 Python3，或者多个 Python3 版本如何在一个系统中共存？Native / Virtualenv / Docker

1. [开始] **有哪些优秀的 Python 学习资源可以参考？** [Python学习手册5th](https://book.douban.com/subject/30364619/)，[Python3标准库](https://book.douban.com/subject/30346181/)，[Python 官网教程](https://docs.python.org/zh-cn/3/tutorial/)

1. [环境] **在 Windows 下如何搭建 Python 开发环境？**[单独安装 Python](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/Installation-Python.md)，[一体化安装 Anaconda](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/Installation-Anaconda.md)，[IDE-Eclipse-Pydev](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/Installation-Eclipse-Pydev.md)，[IDE-PyCharm](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/Installation-PyCharm.md)

1. [环境] 在 Mac 下如何搭建 Python 开发环境？brew

1. [环境] 在 Linux 下如何搭建 Python 开发环境？apt / yum

1. [环境] 如何使用 IDLE 进行开发和调试？自动补全用 tab。

1. [环境] 如何使用 `Eclipse+Pydev` 运行和调试 Python 程序？**

1. [环境] 如何使用 Pycharm 进行开发和调试？[Download Community](https://www.jetbrains.com/pycharm/download/#section=windows)

1. [环境] **如何使用 VSCode 进行开发和调试？**[Download](https://code.visualstudio.com/Download)，[环境配置](https://code.visualstudio.com/docs/python/python-tutorial)

1. [环境] 如何使用 VisualStudio 进行开发和调试？[Download](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)

1. [环境] 还有什么其它的Python调试套件？[Notepad++](http://www.cnblogs.com/zhcncn/p/3969419.html)，[Sublime](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/Installation-Sublime.md)

1. [环境] **如何查阅Python帮助文档？** pydoc, help, chm

1. [环境] **Python2 代码中如何包含中文？**源文件存成utf-8，文件头：`# -*- coding: utf-8 -*`，python3 已经不需要这么做。

1. [数字] **如何实现取整运算？截余取整、四舍五入、向上取整、向下取整。**

1. [数字] **整数的除法运算和整除运算是如何实现的？**怎么写兼容代码？

1. [数字] **Python 如何处理进制转换（整数与字符串的互相转换）？**0b/0/0x，bin/ocx/hex，%o/%x

1. [数字] **应如何判断两个浮点数是否相等？**原理

1. [数字] **浮点数精度控制(`round`)和精度显示(`%.2f`)应用场景？如何编码？**

1. [数字] 在 Python 中我们应该如何同时取商和取余？

1. [数字] 如何对一组数据求和？求最大值？求最小值？

1. [数字] **如何产生随机数？`random/randint/choice`**

1. [数字] Python 中如何进行位运算？置位/清零/2 的次方数/木匠 7 日分金/读心术/作弊问题/小白鼠试药/盒子装球

1. [数字] **如何理解 Python 中变量和对象的存储模型？id**

1. [数字] 如何理解 Python 语言的三个定语：动态语言，动态类型语言，强类型语言？

1. [字符] 如何编码单行、多行和分行输入的字符串？

1. [字符] 我们应该用单引号还是双引号？

1. [字符] **什么是原始字符串？**

1. [字符] **如何处理 Unicode 字符串和编解码问题？**`u'我们'.encode('utf-8').decode('utf-8')``

1. [字符] **如何实现字符串的乘法和加法？**

1. [字符] **如何操作字符串切片？负 index、左开右闭、None、负步长**

1. [字符] **如何实现子字符串的替换、查找？**

1. [字符] 如何实现字符串的大小写转换，以及将首字母、每个单词首字母变为大写？

1. [字符] **如何实现字符串的切割和粘合？**

1. [字符] **如何移除字符串两端的空白？**

1. [字符] **如何获取字符串的长度？**

1. [字符] 如何实现 ASCII 码与字符的相互转换？

1. [字符] **如何格式化字符串？`%`，`format`**

1. [字符] 对象可以有哪些属性？字段属性和方法属性分别有哪些例子？

1. [字符] **什么是反射和自省机制？`__dict__`, `dir`**

1. [序列] 如何初始化一个元组 Tuple 对象？

1. [序列] **元组/列表对象在 PVM 中的存储模型是怎样的？**

1. [序列] 基于元组的赋值语法有哪些常见的应用场景？

1. [序列] **序列的通用运算？`in`, `for`, `+/*`, 切片, `len/min/max/sorted`**

1. [序列] **如何理解列表对象的可变性与元组对象的不可变性？**

1. [序列] 生成器对象和列表对象的区别是什么？

1. [序列] **列表对象的常见运算有哪些？元素的添加，访问，排序，反序，移除，修改，遍历**

1. [序列] **什么是列表解析？**

1. [序列] **重映射，浅拷贝，深拷贝的区别是什么？**`[[0]]*5`

1. [散列] 序列和散列的区别是什么？

1. [散列] 如何利用集合对序列元素去重？

1. [散列] **集合运算有哪些？**

1. [散列] 如何初始化一个字典？

1. [散列] **集合/字典的存储模型是怎样的？**

1. [散列] **字典的常见运算有哪些？元素的添加，访问，移除，修改，遍历，排序，存在判断**

1. [散列] **什么是唱票算法？它适用于什么场合？Counter 类能实现什么功能？**

1. [散列] 除了列表解析外，还有什么类似的语法？生成器表达式，集合解析，字典解析

1. [流程] 什么是连续赋值语法？

1. [流程] **什么是连续比较语法？**

1. [流程] 序列的比较逻辑是怎样的？

1. [流程] **什么是短路效应？**

1. [流程] 分支结构的语法是怎样的？

1. [流程] 三元运算符的语法是怎样的？

1. [流程] **Python 中的循环结构语法是怎样的？else 部分在什么时候会被运行到？**

1. [流程] **对序列和散列进行循环遍历应如何编码？**

1. [文件] 怎样处理命令行参数？

1. [文件] 文件对象的存储模型是怎样的？

1. [文件] **如何实现文件的读、写、flush 和偏移量操作？**

1. [文件] 什么是基本输入/基本输出/基本错误？如何实现它们的重定向？

1. [文件] 如何序列化一个Python对象？

1. [文件] **如何以编码方式读取一个文件，比如 utf8 ？**

1. [文件] 如何实现二进制文件的读写？

1. [函数] **如何定义和调用一个函数？**

1. [函数] 函数对象的存储模型是怎样的？

1. [函数] **什么是LEGB规则？有哪些陷阱？`a += 3`**

1. [函数] **默认参数的陷阱有哪些？`time.time()`, `[]`**

1. [函数] 如何在函数内使用 Global 变量？

1. [函数] **Python 中的实参传递有哪些特殊的语法？**

1. [函数] **Python 中的形参传递有哪些特殊的语法？**

1. [函数] 函数传参和 Return 返回值时实际发生了什么？重映射？浅拷贝？深拷贝？

1. [函数] **Lambda 函数的概念和语法如何？**

1. [函数] 函数调用时，临时变量是如何进栈和出栈的？栈是 Per 线程还是 Per 进程的？

1. [函数] 如何理解和编写递归函数？优势：语义明确；劣势：性能隐患。斐波那契数列，递归，递推。

1. [函数] 回调函数的语法和使用场合是怎样的？

1. [函数] **什么是高阶函数？**

1. [函数] 闭包 closure 是什么？如何用闭包实现加法器？有状态的函数。FP 与 OOP。

1. [函数] **如何使用高阶函数？Map/Filter/Reduce/Sort/Max/偏函数**，`from functools import reduce`

### 进阶

1. [模块] 模块对象的定义和使用是怎样的？

1. [模块] 顶层脚本和模块的区别是什么？

1. [模块] **import 一个模块是实际发生了什么事？**

1. [模块] **import 语法的变化和最佳实践是怎样的（避免`from x import *`，`import module`）？**

1. [模块] 为什么 import 应该以 module 为单位？

1. [模块] reload 的应用场景是什么？`from imp import reload`

1. [模块] **为什么需要这样的语法？`if __name__ == '__main__': `**

1. [模块] 针对模块对象的反射语法是什么？`__import__`

1. [对象] **如何定义和实例化一个类？**

1. [对象] **类与实例的存储模型是怎样的？类的属性和实例的属性存在怎样的关系？**

1. [对象] **类中的字段属性为什么不推荐使用可变对象？**

1. [对象] 什么是伪私有属性？

1. [对象] 什么是属性 property？

1. [对象] 什么是静态 static 方法与类 class 方法？

1. [对象] 什么是钩子方法与运算符重载？

1. [对象] **常见的重载方法有哪些？`__init__`, `__str__`, `__call__`, `__getattr__`, `__len__`**

1. [对象] 如何用类来实现闭包？`__init__`, `__call__`

1. [对象] 如何约束类，使其实例不能随意添加属性？`__slot__`

1. [对象] Python 的继承是单继承还是多继承？继承的搜索策略是深度优先还是广度优先？

1. [对象] **组合和继承各自有什么优缺点？如何用组合和继承分别实现 Name 类？**

1. [对象] **Python 如何实现一个符合开放封闭原则的简单工厂模式？**

1. [对象] 设计模式的基本原则和六大原则是什么？23 种设计模式分为哪 3 类，每个设计模式用于什么场景？参考：[设计模式摘录](http://blog.wuwenxiang.net/Design-Pattern)

1. [异常] **异常处理的语法是怎样的？else 部分在什么时候会被执行？**

1. [异常] 异常处理时的执行流程是怎样的？

1. [异常] 编写异常处理逻辑时，应如何避免过度捕捉异常？

1. [异常] 在什么情况下我们需要自定义异常？应如何编码？

1. [异常] 什么是环境管理协议？

1. [异常] **如何使用 With 语法？**

1. [异常] 如何实现一个环境管理器？

1. [正则] **Python 中使用正则表达式的语法是怎样的？**

1. [正则] **正则表达式中的符号有哪些？**

1. [正则] 什么是非贪心匹配？

1. [正则] 什么是标记匹配？

1. [正则] 如何获取匹配到的字符串？

1. [正则] 如何替换匹配到的字符串？sub，如何通过标记匹配进行替换？

1. [正则] 如何使用 Findall？

1. [正则] 如何按正则表达式切割字符串？

1. [正则] **编写正则表达式时有哪些注意事项？`r`, 如何化繁为简？**

1. [装饰] 什么是装饰器模式？

1. [装饰] **什么是 Python 中的装饰器语法？如何使用？**

1. [装饰] 装饰器语法有哪些语法变化？装饰器类，多重装饰，装饰器参数

1. [生成] **什么是迭代环境，迭代器和迭代协议？Python2和Python3有何区别？`next`和`__next__`**

1. [生成] **如何编写生成器函数和生成器表达式？**

1. [生成] **生成器函数的执行逻辑是怎样的？**

1. [生成] 如何生成无限多的斐波那契数列？`for i in Fib(): print(i)`

1. [生成] 什么是扩展生成器协议？

1. [生成] 什么是协程Co-Routine？有哪些应用场景？

1. [生成] 有哪些常见的协程类库？如何使用？

## 常见应用

### Python 的系统和进程管理中的应用

1. [Bash] Bash 编程有哪些优秀的参考书和资料？

1. [Bash] **Bash 变量的定义和使用方法是怎样的？如何在循环中定义变量？如何重新定义变量？**

1. [Bash] 单引号和双引号对 Bash 字符串操作有什么区别？

1. [Bash] **常见的 Bash 字符串操作有哪些？求长度，提取子串，查找子串位置**

1. [Bash] **Bash 数组操作有哪些？赋值，求长度，引用数组，连接数组，遍历数组**

1. [Bash] **Bash 分支结构的写法是怎样的？**

1. [Bash] **Bash 循环的写法是怎样的？for/while**

1. [Bash] Bash 中的单行注释和多行注释的写法分别是怎样的？

1. [Bash] Bash 中文本处理是怎样的？grep, awk, sed, tr

1. [Bash] Bash 中的管道处理是怎样的？xargs

1. [Bash] Bash 中的数学运算是如何操作的？expr

1. [联合] Python 如何调用 Bash？os.system, subprocess

1. [联合] Bash 如何调用 Python？

1. [联合] sys 模块主要用于处理什么问题？常用方法有哪些？

1. [联合] os 模块主要用于处理什么问题？常用方法有哪些？

1. [目录] **如何遍历一个目录？**

1. [目录] **如何创建和使用临时文件，临时目录？**

1. [目录] shutil模块有哪些常用方法？copy/copy2/copytree/rmtree/move

1. [系统] **psutil 模块如何监控 CPU/Memory/Network/Disk 等资源？**

1. [网络] **IPy 模块的使用范围和使用方法是怎样的？**

1. [网络] Python 如何处理常见的网络协议？Telnet/FTP/Socket/Http/LDAP/SSH/SFTP/SMTP/POP3/IMAP

1. [网络] Twisted 框架的底层实现是怎样的？

1. [网络] Twisted 框架的基本使用方法是怎样的？在Windows 上如何安装 Python3-Twist 框架？[知乎](https://www.zhihu.com/question/52281800), [Python Extension Packages for Windows](https://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted)

1. [并行] **如何处理子进程和管道？subprocess**

1. [并行] **如何编写多线程应用？**

1. [并行] **线程 join 操作有什么作用？**

1. [并行] **什么是后台线程？Daemon**

1. [并行] **什么是线程竞争？**

1. [并行] **如何实现线程同步？锁、信号量**

1. [并行] 什么是全局解释器锁 GIL？

1. [并行] 哪些 Python 内置对象是线程安全的？哪些不是？

1. [并行] 如何像管理线程一样管理进程？

1. [并行] **如何开启和使用进程池？**

1. [并行] **如何开启和使用线程池？**

1. [并行] **Async 异步语法应如何使用？**

1. [并行] 异步的底层实现是怎样的？使用时有哪些陷阱？

### DevOps

1. [版本] 有哪些常见的版本控制工具？

1. [版本] 集中式和分布式版本控制有什么区别？

1. [版本] **如何使用 Git 进行版本控制？CLI & Tortoise**

1. [版本] 应如何理解Git中的rebase概念？

1. [测试] **如何编写基于 XUnit 的单元测试案例？**

1. [测试] **如何 Mock一个需要的对象？`MagicMock` & `create_autospec`**

1. [测试] 有哪些常用的单元测试框架以及 test runner 框架？应如何选择？

1. [测试] Doctest 的作用和用法？

1. [日志] **如何使用 logging 模块打日志？**

1. [日志] 输出日志有哪些注意事项？Async, Daemon, Format, Access/Error/Transaction, RequestID

1. [部署] **pip 的常用命令有哪些？**

1. [部署] 如何为 pip 配置更快的源？Windows, Linux, Mac, [清华源](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)

1. [部署] 什么是 Anaconda？[Download](https://www.anaconda.com/download/)

1. [部署] 如何使用 Anaconda 管理 Python包？[清华源](https://mirror.tuna.tsinghua.edu.cn/help/anaconda/)

1. [部署] Anaconda Navigtor 的使用，[参考](http://docs.anaconda.com/anaconda/user-guide/getting-started/)

1. [部署] Jupyter notebook 如何使用？[Github](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/Installation-Anaconda.md#3-%E5%8F%82%E8%80%83%E6%96%87%E6%A1%A3) 或 [Gitee](https://gitee.com/wu-wen-xiang/training-python/blob/master/doc/Installation-Anaconda.md#3-%E5%8F%82%E8%80%83%E6%96%87%E6%A1%A3)。ipynb 文件可以用 VSCode 打开。

1. [部署] QTConsole 如何使用？

1. [部署] Spyder 如何使用？在tools->preferences->IPython console->advanced Settings 下面，把User the greedy completer 勾选上。自动补全用Tab，批量注释用Ctrl + 1: 注释/反注释，Tab/Shift + Tab: 代码缩进/反缩进

1. [部署] **什么是 virtualenv？如何使用？** `python -m virtualenv .env`

1. [自动] 有哪些常用的自动化管理框架？我们应如何选择？ ansible

1. [自动] Fabric 的实现原理是什么？

1. [自动] Fabric 的常用函数有哪些？

1. [自动] **Ansible 框架的实现原理是什么？**

1. [自动] Ansible 应如何安装？

1. [自动] **Ansible 框架的部署架构是怎样的？**

1. [自动] 什么是 YAML？

1. [自动] **如何理解和编写 YAML 描述文件？**

1. [自动] **什么是 Ansible 中的 Inventory？**

1. [自动] Ansible 中的常用模块有哪些？

1. [自动] **Ansible 中的 playbook 是什么？**

1. [自动] **Ansible 中的 Role 是什么？**

1. [自动] 什么是 AWX？

1. [自动] AWX 应如何安装和使用？

1. [自动] SaltStack 框架的架构是怎样的？

1. [自动] SaltStack 适用于什么场景（Ansible不适用）？

1. [容器] **什么是 Docker？LXC，namespace，cgroup，CE/EE 架构有何不同？**

1. [容器] Docker 的使用场景和使用方法是怎样的？

1. [容器] **DockerFile 应如何编写？**

1. [容器] **Docker-Compose 应如何使用？**

1. [容器] Docker-Machine 是什么？如何使用？

1. [容器] **Docker 网络配置方法是怎样的？Linux 和Windows 环境有什么不同？**

1. [容器] **K8S 是什么？应如何使用？**

### Web框架

1. [ORM] MySQL 的安装和基本操作是怎样的？[参考](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/Database.md)

1. [ORM] Sqlite3 的原理和使用方法是怎样的？

1. [ORM] 使用什么工具查看和编辑 Sqlite3 数据库文件？[sqlitebrowser](http://sqlitebrowser.org/)

1. [ORM] 基本的数据库连接和处理流程是怎样的？

1. [ORM] 什么是 ORM 模型？

1. [ORM] **如何使用 SqlAlchemy 框架？Code First，表初始化，Migration，CRUD**

1. [架构] Web 应用的架构和流程是怎样的？

1. [架构] HTML/CSS/JavaScript 在前端显示中各自起到什么作用？

1. [架构] **什么是 CGI/FastCGI/Module 模式？**

1. [架构] **什么是 WSGI 接口？**

1. [MVC] **什么是 MVC 模型？**

1. [MVC] 如何从头实现一个 Python 框架？[Python2-WSGI](https://github.com/michaelliao/awesome-python-webapp/)，[Python3-Async](https://github.com/michaelliao/awesome-python3-webapp)

1. [服务] 什么是 WebService？

1. [服务] 什么是 JSON？

1. [Flask] Python 中常见的 Web 框架有哪些？各自有什么优缺点？各自有什么代表作？我们应如何选择合适的框架？

1. [Flask] Flask 框架的架构和特点是怎样的？

1. [Flask] **Jinja2 语法是怎样的？**

1. [Flask] **Flask 和 SqlAlchemy 如何整合？**

1. [Flask] **如何使用 Flask 框架快速完成一个基本 CRUD 操作？**

1. [Flask] Flask 项目中如何实现用户认证和权限管理？

1. [Flask] Flask 项目中如何使用 Session/Cookie？

1. [Flask] Flask 项目中如何处理表单？

1. [Flask] **一个用于生产环境的“正经”的 Flask 框架的代码结构是怎样的？blueprint, restful api, configuration, deployment**

1. [Flask] Flask 框架应如何部署？Python2 和 Python3 有何不同？

1. [Django] **Django 框架的特点和基本流程是怎样的？**

1. [Django] 应该如何选择 Django 版本？[参考](https://www.djangoproject.com/download/)

1. [Django] **Django 框架中如何 Startup，编写 config 文件和 Router(urls.py)？** [参考](https://docs.djangoproject.com/en/1.11/intro/tutorial01/)，Demo-Get-Started：[Github](https://github.com/wu-wenxiang/Training-Django-Public/blob/master/01-Get-Started) 或者 [Gitee](https://gitee.com/wu-wen-xiang/training-django/blob/master/01-Get-Started)

1. [Django] **Django 框架中 Views 方法如何接收到 uri参数？** Demo-Dynamic-Urls：[Github](https://github.com/wu-wenxiang/Training-Django-Public/blob/master/02-Dynamic-Urls) 或者 [Gitee](https://gitee.com/wu-wen-xiang/training-django/blob/master/02-Dynamic-Urls)

1. [Django] **Django 框架中 Template 的语法是怎样的？与 Jinja2 有什么区别？** Demo-Templates：[Github](https://github.com/wu-wenxiang/Training-Django-Public/blob/master/03-Templates) 或者 [Gitee](https://gitee.com/wu-wen-xiang/training-django/blob/master/03-Templates)

1. [Django] **Django 框架中的 ORM 模型如何使用？** Demo-Models：[Github](https://github.com/wu-wenxiang/Training-Django-Public/blob/master/04-Models) 或者 [Gitee](https://gitee.com/wu-wen-xiang/training-django/blob/master/04-Models)

1. [Django] **Django 框架中的 Admin 如何使用？** Demo-Admins：[Github](https://github.com/wu-wenxiang/Training-Django-Public/blob/master/05-Admins) 或者 [Gitee](https://gitee.com/wu-wen-xiang/training-django/blob/master/05-Admins)

1. [Django] **Django 框架中的表单处理是怎样的？** Demo-Forms：[Github](https://github.com/wu-wenxiang/Training-Django-Public/blob/master/06-Forms) 或者 [Gitee](https://gitee.com/wu-wen-xiang/training-django/blob/master/06-Forms)

1. [Django] Django 框架中如何编写 Unittest？

1. [Django] Django 框架中如何处理静态文件？

1. [Django] Django 框架中的登陆过程是怎样的？

1. [Django] Django 框架中如何实现 Restful API？

1. [Django] Django 框架如何部署？ [Demo-Fabric](https://github.com/wu-wenxiang/Project-Python-Webdev/tree/master/u1604-fabric)

1. [Django] 如何对 Django 框架进行性能监控和调优？

1. [微信] 订阅号和服务号有什么区别？各自的应用场景是什么？

1. [微信] 微信的身份认证机制是怎样的？

1. [微信] 微信订阅号的开发和调试步骤是怎样的？

1. [微信] 微信服务号的支付功能是怎样实现的？

1. [微信] 小程序的实现原理是什么？适用于什么场景？

1. [微信] Web 站点如何使用微信扫码登录？

### 数据采集

1. [爬虫] 爬虫的基本原理是什么？

1. [爬虫] **urllib 模块的基本使用方法是什么？登录，SSL**

1. [爬虫] 如何使用 bs4 模块解析 HTML 数据？

1. [爬虫] 如何爬取股票数据并绘制K线图？

1. [爬虫] XPath 的使用方法是怎样的？

1. [Scrapy] Scrapy 框架的架构是怎样的？

1. [Scrapy] **Scrapy 的基本使用方法是怎样的？**, [参考](https://docs.scrapy.org/en/latest/)

1. [Scrapy] 如何使用Scrapy框架做整站爬取？

### 数据挖掘

基本概念，常见类库，案例

1. [概念] 什么是结构化数据？

1. [概念] 什么是数据挖掘？

1. [概念] **数据挖掘的基本流程是怎样的？**

1. [概念] 有哪些数据挖掘相关的类库？

1. [概念] 什么是回归分析？适用于哪些场合？

1. [概念] 什么是决策树？适用于哪些场合？

1. [概念] 什么是神经网络算法？适用于哪些场合？

1. [概念] 什么是 K-Means 聚类算法？适用于哪些场合？

1. [概念] 什么是 Apriori 算法？适用于哪些场合？

1. [类库] **如何安装科学计算环境？**[scipy 安装](https://scipy.org/install.html)：[Anaconda](https://www.anaconda.com/download/) 或 `pip install --user numpy scipy matplotlib ipython jupyter pandas sympy nose`（因为用了 --user，所以在 安装到 Windows 系统中个人 roaming 下，如果要用 jupyter 这类工具，需要将 pip install 时的 roaming Scripts 路径加到 Path 环境变量中）

1. [类库] **Numpy 提供什么数据结构对象和能力？** ndarray（别名 array）：多维数组和各种派生对象（如掩码数组和矩阵），能力：用于数组快速操作的 API，包括数学、逻辑、形状操作、排序、选择、输入输出、离散傅立叶变换、基本线性代数，基本统计运算和随机模拟。[快速入门](https://www.numpy.org.cn/user/quickstart.html)

1. [类库] Python 的标准库有哪些涉及数据类型的？[DataType](https://docs.python.org/3/library/datatypes.html)：array, collections, heapq, bisect

1. [类库] NumPy ndarray 和原生 Python array 有何不同？固定大小，支持 numpy 和绝大多数科学计算类库 API（Python array 只处理一维数组并提供较少的功能）

1. [类库] Numpy ndarray 有哪些重要的字段属性？ndim，shape，size，dtype，itemsize

1. [类库] **如何创建 Numpy ndarray？**（zeros, ones, empty, array([...]), arange, linespace，fromfunction）, 随机数，以及 linspace 和 arange step 各自用于什么场景？

1. [类库] **Numpy ndarray 有哪些基本操作？**[功能和方法概述](https://www.numpy.org.cn/user/quickstart.html#%E5%8A%9F%E8%83%BD%E5%92%8C%E6%96%B9%E6%B3%95%E6%A6%82%E8%BF%B0)：Broadcasting，叉乘，+=（会改变自身），b.sum(axis=x)，通函数（ufunc），slice 赋值标量，`[1:5][1]` 和 `[1:5,1]` 的差异（当提供的索引少于轴的数量时，缺失的索引被认为是完整的切片），`...` 索引，迭代和 flat 迭代，布尔数组筛选（`a[a==1]`）

1. [类库] 如何改变 Numpy ndarray 的形状？ravel（c风格），reshape（-1），T，resize，array.shape=2,-1

1. [类库] 如何堆叠两个 Numpy ndarray？hstack, vstack, column_stack, row_stack

1. [类库] 如何拆分 Numpy ndarray？hsplit, vsplit, array_split

1. [类库] 如何理解和使用 Numpy ndarray 对象的浅拷贝（view, 切片），深拷贝（array.copy，b.base is a）？

1. [类库] **Pandas 提供什么数据结构对象和能力？** DataFrame 和 Series，DataFrame 是 Series 的容器，Series 则是标量的容器。可以在容器中以字典的形式插入或删除对象。[快速入门](https://www.pypandas.cn/docs/getting_started/)，[数据结构简介](https://www.pypandas.cn/docs/getting_started/dsintro.html)，CheatSheet：[Github](https://github.com/wu-wenxiang/training-python-public/blob/master/doc/pdf/Pandas-Cheat-Sheet.pdf) 或 [Gitee](https://gitee.com/wu-wen-xiang/training-python/blob/master/doc/pdf/Pandas-Cheat-Sheet.pdf)

1. [类库] **如何理解和使用 Pandas 中的 DataFrame 对象？** 生成对象，describe，选择（列、切片、`loc[:,['A','B']]`、iloc），运算，Merge，Grouping，读写文件（数据库），columns，index，根据 Index 定位

1. [类库] Pandas 如何把一列字符串数据变成时间数据？如何计算时间差？`pd.to_datetime('2014/03/31') - pd.to_datetime(data1['FFP_DATE'])` 如何把时间差变成整数？`df['ts'].dt.days` 如何把字符串变成数字？`pd.to_numeric()`

1. [类库] Pandas 数据如何处理时间序列绘图？设置时间列为 index，然后 `df.plot(); plt.show()`，如果要汇总可以用 resample

1. [类库] Pandas 如何处理数据量一次读入过多问题？千万级数据表问题不大。可以用 ChunkSize 可以解决 [40 亿条数据的问题](https://blog.csdn.net/weixin_43790560/article/details/88587123)

1. [类库] Scipy 提供哪些功能？[参考](https://docs.scipy.org/doc/scipy/reference/tutorial/)：解方程，积分，优化，插值，傅立叶变换，信号处理，线性代数，稀疏矩阵，统计分析，多维图像处理。[相关技术](https://www.scipy.org/getting-started.html)：numpy、pandas、[matplotlib](http://matplotlib.org/users/beginner.html)、[symPy](http://docs.sympy.org/latest/tutorial/)

1. [类库] 如何使用 Scipy 解非线性方程组？

1. [类库] 如何使用 Scipy 做数值积分？

1. [类库] **如何使用 Matplotlib 绘制解析几何图形？**[教程](https://www.matplotlib.org.cn/tutorials/)，[图例库](https://www.matplotlib.org.cn/gallery/)，CheatSheet（[Github](
https://github.com/wu-wenxiang/training-python-public/blob/master/doc/pdf/Matplotlib-Cheat-Sheet.pdf) 或 [Gitee](https://gitee.com/wu-wen-xiang/training-python/blob/master/doc/pdf/Matplotlib-Cheat-Sheet.pdf)），[官方快速入门](https://matplotlib.org/stable/tutorials/introductory/usage.html)，[官方图例](https://matplotlib.org/stable/gallery/index.html)

1. [类库] **如何使用 Scikit-Learn 完成回归、分类、聚类运算？** [参考](http://blog.wuwenxiang.net/Machine-Learning)，[官方文档](https://sklearn.apachecn.org/)，[Gitee](https://apachecn.gitee.io/sklearn-doc-zh)

1. [案例] 什么是适用于消费类数据的 RFM 模型？[Recency/Frequency/Monetary](https://searchdatamanagement.techtarget.com/definition/RFM-analysis)

1. [案例] 数据分析的一般步骤是什么？数据探索，数据清洗，属性规约，数据变换

1. [案例] 如何完成航空公司客户价值分析？KMeans

1. [案例] 如何完成电商评论产品评论数据情感分析？[gensim](http://gensim.apachecn.org) & LDA（每篇文档的每一个词都是通过一定概率选择了某一个主题，并从这个主题中以一定概率选择了某个词语）：三层贝叶斯模型

1. [案例] 如何完成财政收入影响因素分析？

1. [案例] 如何完成电商用户行为分析及服务推荐？[协同过滤推荐算法](https://zhuanlan.zhihu.com/p/80069337)

1. [案例] 如何完成电力窃漏电用户自动识别？拉格朗日插值，窃电模型，（LM 神经网络 / CART 决策树）分类

## 其它应用

### 云计算OpenStack

### PaaS云平台

1. [GAE] GAE 的基本使用方法是怎样的？

1. [SAE] SAE 的基本使用方法是怎样的？

### Office文档处理

1. [Word] 如何实现对 Word 文档的读、写操作？

1. [Excel] **如何实现对 Excel 文档的读、写操作？** pandas 对 to_excel('*.xls') 即将技术支持，后续要用 openpyxl 代替，写文件只能写 xlsx 格式

### Python-C

1. [CType] Python 如何调用 C 类库？[CType](https://github.com/wu-wenxiang/training-python-public/tree/master/src/C-Extension)

1. [调用] C 语言如何使用 Python 对象？

1. [实现] 如何实现一个基于 C 的 Python 模块？[CModule](https://github.com/wu-wenxiang/training-python-public/tree/master/src/C-Extension)

### PVM内存分析

1. [PDB] Pdb 的使用和局限性是怎样的？

1. [Dump] 如何收集 Dump？

1. [Windbg] 如何使用 Windbg 分析 PVM 内存？

1. [GDB] 如何使用 Windbg 分析 PVM 内存？

### IoT

1. [树莓] **如何烧制树莓板？Win10/Raspbian**

1. [GPIO] **如何使用 Python 控制 GPIO 口**

1. [GPIO] 如何使用面包板搭建 GPIO 口的输入、输出电路？

1. [Azure IoT DevKit] 如何使用 DevKit 将收集到的温度/湿度信息上传到 Azure 云端，并通过 PowerBI 显示出来？[参考](https://github.com/wu-wenxiang/Training-Python/tree/master/Python-Common/IoT/%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E4%BD%A0%E5%81%9AIoT%E8%AE%BE%E5%A4%87)

### MineCraft

1. [基础] MineCraft 基础和 Hook 原理是什么？

1. [绘制] 如何绘制三维图形？

1. [捕捉] 如何步骤游戏角色的位置？

1. [现实] 如何在游戏中控制 GPIO 口？

### GUI

1. [TK] TK 的基本处理流程和布局方式是怎样的？
