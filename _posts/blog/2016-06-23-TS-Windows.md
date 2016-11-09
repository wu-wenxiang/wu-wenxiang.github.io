---
layout:         post
title:          TS-Windows-Platform
category:       blog
description:    介绍排查问题的若干简单有效的方法
---

## 简介

## 常用方法

### 重启进程/服务

### 重启系统

### 重新登录

### 安全模式登录

- Windows Server 2012
	- 参考：[这里](http://www.isunshare.com/windows-2012/boot-windows-server-2012-in-safe-mode.html)
	- 感谢**Li Ruoyu**童鞋的整理
		- Enable safe mode in Network option
		- System Configuration -> Boot -> Boot Options -> Safe Boot -> Network（这里如果选minimal是不带网络连接的）
		- Restart server and press “enter” to enter safe mode 
		- Refer to [Details](http://7xudfs.com1.z0.glb.clouddn.com/0f411cda89af489da677eaec9d2e3a5f-SafeModeNetworkTest.docx)