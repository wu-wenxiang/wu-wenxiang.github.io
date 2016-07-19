---
layout:         post
title:          稀奇古怪的技术
category:       course
description:    专门收集一些稀奇古怪的技术，说不定有用呢 :)
---

## VisualStudio-DSL

### 简介
- DSL是Domain Specific Language的缩写，功能与UML相似，可以通过图形界面快速完成业务逻辑设计，然后自动生成代码。
- DSL和UML的区别在于：
	- DSL需要你自己设计，UML国际通用
	- DSL只针对某类具体问题，UML for general purpose
	- VisualStudio DSL的参考在：[这里](https://msdn.microsoft.com/en-us/library/bb126259.aspx)

### 快速入门
- 入门项目可以参见：[这里](https://msdn.microsoft.com/en-us/library/ee943825.aspx?f=255&MSPPError=-2147217396)

### DSL问题排查
- Case 103092415160611
	- Symptom: VS2015-DSL-Toolbox-不显示图标
	- 比较图标，发现用户制作的图标是32×32的，Demo中16×16的（online icon generator）
		- http://www.picresize.com/
		- http://resizeimage.net/
	- 换成16×16的图标，可以正常显示

## AppFabric

### 简介
- 参考MSDN：[Introducing Windows Server AppFabric](https://msdn.microsoft.com/en-us/library/ee677312%28v=azure.10%29.aspx?f=255&MSPPError=-2147217396)，AppFabric是Web服务和中间件的缓存，托管和管理工具。
- 如果用到了HTTP Bind，必须IIS，否则AppFabric只要WAS服务即可（WCF），可以不用IIS。
- AppFabric通过IIS Extension的方式来实现管理功能。
- [Fast QA](http://social.technet.microsoft.com/wiki/contents/articles/609.appfabric-faq-hosting.aspx)
- Prem cases pass through to US now.

## Side By Side COM

### 简介

