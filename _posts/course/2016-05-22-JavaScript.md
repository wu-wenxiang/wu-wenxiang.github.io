---
layout:         post
title:          Learning JavaScript-ES6-尚未完成
category:       course
description:    涵盖ES6标准的JavaScript入门教程
---

## JavaScript简介

### 为什么要学习JavaScript？

1. 在Web世界里，只有JavaScript能跨平台、跨浏览器驱动网页，与用户交互。
	
	Flash背后的ActionScript曾经流行过一阵子，不过随着移动应用和HTML5的崛起，没有人用Flash开发手机App，它目前已经边缘化了。
	
1. 随着HTML5在PC和移动端越来越流行，JavaScript变得更加重要，新的JS框架层出不穷。
		
	![JS框架](http://7xudfs.com1.z0.glb.clouddn.com/171e26bd0d104c00857f967cbeb77fbc-JSFramworkList.png)
		 
1. 新的Web前后端模型

	![Web前后端模型](http://7xudfs.com1.z0.glb.clouddn.com/171e26bd0d104c00857f967cbeb77fbc-WebFrontendBackendModel.png)

1. 新兴的Node.js把JavaScript引入到了服务器端，使JavaScript变成了名副其实的全栈型语言。

	![Meteor框架](http://7xudfs.com1.z0.glb.clouddn.com/171e26bd0d104c00857f967cbeb77fbc-MeteorFramework.png)

### JavaScript语言学习的误区和注意事项
1. JavaScript一度被认为是一种玩具编程语言
	
	它有很多缺陷，所以不被大多数后端开发人员所重视。
	很多人认为，写JavaScript代码很简单，并且JavaScript只是为了在网页上添加一点交互和动画效果。

1. JavaScript易学而难精

	JavaScript确实很容易上手，但其精髓却不为大多数开发人员所熟知。编写高质量的JavaScript代码更是难上加难。

1. JavaScript是Web开发工程师必须精通的语言

	一个合格的Web开发人员应该精通JavaScript和其他编程语言。如果你已经掌握了其他编程语言，或者你还什么都不会，请立刻开始学习JavaScript，不要被Web时代所淘汰。

## JavaScript的简介

### JavaScript的历史

在上个世纪的1995年，当时的网景公司正凭借其Navigator浏览器成为Web时代开启时最著名的第一代互联网公司。

由于网景公司希望能在静态HTML页面上添加一些动态效果，于是叫Brendan Eich这哥们在两周之内设计出了JavaScript语言。你没看错，这哥们只用了10天时间。

为什么起名叫JavaScript？原因是当时Java语言非常红火，所以网景公司希望借Java的名气来推广，但事实上JavaScript除了语法上有点像Java，其他部分基本上没啥关系。

### ECMAScript

因为网景开发了JavaScript，一年后微软又模仿JavaScript开发了JScript，为了让JavaScript成为全球标准，几个公司联合ECMA（European Computer Manufacturers Association）组织定制了JavaScript语言的标准，被称为ECMAScript标准。

所以简单说来就是，ECMAScript是一种语言标准，而JavaScript是网景公司对ECMAScript标准的一种实现。

那为什么不直接把JavaScript定为标准呢？因为JavaScript是网景的注册商标。

不过大多数时候，我们还是用JavaScript这个词。如果你遇到ECMAScript这个词，简单把它替换为JavaScript就行了。

### JavaScript的版本

JavaScript语言是在10天时间内设计出来的，虽然语言的设计者水平非常NB，但谁也架不住“时间紧，任务重”，所以，JavaScript有很多设计缺陷，我们后面会慢慢讲到。

此外，由于JavaScript的标准——ECMAScript在不断发展，最新版ECMAScript 6标准（简称ES6）已经在2015年6月正式发布了，所以，讲到JavaScript的版本，实际上就是说它实现了ECMAScript标准的哪个版本。

由于浏览器在发布时就确定了JavaScript的版本，加上很多用户还在使用IE6这种古老的浏览器，这就导致你在写JavaScript的时候，要照顾一下老用户，不能一上来就用最新的ES6标准写，否则，老用户的浏览器是无法运行新版本的JavaScript代码的。

不过，JavaScript的核心语法并没有多大变化。我们的教程会先讲JavaScript最核心的用法，然后，针对ES6讲解新增特性。

### 兼容性列表

1. [ES各版本兼容性列表](http://kangax.github.io/compat-table/es6/)
2. [综合兼容性列表: caniuse](http://caniuse.com/)

## 快速入门

### 基本语法

### 数据类型和变量

### 字符串

### 数组

### 对象

### 条件判断

### 循环

### Map和Set

### Iterable

## 函数

### 函数的定义和调用

### 变量作用域

### 方法

### 高阶函数（函数式编程）

### 闭包

### 箭头函数

### 生成器（generator）

## 标准对象

### Date

### RegExp

### JSON

## 面向对象编程

### 创建对象

### 原型继承

### Class继承

## 浏览器

### 浏览器对象

### 操作DOM

### 操作表单

### 操作文件

### AJAX

### Promise

### Canvas

### Workers

## JQuery

### 选择器

### 操作DOM

### 事件

### 动画

### AJAX

### 扩展

## underscore

### Collections

### Arrays

### Functions

### Objects

### Chaining

## MVVM框架

## MVC框架

## Node.JS

### 安装Node.JS和npm

### QuickStart

### 模块

### 基本模块

### Web开发

### 自动化工具