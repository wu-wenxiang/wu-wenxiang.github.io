---
layout:         post
title:          Python Unittest
category:       blog
description:    总结了关于Python单元测试的最佳实践的一些例子
---

## Startup

随着Test-Driven方法论的流行，测试类库对于高级语言来说变得不可或缺。Python的单元测试类库种类繁多，本文总结了这些类库在实战中的作用和最佳实践，以供读者参考。

这里是官方的Python测试文档：

- [英文版](http://docs.python-guide.org/en/latest/writing/tests/)
- [中文版](http://pythonguidecn.readthedocs.io/zh/latest/writing/tests.html)

本文在该文档的基础上删减了入门部分，增加了实战案例。

## 类库

### Unittest

### Mock

Mock类库是一个专门用于在unittest过程中伪造和篡改测试对象的类库，伪造和篡改的目的是避免这些对象在单元测试过程中依赖外部资源（网络资源，数据库连接，其它服务以及耗时过长等）。Mock是一个如此重要的类库，如果没有它，Unittest框架从功能上来说就是不完整的。所以我不能理解为何它没有出现在Python2的标准库里，不过万幸，在Python3中mock以及是unittest框架的一部分。

### Unittest2

### py.test

### Nose

### tox

### Mox

Mox是一个过时的，很像mock的类库。从现在开始，你应该放弃学习Mox，在任何情况下都用Mock就对了。

参考 [Mox的官方文档](https://pypi.python.org/pypi/mox)：

	Mox is a mock object framework for Python based on the Java mock object framework EasyMock.
	New uses of this library are discouraged.
	People are encouraged to use https://pypi.python.org/pypi/mock instead which matches the unittest.mock library available in Python 3.

[Mox3](https://pypi.python.org/pypi/mox3) 是一个非官方的类库，是mox的Python3兼容版本

	Mox3 is an unofficial port of the Google mox framework (http://code.google.com/p/pymox/) to Python 3. 
	It was meant to be as compatible with mox as possible, but small enhancements have been made. 
	The library was tested on Python version 3.2, 2.7 and 2.6.
	Use at your own risk ;)

## 实战案例

### 案例1

### 案例2