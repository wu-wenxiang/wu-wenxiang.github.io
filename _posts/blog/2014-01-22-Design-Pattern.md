---
layout:         post
title:          Design Pattern摘录
category:       blog
description:    小结设计模式的What/Why/How，基于C#
---

## Overview
- 关联代码：[Github](https://github.com/wu-wenxiang/Training-DesignPattern-Public)

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
	- 根据敏捷开发原则，不要为代码添加基于猜测、实际不需要的功能，代码**刚刚够用**即可
	- 对于新增功能，只新增代码，不修改代码
	- 如果做不到，就选用合适的设计模式进行**重构**（Refactoring）
- 本原则是面向对象设计中“**可复用设计**”的基石，是最重要的原则，其它衍生原则是实现开闭原则的手段。

### 六大衍化原则

- **单一职责原则（Single Responsibility Principle，SRP）**
	- 设计目的单一的类，一个类只负责一个职责，把这个职责最到最好（紧内聚）
	- 如果一个类承担的职责过多，等于将这些职责耦合在一起
	- 这种耦合下，一个职责的变化可能会削弱这个类完成其它职责的能力，从而破坏原有设计
	- 若将多个职责分开，各自对应到一个单独的类，可以令一个职责的变化只会涉及到一个类，不及其余
	- 举例来说，MVC，数据模型/界面/业务逻辑，各自独立。
	- ![Example.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Example.jpg)
	- 简单的编程题（Python），循环逻辑/或逻辑/算法逻辑，各自独立。
	
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
	- 桥接（Bridge）模式
- **最少知道原则（Least Knowledge Principle，LKP）**
	- 也叫迪米特法则（Law of Demeter，LoD），Only talk to your immediate friends
	- Demeter是希腊神话中司掌农业之神，她的爱女被冥王拐走，伤心欲绝，出此警言
	- 迪米特法则不希望类之间都建立直接联系，每个类应减少对其他类的依赖，以降低耦合
	- 过度应用迪米特法则会令系统中存在大量中介类，在一定程度上增加了系统的复杂度
	- 门面模式（Facade）和中介模式（Mediator），都是迪米特法则应用的例子

## 设计模式详述

### 分类
- ![Design-Pattern-Summary.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Summary.jpg)
- 创建型（Creational）：为类如何创建实例对象提供指南
- 结构型（Structural）：为如何设计类以形成更大的结构提供指南
- 行为型（Behavioral）：为类之间交互以及分配责任提供指南

### 工厂方法模式（Factory Method）
- Creates an instance of several derived classes
- Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses
- 定义一个创建目标对象的接口，由子类决定需要实例化哪一个类
- 在工厂方法中完成目标对象的实例化
- ![Design-Pattern-Factory.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Factory.jpg)

### 抽象工厂模式（Abstract Factory）
- Creates an instance of several families of classes
- Provide an interface for creating families of related or dependent objects without specifying their concrete classes
- 提供一个接口，可以创建一系列对象，而不用指定对应的类
- ![Design-Pattern-Abstract-Factory.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Abstract-Factory.jpg)
- 比如ORM模型中，对不同的Provider（Sqlite3/MySql）创建一系列不同的实例对象（Connection/Cursor）

### 构建器模式（Builder）
-  Separates object construction from its representation
-  Separate the construction of a complex object from its representation so that the same construction processes can create different representations
- 将一个复杂类的构造过程（子部件的选择和构造顺序）与其子部件的构造过程分离
- ![Design-Pattern-Builder.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Builder.jpg)
- 比如，构建一个人，需要分别实例化头部、身体和四肢。BuildPartHead/BuildPartBody/BuildPartArm/BuildPartLeg方法实现在ConcreteBuilder类中，然后在Director::construct()方法中调用它们

### 原型模式（Prototype）
- A fully initialized instance to be copied or cloned
- Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype
- 对象要支持copy方法，可以进一步区分深拷贝和浅拷贝
- ![Design-Pattern-Prototype.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Prototype.jpg)

### 单例模式（Singleton）
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

### 适配器模式（Adapter）
- Match interfaces of different classes
- Convert the interface of a class into another interface clients expect
- Adapter lets classes work together that couldn’t otherwise because of incompatible interfaces
- 将一个类的接口转换成用户希望得到的另一种接口
- ![Design-Pattern-Adapter.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Adapter.jpg)
- 这个模式一般用于软件项目开发后期或者维护器，或者用于集成不同的Provider
- .NET中DataAdapter类，Fill方法将数据源里的数据同步到DataSet，Update方法将DataSet中的数据同步到数据源
- 适配器模式用于解决接口不匹配的问题，可类比扁鹊三兄弟。
	- 如果实现能定义好统一的接口，不匹配的问题就不会发生
	- 有小的接口不统一这类问题发生时，及时重构，问题不至于扩大
	- 只有碰到了无法改变原有设计和代码的情况时，才考虑适配
	- 事后补救不如事中控制，事中控制又不如事前规划
	- ![Bianque.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Bianque.jpg)

### 桥接模式（Bridge）
- Separates an object’s interface from its implementation
- Decouple an abstraction from its implementation so that the two can vary independently
- 将类的抽象部分和实现部分分离开来，使它们可以独立地变化
- ![Design-Pattern-Bridge.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Bridge.jpg)
- 组合重用原则（Composite/Aggregate Reuse Principle，CARP）
- Abstraction类的实例对象，可以调用成员方法SetImplementor来将ConreteImplementA或者B的实例对象替换掉，替换成任何一个实现了Implement接口的类的实例对象。
- C#客户端代码：
	
		Abstraction ab = new Abstraction();
		ab.SetImplementor(new ConcreteImplementA());
		ab.Operation();
		ab.SetImplementor(new ConcreteImplementB());
		ab.Operation();

### 组合模式（Composite）
- A tree structure of simple and composite objects
- Compose objects into tree structures to represent part-whole hierarchies
- Composite lets clients treat individual objects and compositions of objects uniformly.
- 将对象组合成树形结构以表示“整体-部分”的层次结构
- 组合模式令client能处理单个对象或组合对象时做到一视同仁
- ![Design-Pattern-Composite.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Composite.jpg)
- Leaf类中也有add/remove/getChild方法，但不做事即可。
- C#客户端代码：
	
		Composite root = new Composite("Root");
		root.add(new Leaf("Leaf A"));
		compX = new Composite("Composite X");
		root.add(compX);
		compX.add(new Leaf("Leaf B"));
		root.operation();

### 装饰模式（Decorator）
- Add responsibilities to objects dynamically
- Attach additional responsibilities to an object dynamically
- Decorators provide a flexible alternative to subclassing for extending functionality
- 动态给一个对象添加一些额外的职责。
- ![Design-Pattern-Decorator.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Decorator.jpg)
- 装饰器模式可以用于抽象一些Common的逻辑（MiddleWare），比如为所有Library方法统计时间（离开时间戳-进入时间戳），或者在进入和离开Library方法时记录日志
- C#客户端代码：
	
		ConcreteComponent c = new ConcreteComponent();
		ConcreteDecoratorA d1 = new ConcreteDecoratorA();
		ConcreteDecoratorB d2 = new ConcreteDecoratorB();
		d1.SetComponent(c);
		d2.SetComponent(d1);
		d2.Operation();
- ConcreteDecoratorA::Operation方法的实现
	
		base.Operation();
		AddedBehavior();

### 外观模式（Facade）
- A single class that represents an entire subsystem
- Provide a unified interface to a set of interfaces in a system
- Facade defines a higher-level interface that makes the subsystem easier to use
- 定义一个高层接口，为子系统中的一组接口提供一个一致的外观，即对外统一接口
- ![Design-Pattern-Facade.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Facade.jpg)
- 外观模式在开发初、中、后都可以应用：
	- 开发之初，应该有意识地分层，比如MVC，层与层之间用Facade模式
	- 开发中，子系统不断重构，越发复杂，此时Facade模式可以减少彼此间依赖
	- 开发末，类似Adapter作用，亦可用Facade模式

### 享元模式（Flyweight）
- A fine-grained instance used for efficient sharing
- Use sharing to support large numbers of fine-grained objects efficiently
- A flyweight is a shared object that can be used in multiple contexts simultaneously. The flyweight acts as an independent object in each context — it’s indistinguishable from an instance of the object that’s not shared
- 提供一个Pool，在需要实例对象时，检查Pool，如果有现成的就不new，直接使用，用完放回Pool中
- ![Design-Pattern-Flyweight.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Flyweight.jpg)
- Flyweight是次最轻量级拳击选手的意思，表示此模式能通过复用实例来减轻系统负担。

### 代理模式（Proxy）
- An object representing another object
- Provide a surrogate or placeholder for another object to control access to it.
- 为其他对象提供一种代理以控制这个对象的访问
- ![Design-Pattern-Proxy.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Proxy.jpg)
- WebService就是一种Proxy模式，WebReference就是其代理
- 与装饰品模式相比，Proxy模式中对目标类的伪装是静态（编译时）的
- 而Decorator是动态的，特别提供一个装饰类

### 职责链模式（Chain of Responsibility）
- A way of passing a request between a chain of objects
- Avoid coupling the sender of a request to its receiver by giving more than one object a  chance to handle the request
- Chain the receiving objects and pass the request along the chain until an object handles it
- 通过给多个对象处理请求的机会，减少请求发送者与接收者之间的耦合。
- 将接收对象链接起来，在链中传递请求，直到有一个对象处理这个请求。
- ![Design-Pattern-Chain-of-Responsiblity.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Chain-of-Responsiblity.jpg)
- 链中的对象自己也并不知道链的结构，但它们都有一个指向后继者的引用。
- C#客户端代码
	
		Handler h1 = new ConcreateHandler1();
		Handler h2 = new ConcreateHandler2();
		Handler h3 = new ConcreateHandler3();
		h1.SetSuccessor(h2);
		h2.SetSuccessor(h3);
		int[] requests = { 1, 2, 3, 4, 5 };
		foreach (int request in requests)
		{
		    h1.HandleRequest(request);
		}
- ConCreateHandler1::handleRequest代码
	
		if (request >= 0 && request < 10)
		{
		    // DoSomething
		}
		else if (succssor != null)
		{
		    successor.HandleRequest(request); // 转移到下一跳处理
		}

### 命令模式（Command）
- Encapsulate a command request as an object
- Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations
- 将一个Command Request封装为一个对象，Client接受这些命令请求对象作为参数
- 可以将Request对象放入队列、记录日志，以及支持可撤销操作
- ![Command.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Command.jpg)
- Command实例对象要设定其接收者Receiver，Invoker对象通过SetCommand收集Command对象
- C#客户端代码
	
		Reveiver r = new Receiver();
		Command c = new ConcreteCommand(r);
		Invoker i = new Invoker();
		i.SetCommand(c);
		i.ExecuteCommand();
- Invoker::ExecuteCommand代码
	
		command.Excute();
- ConcreteCommand::Excute方法
	
		receiver.Action();

### 解释器模式（Interpreter）
- A way to include language elements in a program
- Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language
- 给定一种语言，定义它的文法表示，并定义一个解释器
- 该解释器用来根据文法表示来解释语言中的句子
- ![Interpreter.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Interpreter.jpg)
- C#客户端代码
	
		Context context = new Context();
		TerminalExpression exp = new TerminalExpression();
		exp.Interpret(context);
- 每一个Expression类对应了一种文法规则
- Context类包含解释器之外的一些全局信息

### 迭代器模式（Iterator）
- Sequentially access the elements of a collection
- Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation
- 提供一种方法来顺序访问一个聚合对象中的各个元素，而不需要暴露该对象的内部表示。
- ![Iterator.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Iterator.jpg)
- Iterator对象的实例化参数是Aggregate对象。简言之，由列表生产迭代器
- Aggregate对象提供Count和Index接口，Iterator提供Next方法，内部有一个current字段，每次调用Next方法时current会减一。简言之，迭代器带游标，只迭代一次
- .NET中，迭代器对应的接口是IEumerator，包含Current属性，MoveNext方法，Reset方法
- foreach与while的对应关系
	
		foreach (string item in a) {}
		// 等同于
		IEnumerator<String> e = a.GetEnumerator();
		while (e.MoveNext()) {}

### 中介者模式（Mediator）
- Defines simplified communication between classes
- Define an object that encapsulates how a set of objects interact
- Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently
- 用一个中介对象来封装一系列的交互
- 它使各对象不需要显式地相互调用，从而达到松耦合，还可以独立地改变对象之间的交互
- 就是将网状结构该为星形结构
- ![Mediator.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Mediator.jpg)
- Mediator将Colleagues上之间的交互复杂性变成了中介者的复杂性
- 当系统中出现了多对多交互复杂的对象群时，先不要急着用中介者模式，而是要反思系统设计是否合理
- WinForm设计中，Form窗体上有多个Button/Label等对象，Button/Label对象之间的交互（事件传递）都是由Form窗体来作中介，而事件处理则是个Button/Label对象自己处理
- 中介者模式适用于如下场合
	- 一组定义良好但是通信方式复杂的对象，比如WebForm
	- 定制一个分布在多个类中的行为，而又不想生成太多子类

### 备忘录模式（Memento）
- Capture and restore an object's internal state
- Without violating encapsulation, capture and externalize an object’s internal state so that the object can be restored to this state later
- 保存一个对象的某个状态，以便在适当的时候恢复对象
- ![Memento.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Memento.jpg)
- 客户端代码
	
		GameRole hero = new Originator();
			
		CareTaker stateAdmin = new CareTaker();
		stateAdmin.Memento = hero.CreateMemento();
			
		hero.DoSomthing();
		hero.SetMemento(stateAdmin.Memento);
- Originator（发起人）通过CreateMemento方法创建一个备份，通过SetMemento方法从恢复到某个备份的内容
- Caretaker（管理者）负责保存好Mementor，但不能对备忘录内容进行操作或者检查
- Originator看到Mementor的宽接口，Caretaker只看到窄接口，只能将备忘录传递给其它对象

### 观察者模式（Observer）
- A way of notifying change to a number of classes
- Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically
- 定义对象之间的一种一对多的依赖关系
- 当一个对象的状态发生变化时，所有依赖于它的对象都得到通知并自动更新，用于：联动/发布和订阅
- ![Observer.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Observer.jpg)
- 其实就是Subject::Attach往列表里加Observer，detach是从列表里减，notify遍历列表，调用Observer.update()
- subjectState和observerState用于表示订阅对象/观察者的特征字段
- 在上述结构中，Subject依赖于Observer接口的update()方法
- 在.NET中，这种依赖关系可以不是Hardcode的，而是在客户端代码中动态指定

		ConcreteSubject s1 = new ConcreteSubject();
		ConcreteObserver o1 = new ConcreteObserver(s1);
		ConcreteObserver o2 = new ConcreteObserver(s1);
		
		s1.Update += new EventHandler(o1.DoThingA); // 事件 += 委托“实例”
		s1.Update += new EventHandler(o2.DoThingB);
		s1.SubjectState = "TestSubjectState";
		s1.Notify();
- ConcreteSubject类实现

		delegate void EventHandler(); // 委托是一个特殊的“引用方法”类
		class ConcreteSubject : Subject
		{
		    public event EventHandler Update; // 定义委托事件
		    public void Notify()
		    {
		        Update(); //事件当函数调用，会遍历运行其委托实例列表中所有元素
		    }
		}

### 状态模式（State）
- Alter an object's behavior when its state changes
- Allow an object to alter its behavior when its internal state changes
- The object will appear to change its class
- 允许一个对象在其内部状态改变时改变其行为，这个对象看起来像是改变了其类
- ![State.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-State.jpg)
- 应用场景：一个对象状态的判断逻辑表达式过于复杂的情况，将状态判断逻辑转移到表示不同状态的一系列类中
- C#客户端代码

		Context c = new Context(new ConcreteStateA());
		c.Request(); 
		c.Request();	
- Context::Request()

		state.Handle(this);
- ConcreteState::Handle(Context context)

		// doSomthing;
		context.state = new ConcreteStateA();
- Context实例只管Request，Context::Request()调用state的Handle
- 所有与状态相关的代码都实现在ConcreteState中，比如

		public override void handle (Context c)
		{
		    if (c.Hour < 12)
		    {
		        // doSomething; // 符合状态，做事
		    }
		    else
		    {
		        c.SetState(new NextState()); // 切换状态
		        c.handler(); // 做事
		    }
		}

### 策略模式（Strategy）
- Encapsulates an algorithm inside a class 
- Define a family of algorithms, encapsulate each one, and make them interchangeable
- Strategy lets the algorithm vary independently from clients that use it.
- 定义一系列算法，把它们一个个封装起来，使它们之间可相互替换
- 策略模式让算法的变化不会影响到使用算法的用户
- ![Strategy.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Strategy.jpg)
- 客户端只需要实例化Context一个类即可，对StrategyA/B类的实例化和excute调用都放在Context类的方法中
- 策略模式用组合，模版方法用继承

### 模版方法模式（Template Method）
- Defer the exact steps of an algorithm to a subclass
- Define the skeleton of an algorithm in an operation, deferring some steps to subclasses
- Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm’s structure
- 定义一个操作中的算法骨架，而将一些步骤延迟到子类中。
- 模版方法使得子嘞可以不改变一个算法的结构，即可重新定义改算法的某些特定步骤。
- ![Template-Method.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Template-Method.jpg)
- 策略模式用组合，模版用继承

### 访问者模式（Visitor）
- Defines a new operation to a class without change
- Represent an operation to be performed on the elements of an object structure
- Visitor lets you define a new operation without changing the classes of the elements on which it operates.
- 一种分离对象数据结构与行为的方法。
- 通过这种分离，可达到一个被访问者动态添加新的操作而无需做其他修改的效果。
- 适用于数据结构相对稳定，算法易变化的系统
- ![Visitor.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/d3982739435445939afcf1c492cddf08-Design-Pattern-Visitor.jpg)
- C#客户端代码

		class ObjectStructure
		{
		    public IList<Element> elements = new List<Element>();
		    
		    public void Accept(Visitor visitor)
		    {
		        foreach (Element e in elements)
		        {
		            e.Accept(visitor);
		        }
		    }
		}
		
		ObjectStructure o = new ObjectStructure();
		o.elements.add(new ConcreteElementA());
		o.elements.add(new ConcreteElementB());
		ConcreteVisitor1 v1 = new ConcreteVisitor1();
		ConcreteVisitor2 v2 = new ConcreteVisitor2();
		
		o.Accept(v1);
		o.Accept(v2);

## 附录：一些基础知识
- 对象是一个自包含实体，用一组可识别的特征和行为来标识
- 类是具有相同属性的对象的抽象
- Overload（重载）是指同一个类中，方法名相同而参数（类型或者个数）不同
- Override（重写）是指在父子类中，方法名和参数都相同的virtual方法
- 封装（Encapsulation）
	- 用于解耦合，令类的实例对象成自包含实体。
	- 清晰接口（不变）
	- 隐藏实现（变化）
- 继承（Inheritance）
	- is-A
	- 只有virutal方法可以被override
	- 构造方法不能被override
	- 子类方法调用父类方法，用base()，可以简写
	
			class B : A
			{
			    public B() : base()
			    {}
			}		
	- 继承增加了耦合，为了减少重复代码
- 多态（Polymorphism）
	- 多态的原理是，当virtual方法被调用时，无论对象是否被转换成其父类，都只有对象继承链最末端的方法会被调用
	- virtual方法是按照运行时的类型，而非编译时类型进行动态绑定调用的
- 抽象类
	- 抽象类必须被继承，不能实例化
	- 抽象方法，必须被重写（在继承时）
	- 抽象类是对类的抽象
- 接口
	- 接口时**公共**方法与属性的组合
	- 接口无字段，不实现，无static属性
	- 接口是对行为的抽象
- 装箱/拆箱
	- boxing，值类型 -> 引用类型
	- unboxing，引用类型 -> 值类型
	- 举例
	
			int i = 123;
			object o = (object)  // boxing
			o = 123;
			i = (int) o;  // unboxing
- 范型
	- collction：用于数据存储和检索的类，统称collection，包括stack/queue/list/hash
	- 范型是具有占位符（参数类型）的类、结构、接口和方法
	- ArrayList类似Python中的list
	- List<T>是ArrayList的范型等效类，如果类型不匹配，编译时就能检查出来
- 委托
	- 委托是对函数的封装
	- 举例
	
			public delegate void DelegateA();
			
			public void FunctionA()
			{
				// do something;
			}
			
			DelegateA f1 = new DelegateA(FunctionA);
			f1();
- 事件
	- 事件是委托的一种特殊形式
	- 事件时观察者模式Observor在.NET中的一种实现方式
	- 事件的写法：`public event DelegateA EventA;`，这表示
		- 事件发生时（调用`EventA();`）
		- 执行被委托的方法（EventA是一个List，`EventA += new DelegateA(FunctionA);`）
	- 如果在EventA被调用时传参数，可以
	
			public delegate void DelegateA(object sender, EventArgsA args);
			public event DelegateA EventA;
			
			public FunctionB()
			{
			    EventA(this, new EventArgs()); // 这两个参数会传给所有绑在EventA上的委托实例
			}