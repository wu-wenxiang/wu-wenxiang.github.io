---
layout:         post
title:          Go-逆向学习问题总结
category:       course
description:    逆向法学习 Go，一网打尽那些关于 Go 你需要能回答出的问题
---


## 问题分类
- 核心知识
	- **基础**: 开始，开发环境搭建
	- **进阶**: 
- 常见应用
	- **Web开发**: Micro / Go-Micro

---------------------------------------
---------------------------------------


## 核心知识

### 基础

1. [开始] **Go 在编程语言世界中的处于什么样的位置？**[TIOBE](https://tiobe.com/tiobe-index/)，10-20名，2 次入选年度语言
1. [开始] Go 源于什么需求而被创造？规模复杂 + 性能优异。
    - C++ => MySQL / MongoDB，未能平衡代码易用性和健壮性
    - Java => Hadoop
    - 高并发：Erlang（ RabbitMQ ），Scala （ Apache Spark ），Rust，Go
1. [开始] **Go 有什么样的特质和设计哲学？**
    - 简单（ 类 C 非 OOP，接口抽象数据灵活 ）
    - 可靠（ 静态类型 & 垃圾回收 ）
    - 高效（ 高并发 ）
1. [开始] Go 继承和发展了哪些语言？
    - C（ 表达式语法，控制流语句，基本数据类型，按值调用的形参传递和指针 ）
    - Pascal / Oberon-2（ 包、导入、声明、方法声明的语法 ）
    - CSP / Alef （ 并发 ）
    - 其它，APL（ iota ），Scheme（ 嵌套函数 ）
    - 全新（ Slice / defer / 垃圾回收 ）
1. [开始] **Go 适用于哪些场景？**网络服务器 & 客户端工具，可以替换无类型脚本语言，因为兼顾了表达力和安全性。
1. [开始] **和其它高级语言相比，Go 哪些特性有，哪些特性没有？**
    - 有 GC，包系统，一等公民函数，词法作用域，系统调用接口，默认用 UTF-8 编码的不可变字符串。
    - 没有隐式数据类型强制转换（ 浮点数赋值给整数不可以，要显示转换，并且数值过大时行为不保证 ），没有构造和析构函数，没有运算符重载，没有形参默认值，没有继承，没有**范型**，没有**异常**，没有宏，没有函数注解，没有线程局部变量存储（ 依赖编译器判断是否放在线程栈上 & GC ），没有切片语法糖，没有 map / filter / reduce
    - 保证兼容（旧版本Go写的程序，能用新版本的编译器和库）
1. [开始] Go 有什么缺点？[go-is-not-good](https://github.com/ksimka/go-is-not-good)
1. [开始] **有哪些优秀的 Go 学习资源可以参考？** [TGPL](https://book.douban.com/subject/27044219/)，[Learning basic Golang in one day](https://github.com/go-training/training)，[Training Meterials](https://github.com/golang/go/wiki/Training)
1. [开始] 有哪些框架是 Go 语言开发的？
    - 容器：Docker / ETCD / Kubernetes
    - 数据库：TiDB / InfluxDB
    - 消息系统：NSQ
    - 缓存系统：GroupCache
    - Web框架：Beego / Gorilla / Micro / Go-Micro
1. [环境] **如何搭建 Go 开发和调试环境？**
    - VSCode
        - [Go in Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
        - [Code Navigation](https://code.visualstudio.com/docs/editor/editingevolved)
        - [Debugging Go code using VS Code](https://github.com/Microsoft/vscode-go/wiki/Debugging-Go-code-using-VS-Code)
    - Delve
        - [Get Started](https://github.com/go-delve/delve/blob/master/Documentation/cli/getting_started.md)
        - [Delve Documentation](https://github.com/go-delve/delve/tree/master/Documentation)
        - [Using the Go Delve Debugger from the command line](https://www.jamessturtevant.com/posts/Using-the-Go-Delve-Debugger-from-the-command-line/)
        - [Debugging Go with VS Code and Delve](https://flaviocopes.com/go-debugging-vscode-delve/)
    - Goland
        - [Debugging code](https://www.jetbrains.com/help/go/debugging-code.html)
        - [Debugging containerized Go applications](https://blog.jetbrains.com/go/2018/04/30/debugging-containerized-go-applications/)
