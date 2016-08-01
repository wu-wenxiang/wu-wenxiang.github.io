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
- 参考[blog](https://weblog.west-wind.com/posts/2011/oct/09/an-easy-way-to-create-side-by-side-registrationless-com-manifests-with-visual-studio)
- Demo：
- 加载过程：
	- After ConsoleExeCS.exe launched, CreateProcess/CreateActCtx is called
	- CreateProcess/CreateActCtx does some validation, constructs a message, sends the message to CSRSS.exe, and waits for CSRSS.exe to return
	- CSRSS.exe would probe all the manifest files list in ConsoleExeCS.exe.manifest. This step cost the most time (10+ seconds)
	- After CSRSS.exe finished the probe jobs, it would cache all the manifest files to memory, so that no more probe-cost if you run ConsoleExeCS.exe later
- About the cache:
	- The cache key is the path of ConsoleExeCS.exe.manifest & its last modification time. 
	- So if you modified ConsoleExeCS.exe.manifest file, then re-run ConsoleExeCS.exe, CSRSS.exe will recheck all the sxs dlls’ manifest files. Besides, If you modified a sxs manifest file while keeping the ConsoleExeCS.exe.manifest file, the changes of sxs manifest will be ignored since CSRSS.exe would use caches instead of re-check all sxs manifest files.
	- The cache was host in the memory of the CSRSS.exe process, & each session has independent CSRSS.exe process, so cache couldn’t across the sessions
	- With machine reboot, CSRSS.exe process will also restart, the cache would lost in this case.
- About your questions: If there’s a way to delay that context validation & cached in coding way?
	- Answer:
		- I’m afraid the quick answer is no.
		- Because:
			- The context validation & caching logic would process before your application assembly images being loaded.
			- The context validation & caching logic was host in CSRSS.exe rather than you application. We could hook additional logics to OpenProcess/ CreateActCtx API, however we couldn’t remove the the original logics.
		- So, it seems the only way is building the caches before our customers running the ConsoleExeCS.exe

- 参考
	- [sxs-activation-context-activate-and-deactivate](https://blogs.msdn.microsoft.com/junfeng/2006/03/19/sxs-activation-context-activate-and-deactivate/)
	- [windows-vista-sxs-activation-context-cache](https://blogs.msdn.microsoft.com/junfeng/2007/10/01/windows-vista-sxs-activation-context-cache/)
	- [activation-context-creation-flow](https://blogs.msdn.microsoft.com/junfeng/2007/06/12/activation-context-creation-flow/)
	- [Find-out-why-your-external-manifest-is-being-ignored](http://csi-windows.com/blog/all/27-csi-news-general/245-find-out-why-your-external-manifest-is-being-ignored)
	- [AppCache](http://www.html5rocks.com/en/tutorials/appcache/beginner/)
