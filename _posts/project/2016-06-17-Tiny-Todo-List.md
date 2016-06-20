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