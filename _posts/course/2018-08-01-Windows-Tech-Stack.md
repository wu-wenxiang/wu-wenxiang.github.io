---
layout:         post
title:          MS Support Engineer养成手册
category:       course
description:    小结了MS Support Engineer的技术栈的体系结构，厘清基础，再分门别类
---

## 需要读什么书/文档
- 当然你可以去看MSDN/TechNet上的官方文档，因为书上的内容往往有些过时
- 但如果是一些涉及原理或者变化甚少的技术，看书还是比看文档要好，比如Debug技术、C/C++语言、Windows体系

## 核心（按推荐阅读顺序排序）
- Language
	- [C# 本质论](https://book.douban.com/subject/26966099/)，目前出到6.0，适合**入门**
	- [CLR via C#](https://book.douban.com/subject/26285940/)，可以说是**C#圣经**了，目前第4版
	- [The C Program Language](https://book.douban.com/subject/1139336/)，**C语言圣经**，大道至简的典范
	- [C Primer Plus](https://book.douban.com/subject/26792521/)，出到了第6版，更详细些
	- [C++ Primer](https://book.douban.com/subject/25708312/)，第5版，**C++圣经**，有人说TCPL才是C++圣经，别听他们的，没人看完那本书
	- [C++ Primer Plus](https://book.douban.com/subject/3542212/)，第6版，更多例子和练习，更厚
- Windows Internals
	- [Windows Sysinternals实战指南](https://book.douban.com/subject/27590761/)，Windows平台下最好的问题排查工具集
	- [深入解析Windows操作系统](https://book.douban.com/subject/25844377/)，中文版的只出到第6版，Win7/08R2
	- [Windows Internals](https://docs.microsoft.com/en-us/sysinternals/learn/windows-internals)，英文版出到了第7版，Win10/16
- Debug
	- [安装](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools)，[配置Symbol](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/symbol-path)
	- [入门文档](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/getting-started-with-windbg)，[进阶文档](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-using-windbg)
	- [软件调试](https://book.douban.com/subject/3088353/)，张银奎，史诗级作品，中压英系列
	- [Advanced .NET Debugging](https://www.xcode.me/book/dot-net-advanced-debug-book)，.NET调试圣经
	- [Windows高级调试](https://book.douban.com/subject/3781532/)，也是圣经啦 :-(

## 外延

### Windows Platform
- Windows Server
	- 戴有玮三卷，08R2，Server配置入门书籍
		- [Windows Server 2008 R2安装与管理](https://book.douban.com/subject/5450815/)
		- [Windows Server 2008 R2 Active Directory配置指南](https://book.douban.com/subject/5418964/)
		- [Windows Server 2008 R2网络管理与架站](https://book.douban.com/subject/5921486/)
	- 戴有玮三卷，Server2012
		- [Windows Server 2012系统配置指南](https://book.douban.com/subject/25901051/)
		- Windows Server 2012 R2 Active Directory配置指南
		- [Windows Server 2012网络管理与架站](https://book.douban.com/subject/25876180/)
- COM/COM+ 
- Windbg Extension
- Network
- Docker

### Language Framework
- JavaScript
- Node.js
- Powershell
- VBScript
- Python
	- [Learning Python](https://book.douban.com/subject/24878044/)，最好的入门书，清晰透彻，目前第5版，中译版出到第4版

### Dev Framework
- ASP.NET
- SQL Server
	- [SQL Server 2012实施与管理实战指南](https://book.douban.com/subject/21823753/)，国内无出其右，有幸与作者们做过同事……
- WCF
	- [Programming WCF Services](http://shop.oreilly.com/product/0636920032373.do)，目前第4版，中译版译得不好，推荐看原版
- VisualStudio
- TFS/VSTS
- Hardware
- IoT
- Azure
- Dotnet Core

### Application
- Sharepoint
- Dynamic
- Exchange
- Skype
- IE/Edge
- IIS/ARR
- MSMQ
- BizTalk