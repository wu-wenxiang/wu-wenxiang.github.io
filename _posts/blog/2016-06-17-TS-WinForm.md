---
layout:         post
title:          TS-WinForm
category:       blog
description:    小结了若干个WinForm程序的排查步骤和解决办法
---

## WinForm基础

## 常用工具

## 案例小结

### Red-X（红叉问题）
- 我在256321414050611中遇到了红叉问题。

	![RedX.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/1689ecbf60a940cc80452289d2d5df7d-RedX.png)

- 参考：
	- [MSDN Blog](https://blogs.msdn.microsoft.com/shawnhar/2010/11/22/winforms-and-the-big-red-x-of-doom/) 说明了此类问题的原因
	- 这篇[博客](https://oliversturm.com/2005/03/23/red-x/)给出了示例代码来重现问题。
- 排查：
	- 建议客户捕捉异常+添加日志
	- 客户添加异常处理代码后，问题出现频率减少，但依然时有发生。
	- 问题发生，抛出的异常是OutOfMemory Exception
	- 检查Dump，发现.NET版本是4.5，建议：
		- 升级.NET版本到4.5.2（4.5和4.5.1已经于2016年1月停止支持）
		- 如果问题重现，可以使用DebugDiag Hook TrackLeak DLL到exe程序中，然后设置规则，随着内存使用量上升捕捉若干个Dump，然后分析是否存在内存泄露。
	- 客户升级.NET到4.5.2版本后，问题解决。

### ServicePoint ConnectionLimit    
- Case: 234486419190611
- Dump中可以看到，进程中发起WCF请求，ConnectionLimit已满

		WCF Call
		--------
		Uri        http://188.188.188.188/xxxxx.svc
		SoapAction http://tempuri.org/xxxxx
		
		HttpWebRequest
		--------------
		Address                         0x000000000247b4c4
		Uri                             http://188.188.188.188/xxxxx.svc
		Verb                            POST
		ServicePoint                    http://188.188.188.188
		ServicePoint ConnectionLimit    2
		ServicePoint CurrentConnections 12
		Timeout                         00:00:59.9990000
		Duration?                       00:00:28.8380000

- 调大办法
	- [App.config](http://social.msdn.microsoft.com/forums/en-US/netfxnetcom/thread/1f863f20-09f9-49a5-8eee-17a89b591007)

			<?xml version="1.0" encoding="utf-8" ?> 
			<configuration> 
			 <system.net> 
			  <connectionManagement> 
			   <add address="*" maxconnection="100" /> 
			  </connectionManagement> 
			 </system.net> 
			</configuration>

	- [Programming](http://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)
	
			class AwesomeWebClient : WebClient
			{
			    protected override WebRequest GetWebRequest(Uri address)
				{
			    	HttpWebRequest req = (HttpWebRequest)base.GetWebRequest(address);
			    	req.ServicePoint.ConnectionLimit = 10;
			    	return (WebRequest)req;
			    }
			}
