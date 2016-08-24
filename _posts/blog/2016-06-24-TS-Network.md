---
layout:         post
title:          TS-Network Issue
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

### TLS 1.2/1.1 Enable/Disable

TLS 1.2/1.1在08R2上默认是禁用的，在12R2上默认启用。
如果希望禁用它，改下注册表，重启就可以了。
参考：[BLog](https://blogs.msdn.microsoft.com/kaushal/2011/10/02/support-for-ssltls-protocols-on-windows/)，[Technet](https://technet.microsoft.com/en-us/library/dn786418%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396#BKMK_SchannelTR_TLS12)，[KB245030](https://support.microsoft.com/en-us/kb/245030)

		Windows Registry Editor Version 5.00
		
		[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1]
		
		[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server]
		"Enabled"=dword:00000000
		
		[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
		
		[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server]
		"Enabled"=dword:00000000

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

### Case: 080553410360611

### Case: 317234419170611
- 问题现象：
	- 客户端访问IIS Https站点正常
	- 手机端访问IIS Http站点正常，部分安卓收集访问Https站点下载不了apk文件
	- 第一台安卓手机端 + 内网，访问IIS站点下载文件，失败，同一台安卓手机端 + 外网（4G热点或者2G网络），访问IIS站点下载文件，能成功
	- 第二台安卓客户端使用Chrome/Oppo浏览器，无论内外网，访问IIS站点，均不能下载成功，但是同一台机器，同网络，UC浏览器可以下载成功
	- 第三台安卓客户端使用Chrome浏览器访问IIS-A站点，可以下载成功，使用同一台设备，同网络，同浏览器，访问另一个同构的IIS-B站点，不能下载成功
- 问题分析
	- IIS Log 200.0.995，WSA_OPERATION_ABORTED（995）：An overlapped operation was canceled due to the closure of the socket, or the execution of the SIO_FLUSH command in WSAIoctl. 参考 [MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/ms740668(v=vs.85).aspx)
	- 从网络包中，我们能验证1中的结论，确实是Socket意外关闭导致了传输apk文件失败。可以看到TLS 1.2 握手能正常完成，并开始传输数据。传输了约0.1秒后，IIS Server的对端（客户端）在收到了IIS发出的Application Data帧后，主动发了FIN/RST包，断开了TCP会话。
	- 考虑到在2008R2中TLS 1.2/1.1是默认禁用的，所以我们建议客户在2012R2上也禁用TLS1.2/1.1，看问题是否依然重现。测试发现，禁用TLS1.2后，问题依然重现，根据客户进一步测试发现，此问题在08R2上也有发生。并且正常环境中的TLS版本也是1.2。所以，看起来问题与TLS版本无关
	- 鉴于造成下载失败的直接原因是客户端主动断开了TCP 会话，所以我们建议您架设代理，并在代理上收集网络包：
		- 如果RST/FIN是从手机端发出，则需要您Involve手机设备相关工程师作进一步支持
		- 反之，需要请贵方网络工程师逐级排查网络包
- 结论
	- 因为握手可以完成，数据开始传输（双边都开始传输SSL Application Data），所以我没有考虑可能是证书问题。
	- 但更换证书后，问题确实解决了……
	- 下次遇到此类问题，**TLS降级**和**更换证书**都要试试。