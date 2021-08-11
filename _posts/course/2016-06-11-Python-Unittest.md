---
layout:         post
title:          Python-Unittest
category:       course
description:    总结了 Python 单元测试中常见的单元测试框架，比较他们的适用场景，并给出使用和选择建议。
---

## Startup

- 单元测试的核心价值在于两点：
    1. 更加精确地定义某段代码的作用，从而使代码的耦合性更低
    1. 避免程序员写出不符合预期的代码，以及因新增功能而带来的 Regression Bug
- 本文的目的
    - 随着 Test-Driven 方法论的流行，测试类库对于高级语言来说变得不可或缺。
    - Python 生态圈中的 unit testing framework 相当多，不同于 Java 几乎只有 JUnit 与 TestNG 二选一，Python unittest 框架中较为活跃并也有较多使用者的 framework 就有unittest、unittest2、nose、nose2 与 py.test 等。不计其他较小众的工具，光是要搞懂这些工具并从中挑选一个合适的出来使用就让人头大了。
    - 本文因此总结了这些类库在实战中的作用，以便读者在选择时方便比对参考。
- 这里是介绍 Python 测试的官方文档：
    - [英文版](http://docs.python-guide.org/en/latest/writing/tests/)
    - [中文版](http://pythonguidecn.readthedocs.io/zh/latest/writing/tests.html)
- 本文在该文档的基础上删减了入门部分，增加了深入讲解和实战案例。

## 常见类库

### Unittest

- Unittest 的标准文档在这里：
    1. [Python2](https://docs.python.org/2/library/unittest.html)
    1. [Python3](https://docs.python.org/3/library/unittest.html)
- Unittest 是 Python 标准库的一部分。它是目前最流行的固件测试框架 XUnit 在 Python 中的实现，如果你接触过 Junit，nUnit，或者 CppUnit，你会非常熟悉它的 API。
- Unittest 框架的单元测试类用例通过继承 unittest.TestCase 来实现，看起来像是这样：

    ```python
    import unittest

    def fun(x):
        return x + 1

    class MyTest(unittest.TestCase):
        def test(self):
            self.assertEqual(fun(3), 4)
    ```

- Unittest 一共包含 4 个概念：
    1. Test Fixture，就是 Setup() 和 TearDown()
    1. Test Case，一个 Test Case 就是一个测试用例，他们都是 unittest.TestCase 类的子类的方法
    1. Test Suite，Test Suite 是一个测试用例集合，**基本上你用不到它**，用 unittest.main() 或者其它发现机制来运行所有的测试用例就对了。 :)
    1. Test runner，这是单元测试结果的呈现接口，你可以定制自己喜欢的呈现方式，比如GUI界面，**基本上你也用不到它**。
- **一些实战中需要用到的技巧**：
    - 发现机制

        ```python
        python -m unittest discover -s Project/Test/Directory -p "*test*"
        # 等同于
        python -m unittest discover -s Project/Test/Directory
        ```

    - 用 Assert，**不要**用 FailUnless（它们已经被废弃）

        ![Deprecated.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/1faa032c59274913b7473091b5c42fa7-Deprecated.png)

    - 常用的 Assert

        ![NormalAssert.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/1faa032c59274913b7473091b5c42fa7-NormalAssert.png)

    - 特殊的 Assert

        ![SpecificAssert.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/1faa032c59274913b7473091b5c42fa7-SpecificAssert.png)

        For example:

        `assertAlmostEqual(1.1, 3.3-2.15, places=1)`

        ![SpecificEqual.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/1faa032c59274913b7473091b5c42fa7-SpecificEqual.png)

    - AssertException

        ![AssertException.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/1faa032c59274913b7473091b5c42fa7-AssertException.png)

        - assertRaises
            - `assertRaises(exception, callable, *args, **kwds)`

                ```python
                def raisesIOError(*args, **kwds):
                    raise IOError("TestIOError")

                class FixtureTest(unittest.TestCase):
                    def test1(self):
                        self.assertRaises(IOError, raisesIOError)

                if __name__ == '__main__':
                    unittest.main()
                ```

            - `assertRaises(exception)`

                ```python
                # If only the exception argument is given,
                # returns a context manager so that the code
                # under test can be written inline rather
                # than as a function
                with self.assertRaises(SomeException):
                    do_something()

                # The context manager will store the caught
                # exception object in its exception attribute.
                # This can be useful if the intention is to
                # perform additional checks on the exception raised
                with self.assertRaises(SomeException) as cm:
                    do_something()

                the_exception = cm.exception
                self.assertEqual(the_exception.error_code, 3)
                ```

        - assertRaisesRegexp

            ```python
            self.assertRaisesRegexp(ValueError, "invalid literal for.*XYZ'$", int, 'XYZ')
            # or
            with self.assertRaisesRegexp(ValueError, 'literal'):
                int('XYZ')
            ```

    - Skip，出于各种原因，你可能需要暂时跳过一些测试用例（而不是删除它们）

        ```python
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
        ```

    - Class level fixtures

        ```python
        import unittest

        class Test(unittest.TestCase):
            @classmethod
            def setUpClass(cls):
                cls._connection = createExpensiveConnectionObject()

            @classmethod
            def tearDownClass(cls):
                cls._connection.destroy()
        ```

    - Module level fixtures

        ```python
        # These should be implemented as functions:

        def setUpModule():
            createConnection()

        def tearDownModule():
            closeConnection()
        ```

### Mock

- 简述
    - [Mock](http://www.voidspace.org.uk/python/mock/)  类库是一个专门用于在 unittest 过程中制作（伪造）和修改（篡改）测试对象的类库，制作和修改的目的是避免这些对象在单元测试过程中依赖外部资源（网络资源，数据库连接，其它服务以及耗时过长等）。Mock 是一个如此重要的类库，如果没有它，Unittest 框架从功能上来说就是不完整的。所以不能理解为何它没有出现在 Python2 的标准库里，不过我们可以很高兴地看到在 Python3 中 mock 已经是 unittest 框架的一部分。
- **猴子补丁**，[Monkey-patching](https://en.wikipedia.org/wiki/Monkey_patch) is the technique of swapping functions or methods with others in order to change a module, library or class behavior.

    ```python
    >>> class Class():
    ...    def add(self, x, y):
    ...       return x + y
    ...
    >>> inst = Class()
    >>> def not_exactly_add(self, x, y):
    ...    return x * y
    ...
    >>> Class.add = not_exactly_add
    >>> inst.add(3, 3)
    9
    ```

- **Mock对象**
    - return_value: 设置 Mock 方法的返回值

        ```python
        >>> from mock import Mock
        >>> class ProductionClass(): pass
        ...
        >>> real = ProductionClass()
        >>> real.method = Mock(return_value=3)
        >>> real.method(3, 4, 5, key='value')
        3
        >>> real.method.assert_called_with(3, 4, 5, key='value')
        >>> real.method.assert_called_with(3, 4, key='value')
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
          File "/usr/local/lib/python2.7/site-packages/mock/mock.py", line 937, in assert_called_with
            six.raise_from(AssertionError(_error_message(cause)), cause)
          File "/usr/local/lib/python2.7/site-packages/six.py", line 718, in raise_from
            raise value
        AssertionError: Expected call: mock(3, 4, key='value')
        Actual call: mock(3, 4, 5, key='value')
        ```

    - side_effect:
        - 调用 Mock 方法时，抛出异常

            ```python
            >>> mock = Mock(side_effect=KeyError('foo'))
            >>> mock()
            Traceback (most recent call last):
             ...
            KeyError: 'foo'

            >>> mock = Mock()
            >>> mock.return_value = 42
            >>> mock()
            42
            ```

        - 调用 Mock 方法时，根据参数得到不同的返回值

            ```python
            >>> values = {'a': 1, 'b': 2, 'c': 3}
            >>> def side_effect(arg):
            ...     return values[arg]
            ...
            >>> mock.side_effect = side_effect
            >>> mock('a'), mock('b'), mock('c')
            (1, 2, 3)
            ```

        - 模拟生成器

            ```python
            >>> mock.side_effect = [5, 4, 3, 2, 1]
            >>> mock()
            5
            >>> mock(), mock(), mock(), mock()
            (4, 3, 2, 1)
            >>> mock()
            Traceback (most recent call last):
              File "<stdin>", line 1, in <module>
              File "/usr/local/lib/python2.7/site-packages/mock/mock.py", line 1062, in __call__
                return _mock_self._mock_call(*args, **kwargs)
              File "/usr/local/lib/python2.7/site-packages/mock/mock.py", line 1121, in _mock_call
                result = next(effect)
              File "/usr/local/lib/python2.7/site-packages/mock/mock.py", line 109, in next
                return _next(obj)
            StopIteration
            ```

    - patch：在函数（function）或者环境管理协议（with）中模拟对象，离开函数或者环境管理器范围后模拟行为结束。
        - 在函数中

            ```python
            from mock import patch

            class AClass(object): pass
            class BClass(object): pass

            print id(AClass), id(BClass)

            @patch('__main__.AClass')
            @patch('__main__.BClass')
            def test(x, MockClass2, MockClass1):
                print id(AClass), id(BClass)
                print id(MockClass1), id(MockClass2)
                print AClass
                print AClass()
                assert MockClass1.called
                print x

            test(42)

            # output:
            """
            140254580491744 140254580492688
            4525648336 4517777552
            4525648336 4517777552
            <MagicMock name='AClass' id='4525648336'>
            <MagicMock name='AClass()' id='4525719824'>
            42
            """
            ```

        - 在环境管理协议中

            ```python
            >>> class Class(object):
            ...     def method(self):
            ...         pass
            ...
            >>> with patch('__main__.Class') as MockClass:
            ...     instance = MockClass.return_value
            ...     instance.method.return_value = 'foo'
            ...     assert Class() is instance
            ...     assert Class().method() == 'foo'
            ...
            ```

        - Spec Set 的写法，**你应该用不到**

            ```python
            >>> Original = Class
            >>> patcher = patch('__main__.Class', spec=True)
            >>> MockClass = patcher.start()
            >>> instance = MockClass()
            >>> assert isinstance(instance, Original)
            >>> patcher.stop()
            ```

    - patch.object: 在函数或者环境管理协议中模拟对象，但只 mock 其中一个 attribute

        ```python
        from mock import patch

        class AClass():
            def method(self, *arg):
                return 42

        with patch.object(AClass, 'method') as mock_method:
            mock_method.return_value = "Fake"
            real = AClass()
            print real.method(1, 2, 3)   # Fake

        mock_method.assert_called_once_with(1, 2, 3)
        print real.method(1, 2, 3) # 42
        ```

    - patch.dict: patch.dict can be used to add members to a dictionary, or simply let a test change a dictionary, and ensure the dictionary is restored when the test ends.

        patch.dict(in_dict, values=(), clear=False, **kwargs)

        If clear is True then the dictionary will be cleared before the new values are set.

        ```python
        >>> from mock import patch
        >>> foo = {}
        >>> with patch.dict(foo, {'newkey': 'newvalue'}):
        ...     assert foo == {'newkey': 'newvalue'}
        ...
        >>> assert foo == {}

        >>> import os
        >>> with patch.dict('os.environ', {'newkey': 'newvalue'}):
        ...     print os.environ['newkey']
        ...
        newvalue
        >>> assert 'newkey' not in os.environ
        ```

    - patch.multiple: Perform multiple patches in a single call.

        ```python
        >>> thing = object()
        >>> other = object()

        >>> @patch.multiple('__main__', thing=DEFAULT, other=DEFAULT)
        ... def test_function(thing, other):
        ...     assert isinstance(thing, MagicMock)
        ...     assert isinstance(other, MagicMock)
        ...
        >>> test_function()

        >>> @patch('sys.exit')
        ... @patch.multiple('__main__', thing=DEFAULT, other=DEFAULT)
        ... def test_function(mock_exit, other, thing):
        ...     assert 'other' in repr(other)
        ...     assert 'thing' in repr(thing)
        ...     assert 'exit' in repr(mock_exit)
        ...
        >>> test_function()
        ```

- **MagicMock**: MagicMock 是 Mock 的子类。MagicMock is a subclass of Mock with default implementations of most of the magic methods. You can use MagicMock without having to configure the magic methods yourself.
    - MagicMock 的功能完全 cover Mock，比如：

        ```python
        from mock import MagicMock
        thing = ProductionClass()
        thing.method = MagicMock(return_value=3)
        thing.method(3, 4, 5, key='value')    # return 3

        thing.method.assert_called_with(3, 4, 5, key='value')
        ```

    - MagicMock 相对于 Mock 的优势：

        ```python
        >>> from mock import MagicMock
        >>> mock = MagicMock()
        >>> mock.__str__.return_value = 'foobarbaz'
        >>> str(mock)
        'foobarbaz'
        >>> mock.__str__.assert_called_once_with()

        # 原来需要：

        >>> from mock import Mock
        >>> mock = Mock()
        >>> mock.__str__ = Mock(return_value = 'wheeeeee')
        >>> str(mock)
        'wheeeeee'
        ```

- **create_autospec**: 使 Mock 对象拥有和原对象相同的字段和方法，对于方法对象，则拥有相同的签名

    ```python
    >>> from mock import create_autospec
    >>> def function(a, b, c):
    ...     pass
    ...
    >>> mock_function = create_autospec(function, return_value='fishy')
    >>> mock_function(1, 2, 3)
    'fishy'
    >>> mock_function.assert_called_once_with(1, 2, 3)
    >>> mock_function('wrong arguments')
    Traceback (most recent call last):
     ...
    TypeError: <lambda>() takes exactly 3 arguments (1 given)

    >>> from mock import create_autospec
    >>>
    >>> mockStr = create_autospec(str)
    >>> print mockStr.__add__("d", "e")
    <MagicMock name='mock.__add__()' id='4537473552'>
    ```

### Unittest2

- 简述
    - Unittest2 致力于将 Python2.7 及以后版本上 unittest 框架的新特性移植（backport）到 Python2.4~Python2.6 平台中。
    - Backport 是将一个软件补丁应用到比该补丁所对应的版本更老的版本的行为。
    - 你知道这些就可以了，基本上**你不会用到它**。
- 参考
    - [*The new features in unittest backported to Python 2.4+. unittest2 is a backport of the new features added to the unittest testing framework in Python 2.7 and onwards.*](https://pypi.python.org/pypi/unittest2)
    - [*unittest2 is a backport of Python 2.7’s unittest module which has an improved API and better assertions over the one available in previous versions of Python.*](http://docs.python-guide.org/en/latest/writing/tests/)
    - [*unittest2py3k is the Python 3 compatible version of unittest2*](https://pypi.python.org/pypi/unittest2py3k)

### py.test

- 简述
    - [pytest](http://pytest.org) 是另一种固件测试框架，它的 API 设计非常简洁优雅，完全脱离了 XUnit 的窠臼（unittest 是 XUnit 在 Python 中的实现）。但这也正是它的缺点，unittest 是标准库的一部分，用者甚众，与之大异难免曲高和寡。
- py.test 功能完备，并且可扩展，但是它语法很简单。创建一个测试组件和写一个带有诸多函数的模块一样容易，来看一个例子

    ```python
    # content of test_sample.py
    def func(x):
        return x + 1

    def test_answer():
        assert func(3) == 5
    ```

- 运行一下：

    ```console
    $ py.test
    ============= test session starts =============
    platform darwin -- Python 2.7.11, pytest-2.9.2, py-1.4.31, pluggy-0.3.1
    rootdir: /Users/wuwenxiang/Documents/workspace/testPyDev, inifile:
    collected 1 items

    some_test.py F

    ================== FAILURES ===================
    _________________ test_answer _________________

        def test_answer():
    >       assert func(3) == 5
    E       assert 4 == 5
    E        +  where 4 = func(3)

    some_test.py:6: AssertionError
    ========== 1 failed in 0.01 seconds ===========
    ```

- 官方文档中入门的例子在[这里](http://pytest.org/latest/example/simple.html)，pytest 也给出了 unittest Style 的兼容写法[示例](https://pytest.org/latest/unittest.html)，然并 X，看完之后你会发现：圈子不同，不必强融，这句话还真TM有道理。
- py.test 的 setup/teardown 语法与 unittest 的兼容性不高，实现方式也不直观。我们来看一下 setup/teardown 的例子：

    ```python
    # some_test.py

    import pytest

    @pytest.fixture(scope='function')
    def setup_function(request):
        def teardown_function():
            print("teardown_function called.")
        request.addfinalizer(teardown_function)
        print('setup_function called.')

    @pytest.fixture(scope='module')
    def setup_module(request):
        def teardown_module():
            print("teardown_module called.")
        request.addfinalizer(teardown_module)
        print('setup_module called.')

    def test_1(setup_function):
        print('Test_1 called.')

    def test_2(setup_module):
        print('Test_2 called.')

    def test_3(setup_module):
        print('Test_3 called.')
    ```

- pytest 创建固件测试环境（fixture）的方式如上例所示，通过显式指定 `scope=''` 参数来选择需要使用的 `pytest.fixture` 装饰器。即一个 fixture 函数的类型从你定义它的时候就确定了，这与使用 `@nose.with_setup()` 不同。对于 `scope='function'` 的 fixture 函数，它就是会在测试用例的前后分别调用 setup/teardown。测试用例的参数如 `def test_1(setup_function)` 只负责引用具体的对象，它并不关心对方的作用域是函数级的还是模块级的。
- 有效的 scope 参数限于：**function, module, class, session**，默认为function。
- 运行上例：`$ py.test some_test.py -s`。**-s** 用于显示`print()`函数
- 执行效果：

    ```console
    $ py.test -s some_test.py
    ============= test session starts =============
    platform darwin -- Python 2.7.11, pytest-2.9.2, py-1.4.31, pluggy-0.3.1
    rootdir: /Users/wuwenxiang/Documents/workspace/testPyDev, inifile:
    collected 3 items

    some_test.py setup_function called.
    Test_1 called.
    .teardown_function called.
    setup_module called.
    Test_2 called.
    .Test_3 called.
    .teardown_module called.

    ========== 3 passed in 0.01 seconds ===========
    ```

- 这里需要注意的地方是：setup_module 被调用的位置。

### Nose

- nose 广为流传，它主要用于配置和运行各种框架下的测试用例，有更简洁友好的测试用例发现功能。nose 的自动发现策略是会遍历文件夹，搜索特征文件（默认是搜索文件名中带 test 的文件）

    $ nosetests
    F.
    ======================================================================
    FAIL: some_test.test_answer
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "/usr/local/lib/python2.7/site-packages/nose/case.py", line 197, in runTest
        self.test(*self.arg)
      File "/Users/wuwenxiang/Documents/workspace/testPyDev/some_test.py", line 6, in test_answer
        assert func(3) == 5
    AssertionError

    ----------------------------------------------------------------------
    Ran 2 tests in 0.004s

    FAILED (failures=1)
- 很可惜，[官网](https://nose.readthedocs.io/en/latest/)说：Nose has been in maintenance mode for the past several years and will **likely cease** without a new person/team to take over maintainership. New projects should consider using [Nose2](http://nose2.readthedocs.io/en/latest/), py.test, or just plain unittest/unittest2.
- Nose2 是 Nose 的原班人马开发。[nose2 is being developed by the same people who maintain nose.](http://nose2.readthedocs.io/en/latest/differences.html)
- Nose2 是基于 unittest2 plugins 分支开发的，但并不支持 python2.6 之前的版本。Nose2 致力于做更好的 Nose，它的 Plugin API 并不兼容之前 Nose 的 API，所以如果你 migration from Nose，必须重写这些 plugin。*nose2 implements a new plugin API based on the work done by Michael Foord in unittest2’s plugins branch. This API is greatly superior to the one in nose, especially in how it allows plugins to interact with each other. But it is different enough from the API in nose that supporting nose plugins in nose2 will not be practical: plugins must be rewritten to work with nose2.*
- 然而…… Nose2 的更新…… 也很有限……
- 其作者Jason Pellerin先生坦言他目前(2014年)并没有多余的时间进行 personal projects 的开发，每周对 nose 与 nose2 的实际开发时间大概只有 30 分钟，在这种情况下，nose 与 nose2 都将很难再有大的改版与修正。

### Green

- 不同与 nose/nose2，[green](https://github.com/CleanCut/green)是单纯为了强化 unittest 中 test runner 功能而出现的工具。green 所提供的只有一个功能强大、使用方便、测试报告美观的 test runner。如果你的项目中的测试都是以传统 unittest module 撰写而成的话，green 会是一个很好的 test runner 选择。
- 使用 green 执行测试：

    pip install green
    cd path/to/project
    green

### Doctest

- Doctest 的标准文档在这里：
    1. [Python2](https://docs.python.org/2/library/doctest.html)
    1. [Python3](https://docs.python.org/3/library/doctest.html)
- Doctest 看起来像是在交互式运行环境中的输出，事实上也确实如此 :)

    ```python
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
    ```

- Doctest 的作用是作为函数/类/模块等单元的解释和表述性文档。所以它们有如下特点：
    1. 只有期望对外公开的单元会提供 doctest
    1. 这些 doctest 通常不是很细致
- 编写 doctest 测试基本不需要学习新技能点，在交互式环境里运行一下，然后把输出结果检查一下贴过来就可以了。
- doctest 还有一些高级用法，但基本上用不到，用到的时候再去查标准文档好了。 :)

### Mox

- Mox 是 Java EasyMock 框架在 Python 中的实现。它一个过时的，很像 mock 的类库。从现在开始，你**应该放弃学习 Mox，在任何情况下都用 Mock** 就对了。
- 参考 [Mox 的官方文档](https://pypi.python.org/pypi/mox)：

    ```text
    Mox is a mock object framework for Python based on the Java mock object framework EasyMock.
    New uses of this library are discouraged.
    People are encouraged to use https://pypi.python.org/pypi/mock instead which matches the unittest.mock library available in Python 3.
    ```

- [Mox3](https://pypi.python.org/pypi/mox3) 是一个非官方的类库，是 mox 的 Python3 兼容版本

    ```text
    Mox3 is an unofficial port of the Google mox framework (http://code.google.com/p/pymox/) to Python 3.
    It was meant to be as compatible with mox as possible, but small enhancements have been made.
    The library was tested on Python version 3.2, 2.7 and 2.6.
    Use at your own risk ;)
    ```

## 其它类库

### tox

- [官方文档](https://tox.readthedocs.io/en/latest/): 一个自动化测试框架
    - checking your package installs correctly with different Python versions and interpreters
    - running your tests in each of the environments, configuring your test tool of choice
    - acting as a frontend to Continuous Integration servers, greatly reducing boilerplate and merging CI and shell-based testing.
- Basic example:

    ```ini
    # content of: tox.ini , put in same dir as setup.py
    [tox]
    envlist = py26,py27
    [testenv]
    deps=pytest       # install pytest in the venvs
    commands=py.test  # or 'nosetests' or ...
    ```

- You can also try generating a tox.ini file automatically, by running tox-quickstart and then answering a few simple questions.
- To sdist-package, install and test your project against Python2.6 and Python2.7, just type: `tox`

### testr

- [官方文档](http://testrepository.readthedocs.io/en/latest/): 是一个 test runner。

### Django 的 Unittest

- [官方文档](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/unit-tests/)
- [官方文档](https://docs.djangoproject.com/ja/1.9/topics/testing/) 推荐用 Unittest：The preferred way to write tests in Django is using the unittest module built in to the Python standard library.
- [django.test.TestCase](https://docs.djangoproject.com/ja/1.9/topics/testing/overview/)继承了 unittest.TestCase。
    - Here is an example which subclasses from django.test.TestCase, which is a subclass of unittest.TestCase that runs each test inside a transaction to provide isolation:

        ```python
        from django.test import TestCase
        from myapp.models import Animal

        class AnimalTestCase(TestCase):
            def setUp(self):
                Animal.objects.create(name="lion", sound="roar")
                Animal.objects.create(name="cat", sound="meow")

            def test_animals_can_speak(self):
                """Animals that can speak are correctly identified"""
                lion = Animal.objects.get(name="lion")
                cat = Animal.objects.get(name="cat")
                self.assertEqual(lion.speak(), 'The lion says "roar"')
                self.assertEqual(cat.speak(), 'The cat says "meow"')
        ```

### Flask 的 Unittest

- [官方文档](http://flask.pocoo.org/docs/0.11/testing/)中介绍：Flask provides a way to test your application by exposing the Werkzeug test Client and handling the context locals for you. You can then use that with your favourite testing solution. In this documentation we will use the unittest package that comes pre-installed with Python.

    ```python
    app.test_client()

    app = flask.Flask(__name__)

    with app.test_client() as c:
        rv = c.get('/?tequila=42')
        assert request.args['tequila'] == '42'
    ```

    Unittest with Flask

    ```python
    class FlaskrTestCase(unittest.TestCase):

        def setUp(self):
            self.db_fd, flaskr.app.config['DATABASE'] = tempfile.mkstemp()
            self.app = flaskr.app.test_client()
            flaskr.init_db()

        def tearDown(self):
            os.close(self.db_fd)
            os.unlink(flaskr.app.config['DATABASE'])

        def test_empty_db(self):
            rv = self.app.get('/')
            assert b'No entries here so far' in rv.data
    ```

- 扩展：[Flask-Testing](https://pythonhosted.org/Flask-Testing/)

    ```python
    @app.route("/ajax/")
    def some_json():
        return jsonify(success=True)

    class TestViews(TestCase):
        def test_some_json(self):
            response = self.client.get("/ajax/")
            self.assertEquals(response.json, dict(success=True))
    ```

## 建议和总结

- 在项目中尽量不要 mix 使用多种功能类似的框架。
    - 你可以选 unittest + green，或者 nose/nose2(依使用 Python 版本和项目的历史遗留而定) ，或者 pytest，但是尽量不要混合使用。
- 关于 Unittest
    - 如果没有特别的原因，新项目应该用 unittest。
    - Unittest 中要用 Assert，不要用 FailUnless
    - Django 和 Flask 中都应该用 unittest 框架，他们也都提供了一个 unittest.TestCase 的子类以便于做与 WebServer 相关的测试
- 关于 Mock
    - 如果要 Mock 一个对象，用 MagicMock
    - 如果要在函数或者 with-statment 中 Mock 一个对象，用 patch
    - 如果要在函数或者 with-statement 中 Mock 一个对象的属性，用 patch.object
    - 如果要在函数或者 with-statement 中 Mock 一个字典（增加或重建一个键值对），用 patch.dict
    - 如果要在函数或者 with-statement 中一次 Patch 多个 Mock 对象，用 patch.multiple
    - 如果希望 Mock 对象拥有和原对象相同的字段和方法（对于方法对象，则拥有相同的签名），用 create_autospec。
