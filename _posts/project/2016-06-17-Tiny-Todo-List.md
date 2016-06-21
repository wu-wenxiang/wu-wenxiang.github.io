---
layout:         post
title:          Tiny Todo List
category:       project
description:    Tiny Items Todo List
---

## DotNet

### DotNet CRL
- 问题现象
	- DotNet应用程序初次访问时较慢
	- 从Dump中可以看当时服务进程在等CRL(Certificate Revocation List) Check的返回
	- Netmon中可以印证Dump中看到的现象
- 问题需求
	- 找一个弱签名的Assembly，确认每次初始化JIT编译时会发生吊销证书列表检查（测试服务器不能连接外网，所以等15秒左右timeout）
	- 确认服务器端的IE选项中的Security：Check for publisher's certificate revocation选项是否对弱签名的.NET Assembly在JIT编译时是否去检查吊销证书列表有影响？
	- 除此之外，还有哪些方法可以避免此问题？
		- generatePublisherEvidence ?
		- Strong name signature
		- gpedit.msc -> Computer Configuration -> Windows Settings -> Security Settings -> Public Key Policies，双击打开右侧的"Certificate Path Validation Settings" -> Default Retrive Timeout Setting两个值设置为1 ?
		- 取消勾选"Automatically update certificates in the Microsoft Root Certificate Program" ?

### ASP.NET配置
- 已知
	- appConcurrentRequestLimit
		- Specifies the maximum number of requests that can be queued for an application. The default value is 5000.
		- Refer: [MSDN](https://msdn.microsoft.com/en-us/library/aa347568(v=vs.90).aspx)
	- maxConcurrentRequestsPerCPU
		- Specifies how many simultaneous requests ASP.NET allows per CPU.
		- Refer: [MSDN](https://msdn.microsoft.com/en-us/library/dd560842(v=vs.110).aspx)
		- 这里有另一个版本的processModel \\ [requestqueuelimit](https://msdn.microsoft.com/en-us/library/7w2sway1(v=vs.85).aspx)
	- requestqueuelimit
		- Specifies the maximum number of requests that can be queued for ASP.NET in a single process. When two or more ASP.NET applications run in a single application pool, the cumulative set of requests being made to any application in the application pool is subject to this setting.
		- Refer: [MSDN](https://msdn.microsoft.com/en-us/library/dd560842(v=vs.110).aspx)
- 猜想
	- requestqueuelimit是对进程而言，appConcurrentRequestLimit是对application而言
	- 两个阈值谁先到谁先得……
	- 但，processModel和applicationPool的requestqueuelimit，谁先起作用？
- 需求
	- 实验确认他们之间的区别和优先级
	- 如果超过阈值，会怎样？503？进入Queue？500.3？
	- 其它的参数又各自有什么区别？优先级？超过阈值会怎样？
		- [maxConcurrentThreadsPerCPU](https://msdn.microsoft.com/en-us/library/dd560842(v=vs.110).aspx)
		- [这里](https://technet.microsoft.com/en-us/library/dd425294(v=office.13).aspx)有关于requestQueueLimit到appConcurrentRequestLimit变迁
		- [参考1](http://m.jb51.net/article/36073.htm)
		- [参考2](http://www.cnblogs.com/dudu/archive/2009/11/10/1600062.html)
		- [参考3](http://stackoverflow.com/questions/612242/iis-7-0-503-errors-with-generic-handler-ashx-implementing-ihttpasynchandler)
		- [参考4](https://blogs.technet.microsoft.com/winserverperformance/2008/07/25/tuning-windows-server-2008-for-php/)