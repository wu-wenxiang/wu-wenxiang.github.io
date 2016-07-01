---
layout:         post
title:          TroubleShooting-Network Issue
category:       blog
description:    总结了应用程序在遇到网络问题时的排查思路和常见的案例分析
---

## 简介

## 常用工具和方法

### Network Monitor
- 常规的收集步骤
	1. Download & Install netmon from [Microsoft](http://www.microsoft.com/en-us/download/details.aspx?id=4865)
	1. Restart Machine 
	1. `md d:\mstest`
	1. `nmcap /network * /capture /file d:\mstest\MS_Date_Symptom.cap`
	1. Ctrl+C, collect logs.
- 原理
	- Netmon的抓包在NDIS层（The Netmon driver sits in NDIS which is a wrapper for filter drivers），是在TCP协议栈和NIC驱动之间
	
		![Netmon-Layer.jpg](http://7xudfs.com1.z0.glb.clouddn.com/9e7c39ba1fa54c17b394a1918e4a0f3d-Netmon-Layer.jpg)
		![Windows-OSI.jpg](http://7xudfs.com1.z0.glb.clouddn.com/9e7c39ba1fa54c17b394a1918e4a0f3d-Windows-OSI.jpg)

### Wireshark

### TcpDump

### Switch PortMirror

## 分析思路

## 案例小结

### Case 892443418260611
- Symptom
	- Windows 2012 Server，couldn't access a specific Web Service Endpoint
	- 能找到的所有的Windows 2012都不能访问这个Web Service
	- Web Service是第三方的，不能从Web Service端排查
	- 同一网段的所有Windows 2008R2 Server都可以正常访问这个Web Service，Win10也可以
- Analysis
	- 从2012上Ping WebService，可以ping通
	- 从2012上Telnet WebService的对应端口8080，可以telnet通
	- 从IE或者其它浏览器均不能正常访问
	- 将2012上的IE Document Mode设置为IE8（和2008R2上一样），也不能正常访问
	- 收集网络包，发现对端在3次握手后一直发EAS包，而不响应2012发出的Http Request

			3825       3:32:37 PM 6/29/2016    16.1241952          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:Flags=CE....S., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=0, Seq=2144566297, Ack=0, Win=65535 ( Negotiating scale factor 0x8 ) = 65535           {TCP:40, IPv4:20}
			3826       3:32:37 PM 6/29/2016    16.1255960          IEXPLORE.EXE    10.194.212.86       10.1.15.174       TCP                TCP:Flags=.E.A..S., SrcPort=HTTP Alternate(8080), DstPort=59897, PayloadLen=0, Seq=2689424323, Ack=2144566298, Win=14600 ( Negotiated scale factor 0x7 ) = 1868800   {TCP:40, IPv4:20}
			3827       3:32:37 PM 6/29/2016    16.1256945          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:Flags=...A...., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=0, Seq=2144566298, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920               {TCP:40, IPv4:20}
			3828       3:32:37 PM 6/29/2016    16.1261455          IEXPLORE.EXE    10.1.15.174         10.194.212.86     HTTP               HTTP:Request, GET /uservices/services/UTTSService, Query:wsdl               {HTTP:41, TCP:40, IPv4:20}
			3829       3:32:37 PM 6/29/2016    16.1564137          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[ReTransmit #3828]Flags=...AP..., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=288, Seq=2144566298 - 2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920             {TCP:40, IPv4:20}
			3847       3:32:37 PM 6/29/2016    16.2032276          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[ReTransmit #3828]Flags=...AP..., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=288, Seq=2144566298 - 2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920             {TCP:40, IPv4:20}
			3969       3:32:37 PM 6/29/2016    16.2969620          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[ReTransmit #3828]Flags=...AP..., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=288, Seq=2144566298 - 2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920             {TCP:40, IPv4:20}
			4023       3:32:38 PM 6/29/2016    16.4688686          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[ReTransmit #3828]Flags=...AP..., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=288, Seq=2144566298 - 2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920             {TCP:40, IPv4:20}
			4229       3:32:38 PM 6/29/2016    16.7970066          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[ReTransmit #3828]Flags=...AP..., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=288, Seq=2144566298 - 2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920             {TCP:40, IPv4:20}
			4313       3:32:38 PM 6/29/2016    17.1646334          IEXPLORE.EXE    10.194.212.86       10.1.15.174       TCP                TCP:Flags=.E.A..S., SrcPort=HTTP Alternate(8080), DstPort=59897, PayloadLen=0, Seq=2689424323, Ack=2144566298, Win=14600 ( Negotiated scale factor 0x7 ) = 1868800   {TCP:40, IPv4:20}
			4315       3:32:38 PM 6/29/2016    17.1646816          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:Flags=...A...., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=0, Seq=2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920               {TCP:40, IPv4:20}
			4448       3:32:39 PM 6/29/2016    17.4383078          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[ReTransmit #3828]Flags=...AP..., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=288, Seq=2144566298 - 2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920             {TCP:40, IPv4:20}
			4869       3:32:40 PM 6/29/2016    18.7188673          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[ReTransmit #3828]Flags=...AP..., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=288, Seq=2144566298 - 2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920             {TCP:40, IPv4:20}
			5100       3:32:40 PM 6/29/2016    19.1655610          IEXPLORE.EXE    10.194.212.86       10.1.15.174       TCP                TCP:Flags=.E.A..S., SrcPort=HTTP Alternate(8080), DstPort=59897, PayloadLen=0, Seq=2689424323, Ack=2144566298, Win=14600 ( Negotiated scale factor 0x7 ) = 1868800   {TCP:40, IPv4:20}
			5101       3:32:40 PM 6/29/2016    19.1656195          IEXPLORE.EXE    10.1.15.174         10.194.212.86     TCP                TCP:[Dup Ack #4315]Flags=...A...., SrcPort=59897, DstPort=HTTP Alternate(8080), PayloadLen=0, Seq=2144566586, Ack=2689424324, Win=8195 (scale factor 0x8) = 2097920               {TCP:40, IPv4:20}


	- 对比2008R2，发现08R2上没有这个现象

			19	3:48:54 PM 6/29/2016	4.3653216	iexplore.exe	10.1.15.119	10.194.212.86	TCP	TCP:Flags=......S., SrcPort=61821, DstPort=HTTP Alternate(8080), PayloadLen=0, Seq=2929399143, Ack=0, Win=8192 ( Negotiating scale factor 0x2 ) = 8192	{TCP:14, IPv4:13}
			20	3:48:54 PM 6/29/2016	4.3667548	iexplore.exe	10.194.212.86	10.1.15.119	TCP	TCP:Flags=...A..S., SrcPort=HTTP Alternate(8080), DstPort=61821, PayloadLen=0, Seq=1983542893, Ack=2929399144, Win=14600 ( Negotiated scale factor 0x7 ) = 1868800	{TCP:14, IPv4:13}
			21	3:48:54 PM 6/29/2016	4.3667794	iexplore.exe	10.1.15.119	10.194.212.86	TCP	TCP:Flags=...A...., SrcPort=61821, DstPort=HTTP Alternate(8080), PayloadLen=0, Seq=2929399144, Ack=1983542894, Win=32850 (scale factor 0x2) = 131400	{TCP:14, IPv4:13}
			22	3:48:54 PM 6/29/2016	4.3674762	iexplore.exe	10.1.15.119	10.194.212.86	HTTP	HTTP:Request, GET /uservices/services/UTTSService, Query:wsdl	{HTTP:15, TCP:14, IPv4:13}
			23	3:48:54 PM 6/29/2016	4.3703299	iexplore.exe	10.194.212.86	10.1.15.119	TCP	TCP:Flags=...A...., SrcPort=HTTP Alternate(8080), DstPort=61821, PayloadLen=0, Seq=1983542894, Ack=2929399427, Win=123 (scale factor 0x7) = 15744	{TCP:14, IPv4:13}
			24	3:48:54 PM 6/29/2016	4.3800516	iexplore.exe	10.194.212.86	10.1.15.119	HTTP	HTTP:HTTP Payload, URL: /uservices/services/UTTSService	{HTTP:15, TCP:14, IPv4:13}
			25	3:48:54 PM 6/29/2016	4.3800516	iexplore.exe	10.194.212.86	10.1.15.119	HTTP	HTTP:Response, HTTP/1.1, Status: Ok, URL: /uservices/services/UTTSService	{HTTP:15, TCP:14, IPv4:13}
	
	- 鉴于对端没有回复的现象，我们也希望客户端在下一跳交换机抓包，但是客户不方便执行此步操作。
		- 2012上发出的HTTP Request有没有发送到下一跳？
		- 如果发送到了，有没有服务端回复？
		- 如果服务端回复了，有没有发送到2012上？
	- 另一可能是网卡丢包
		- 客户测试了：带网络连接的安全模式启动2012依然无法访问WebService。这基本排除了第三方软件的影响
		- Netmon的抓包在NDIS层（The Netmon driver sits in NDIS which is a wrapper for filter drivers），是在TCP协议栈和NIC驱动之间，如果我们在网络包中看到这个请求，但是这个请求没有从网卡发出去；或者下一跳把回复发给2012，但是没有到Netmon，这只有2种可能：网卡有问题，或者，网卡驱动有问题
		- 但是因为：2012上的虚拟机2008R2可以正常访问站点（同一块网卡），所以，如果真是这种情况，只有可能是网卡驱动有问题，可以尝试更新驱动。鉴于2012上访问其他的Web Server或者Web Service可以正常访问，所以驱动有问题的可能性也很小。
	- 建议客户：
		- 下一跳抓包
		- 升级驱动
		- 禁用TCP SNPFeature（以admin权限打开cmd窗口，运行如下命令）：

				netsh interface tcp set global chimney=disabled 
				netsh interface tcp set global rss=disabled 
				netsh interface tcp set global autotuning=disabled
		
	- 实施后问题依旧，Review网络包，发现ECN位异常（对端一直发EAS而不是AS，停留在三次握手阶段），禁用ECN，问题不再重现。

- Conclusion
	-  在2012 Server上的一个TCP参数[ECN](https://tools.ietf.org/html/rfc3168#page-6)（显式拥塞通告），默认是Enable的。
	- 但在这个Case中，对端的Web Service服务器或者中间网络设备看起来对ECN的支持有问题（3次握手之后，对端还认为握手没有完成，持续发EAS），所以2012这边发了HTTP请求之后，没有回复。
	- 在2008R2上，ECN是默认关闭的，所以2008R2是可以正常访问Web Service的。
	- 使用命令：`netsh int tcp set global ecncapability=disabled`可以禁用2012上的ECN属性，参考[Technet](http://social.technet.microsoft.com/wiki/contents/articles/20204.how-to-enable-and-disable-explicit-congestion-notification-in-windows.aspx)。BTW，可以使用如下命令检查TCP各项属性：`Get-NetTCPSetting`
	- 2012 Server上禁用TCP ECN后问题解决。

