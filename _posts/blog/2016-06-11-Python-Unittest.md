---
layout:         post
title:          Python Unittest
category:       blog
description:    总结了关于Python单元测试的最佳实践的一些例子
---

## Startup

单元测试的核心价值在于两点：

1. 更加精确地定义某段代码的作用，从而使代码的耦合性更低
1. 避免程序员写出不符合预期的代码，以及因新增功能而带来的Regression Bug

随着Test-Driven方法论的流行，测试类库对于高级语言来说变得不可或缺。Python的单元测试类库种类繁多（Unittest / Unitest2 / Mock / Mox / Nose / Doctest / py.test / tox），他们彼此之间有什么样的联系和区别？什么情况下应该用哪种类库？这些问题足以让开发者感到困惑，本文因此总结了这些类库在实战中的作用和最佳实践，以供读者参考。

这里是介绍Python测试的官方文档：

- [英文版](http://docs.python-guide.org/en/latest/writing/tests/)
- [中文版](http://pythonguidecn.readthedocs.io/zh/latest/writing/tests.html)

本文在该文档的基础上删减了入门部分，增加了深入讲解和实战案例。

## 类库

### Unittest

Unittest的标准文档在这里：

1. [Python2](https://docs.python.org/2/library/unittest.html)
1. [Python3](https://docs.python.org/3/library/unittest.html)

Unittest是Python标准库的一部分。它是目前最流行的固件测试框架XUnit在Python中的实现，如果你接触过Junit，nUnit，或者CppUnit，你会非常熟悉它的API。

Unittest框架的单元测试类用例通过继承unittest.TestCase来实现，看起来像是这样：

	import unittest
	
	def fun(x):
	    return x + 1
	
	class MyTest(unittest.TestCase):
	    def test(self):
	        self.assertEqual(fun(3), 4)


Unittest一共包含4个理念Concepts：

1. Test Fixture，就是Setup()和TearDown()
1. Test Case，一个Test Case就是一个测试用例，他们都是unittest.TestCase类的子类的方法
1. Test Suite，Test Suite是一个测试用例集合，**基本上你用不到它**，用unittest.main()或者其它发现机制来运行所有的测试用例就对了。 :)
1. Test runner，这是单元测试结果的呈现接口，你可以定制自己喜欢的呈现方式，比如GUI界面，**基本上你也用不到它**。

一些实战中需要用到的技巧：

1. Skip，处于各种原因，可能你需要暂时跳过一些测试用例（而不是删除它们）

	class MyTestCase(unittest.TestCase):
	
	    @unittest.skip("demonstrating skipping")
	    def test_nothing(self):
	        self.fail("shouldn't happen")
	
	    @unittest.skipIf(mylib.__version__ < (1, 3),
	                     "not supported in this library version")
	    def test_format(self):
	        # Tests that work for only a certain version of the library.
	        pass
	
	    @unittest.skipUnless(sys.platform.startswith("win"), "requires Windows")
	    def test_windows_support(self):
	        # windows specific testing code
	        pass

1. AssertException
1. Deprecated aliases 

### Mock

Mock类库是一个专门用于在unittest过程中伪造和篡改测试对象的类库，伪造和篡改的目的是避免这些对象在单元测试过程中依赖外部资源（网络资源，数据库连接，其它服务以及耗时过长等）。Mock是一个如此重要的类库，如果没有它，Unittest框架从功能上来说就是不完整的。所以不能理解为何它没有出现在Python2的标准库里，不过我们可以很高兴地看到在Python3中mock已经是unittest框架的一部分。

### Unittest2

### py.test

### Nose

### tox

### Doctest

Doctest的标准文档在这里：

1. [Python2](https://docs.python.org/2/library/doctest.html)
1. [Python3](https://docs.python.org/3/library/doctest.html)

Doctest看起来像是在交互式运行环境中的输出，事实上也确实如此 :)
	
	def square(x):
	    """Squares x.
	
	    >>> square(2)
	    4
	    >>> square(-2)
	    4
	    """
	
	    return x * x
	
	if __name__ == '__main__':
	    import doctest
	    doctest.testmod()

Doctest的作用是作为函数/类/模块等单元的解释和表述性文档。所以它们有如下特点：

1. 只有期望对外公开的单元会提供doctest
1. 这些doctest通常不是很细致

编写doctest测试基本不需要学习新技能点，在交互式环境里运行一下，然后把输出结果检查一下贴过来就可以了。

doctest还有一些高级用法，但基本上用不到，用到的时候再去查标准文档好了。 :)

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