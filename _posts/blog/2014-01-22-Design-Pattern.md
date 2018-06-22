---
layout:         post
title:          Design Pattern摘录
category:       blog
description:    小结设计模式的What/Why/How，以C#和Python分别举例
---

## Overview

### 面向对象和设计模式
- 面向对象（Object-oriented, OO）之于23种设计模式，犹如兵法之于36计
- 兵法千变万化，取其常见者，整理成36计；同样的，设计模式是**面向对象思想的具体表现**
- 离开了具体的计谋来谈兵法，兵法就虚无缥缈；离开了设计模式来谈面向对象也是如此
	- 设计模式使得面向对象的程序设计思想**便于描述和理解**
	- 讨论时程序架构时说“工厂模式”，如同讨论战局时说“美人计”或“围魏救赵”，一言即明
- 在简单的战局用了过于复杂，或者不适合的计谋，会适得其反；设计模式亦然
	- 用错了设计模式不至于“军败身死”，刚开始学习的同学**不妨多试**
	- 了解每种设计模式的优缺点、设计初衷和应用场景，然后就能恰到好处地使用它们了
	- 要达到这种“中庸”的境界，需要在[Github](https://github.com/)上不断地修炼

### 历史
- 1977/79年，建筑师-亚历山大(Christopher Alexander)编制了一本汇集建筑领域设计模式的书
- 1987年，肯特·贝克和沃德·坎宁安借鉴了亚历山大的设计模式思想，将其应用于Smalltalk GUI code generator中
- 1988年，埃里希·伽玛（Erich Gamma）在他的苏黎世大学**博士毕业论文**中开始将这种思想移植到软件开发中
- 1989/91年，James Coplien在也在利用相同的思想致力于C++的开发，并于1991年发表著作《Advanced C++ Programming Styles and Idioms》。
- 还是在1988年，Erich Gamma获得博士学位后去往了美国。在那里，他结识了Richard Helm, Ralph Johnson, John Vlissides。他们合作出版了：《设计模式：可复用面向对象软件的基础》（Design Patterns - Elements of Reusable Object-Oriented Software）。书中案例采用Java和C++编写。
- 在这之后，设计模式的思想很快传播开来并深刻影响了之后的软件开发领域。这四位作者也在软件开发领域里以**GoF**（“四人帮”，即**Gang of Four**）而闻名于世。有时，GoF也会用于代指《设计模式》这本书。
	- ![Design-Pattern-GOF](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-GOF.png)


## 原则

### 总原则
- **开放-封闭原则**，OCP(Open-Closed Principle)
- 该原则是1988年，Bertrand Meyer在著作《Object Oriented Software Construction》中最先提出的
- 原文是：Software entities should be open for extension,but closed for modification
- 翻译：**软件应该对（功能）扩展开放，对（代码）修改关闭**，模块应尽量在不修改原代码的情况下进行扩展。
- 具体的方法论：
	- 根据敏捷开发思想，在初次开发时我们不对功能扩展作超出当前需求的假设，只实现**刚好够用**的功能即可。
	- 对于新增功能，只新增代码，不修改代码。
	- 如果做不到，就选用合适的设计模式进行**重构**（Refactoring）
- 本原则是面向对象设计中“**可复用设计**”的基石，是最重要的原则，其它衍生原则是实现开闭原则的手段。

### 六大衍化原则

- **单一职责原则（Single Responsibility Principle，SRP）**
	- 设计目的单一的类，一个类只负责一个职责，把这个职责最到最好（紧内聚）
	- 如果一个类承担的职责过多，等于将这些职责耦合在一起
	- 这种耦合下，一个职责的变化可能会削弱这个类完成其它职责的能力，从而破坏原有设计
	- 若将多个职责分开，各自对应到一个单独的类，可以令一个职责的变化只会涉及到一个类，不及其余
	- 举例来说，MVC，数据模型/界面/业务逻辑，各自独立。
	- 再举一个栗子，简单的编程题（Python），循环逻辑/或逻辑/算法逻辑，各自独立。
	
			打印输出符合如下条件之一的100以内的自然数：
			1. 能被30整除
			2. 个位+十位=10
			3. 个位-十位=5

			Run:
			[0, 5, 16, 19, 27, 28, 30, 37, 38, 46, 49, 55, 60, 64, 73, 82, 90, 91]
			
			Code:
			funList = [lambda x: x%30==0,
			           lambda x: x%10+x/10==10,
			           lambda x: x%10-x/10==5]
			
			def testFun(i):
			    return any(fun(i) for fun in funList)
			
			print(filter(testFun, range(100)))
- **里氏替换原则（Liskov Substitution Principle，LSP）**
	- 1987年，Barbara Liskov（美国首位计算机女博士、第二位女图灵奖得主）在会议演讲中首先提出：
		- `Let q(x) be a property provable about objects x of type T`
		- `Then q(y) should be true for objects y of type S where S is a subtype of T`
	- 罗伯特·马丁（Robert Martin）对原文的解读：子类对象能够替换其基类对象被使用
	- 换言之：**子类必须能够替换基类，否则不应当设计其为子类**
	- 1994年，Liskov与周以真（Jeannette Wing）在1994年发表论文并提出的以上的里氏替换原则。
	- 周以真是著名的华裔女性计算机科学家，以定义“计算思维”闻名，曾任卡内基—梅隆大学计算机学院院长，微软研究院副总裁，2017年离职
- **依赖倒置原则（Dependency Inversion Principle，DIP）**
	- 高层模块不应该依赖于低层模块，两者都应该依赖于抽象（**接口，Interface**）
	- 抽象（接口）不应该依赖于细节；细节应该依赖于抽象（接口），谓之依赖倒置
- **接口隔离原则（Interface Segregation Principles，ISP）**
	- Client在调用一个庞大臃肿的接口时，会涉及一些实际不用的方法。Client不应该依赖于它不使用的方法
	- 将庞大臃肿的接口拆分成更小的和更具体的接口，这样Client能只调用他们感兴趣的方法
	- 将单个臃肿接口拆分成多个紧内聚接口，能更好地复用这些子接口，也便于重构、更改
- **组合重用原则（Composite/Aggregate Reuse Principle，CARP）**
	- 尽量使用组合，而不是使用继承来达到重用的目的。
	- 继承是一种紧耦合，不仅有遗产，还有债务
	- 组合：Has-A，继承：Is-A
- **最少知道原则（Least Knowledge Principle，LKP）**
	- 也叫迪米特法则（Law of Demeter，LoD），Only talk to your immediate friends
	- Demeter是希腊神话中司掌农业之神，她的爱女被冥王拐走，伤心欲绝，出此警言
	- 迪米特法则不希望类之间都建立直接联系，每个类应减少对其他类的依赖，以降低耦合
	- 过度应用迪米特法则会令系统中存在大量中介类，在一定程度上增加了系统的复杂度
	- 门面模式（Facade）和中介模式（Mediator），都是迪米特法则应用的例子

## 设计模式分类
- ![Design-Pattern-Summary.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Summary.jpg)

### 创建型（Creational）：为类如何创建实例对象提供指南
- 工厂方法模式（Factory Method）
	- Creates an instance of several derived classes
	- Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses
	- 定义一个创建目标对象的接口，由子类决定需要实例化哪一个类
	- 在工厂方法中完成目标对象的实例化
	- ![Design-Pattern-Factory.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Factory.jpg)
- 抽象工厂模式（Abstract Factory）
	- Creates an instance of several families of classes
	- Provide an interface for creating families of related or dependent objects without specifying their concrete classes
	- 提供一个接口，可以创建一系列对象，而不用指定对应的类
	- ![Design-Pattern-Abstract-Factory.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Abstract-Factory.jpg)
	- 比如ORM模型中，对不同的Provider（Sqlite3/MySql）创建一系列不同的实例对象（Connection/Cursor）
- 构建器模式（Builder）
	-  Separates object construction from its representation
	-  Separate the construction of a complex object from its representation so that the same construction processes can create different representations
	- 将一个复杂类的表示与其构造相分离，使得相同的构建过程能得到不同的表示
	- ![Design-Pattern-Builder.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Builder.jpg)
	- 比如，构建一个人，需要分别实例化头部、身体和四肢。BuildPartHead/BuildPartBody/BuildPartArm/BuildPartLeg方法实现在ConcreteBuilder类中，然后在Director::construct()方法中调用它们
- 原型模式（Prototype）
	- A fully initialized instance to be copied or cloned
	- Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype
	- 对象要支持copy方法，可以进一步区分深拷贝和浅拷贝
	- ![Design-Pattern-Prototype.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Prototype.jpg)
- 单例模式（Singleton）
	- A class of which only a single instance can exist
	- Ensure a class only has one instance, and provide a global point of access to it.
	- 保证一个类只有一个实例，并提供一个访问它的全局访问点
	- ![Design-Pattern-Singleton.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Singleton.jpg)
	- 实现时要注意：
		- 构造方法私有化，最好在类定义时加上Sealed，避免产生派生类
		- 添加静态字段，字段类型就是这个单例类
		- 添加静态构造方法，返回上述的静态字段
		- 分为饿汉模式和懒汉模式
	- 饿汉模式
		- 在定义静态字段时直接初始化
		- 这样类在初次被引用到时会被加载到DotNet Runtime，会完成实例化
		- 这种方法不存在线程竞争问题（Race导致出现多个实例）
	- 懒汉模式
		- 在静态方法中判断静态字段是否被初始化（如果没有就new一个），然后返回静态字段
		- 这种方法存在线程竞争问题，需要加锁
		
				lock(object)
				{
				    if (instance == null) 
				    {
				        instance = new Singleton()
				    }
				}
		- 为了Performance，需要双重锁定
		
				if (instance == null)
				{
				    lock(object)
				    {
				        if (instance == null) 
				        {
				            instance = new Singleton()
				        }
				    }
				}

### 结构型（Structural）：为如何设计类以形成更大的结构提供指南
- 适配器模式（Adapter）
	- 将一个类的接口转换成用户希望得到的另一种接口。
- 桥接模式（Bridge）
	- 将类的抽象部分和实现部分分离开来，使它们可以独立地变化。
- 组合模式（Composite）
	- 将对象组合成树形结构以表示“整体-部分”的层次结构，对单个对象和组合对象的使用具有一致性。
- 装饰模式（Decorator）
	- 动态给一个对象添加一些额外的职责。
- 外观模式（Facade）
	- 定义一个高层接口，为子系统中的一组接口提供一个一致的外观，即对外统一接口
- 享元模式（Flyweight）
	- 提供支持大量细粒度对象共享的有效方法。
- 代理模式（Proxy）
	- 为其他对象提供一种代理以控制这个对象的访问。

### 行为型（Behavioral）：为类之间交互以及分配责任提供指南
- 职责链模式（Chain of Responsibility）
	- 通过给多个对象处理请求的机会，减少请求发送者与接收者之间的耦合。
	- 将接收对象链接起来，在链中传递请求，直到有一个对象处理这个请求。
	- ![Design-Pattern-Chain-of-Responsiblity.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Chain-of-Responsiblity.jpg)
- 命令模式（Command）
	- 将一个请求封装为一个对象，从而可用不同的请求对客户进行参数化
	- 将请求排队或者记录请求日志，支持可撤销操作。
- 解释器模式（Interpreter）
	- 给定一种语言，定义它的文法表示，并定义一个解释器
	- 该解释器用来根据文法表示来解释语言中的句子。
- 迭代器模式（Iterator）
	- 提供一种方法来顺序访问一个聚合对象中的各个元素，而不需要暴露该对象的内部表示。
- 中介者模式（Mediator）
	- 用一个中介对象来封装一系列的交互。它使各对象不需要显式地相互调用，从而达到低耦合
	- 还可以独立地改变对象之间的交互，也就是将网状结构该为星形结构
- 备忘录模式（Memento）
	- 保存一个对象的某个状态，以便在适当的时候恢复对象
- 观察者模式（Observer）
	- 定义对象之间的一种一对多的依赖关系
	- 当一个对象的状态发生变化时，所有依赖于它的对象都得到通知并自动更新，用于：联动/发布和订阅
- 状态模式（State）
	- 允许一个对象在其内部状态改变时改变他的行为，状态变成类，例如：会员卡的状态（铜－银－金）
- 策略模式（Strategy）
	- 定义一系列算法，把它们一个个封装起来，并且使它们之间可相互替换，从而让算法可以独立于使用它的用户而变化。
- 模版方法模式（Template Method）
	- 定义一个操作中的算法骨架，而将一些步骤延迟到子类中。
- 访问者模式（Visitor）
	- 一种分离对象数据结构与行为的方法。
	- 通过这种分离，可达到一个被访问者动态添加新的操作而无需做其他修改的效果。
	- 适用于数据结构相对稳定，算法易变化的系统。

## 其它设计模式

### 简单工厂模式

### MVC模式