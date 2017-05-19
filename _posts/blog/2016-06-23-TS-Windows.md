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
		- Refer to [Details](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/0f411cda89af489da677eaec9d2e3a5f-SafeModeNetworkTest.docx)

## 常见问题

### win2012系统使用Server manager无法安装.net 3.5
- Case: 875337419290611
- 本地尝试复现问题，未能复现
- 客户远程尝试复现问题，提示找不到源
- 注意到系统中为中文，但使用的源为英文版本
- 解压系统安装文件的中文源到客户环境中，重新安装，依然提示找不到源
- 将中文源与英文源合并，并重新安装成功
- 最终问题定位为客户使用的是英文安装包，但安装了中文的语言包，因此需要英文中文源均在线时才能正常安装。
