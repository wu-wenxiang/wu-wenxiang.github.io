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
	- Case 604271417150611，只要在创建Console程序时选择without mainifest就可以了。
	- Console的外置Manifest（testConsole.exe.manifest）文件看起来像这样：

			<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
			<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0" >
			    <assemblyIdentity type="win32" name="ConsoleApplication24.exe" version="1.0.0.0" />
			    <dependency>
			        <dependentAssembly>
			            <assemblyIdentity type="win32" name="FlcnOptics.sxs" version="1.0.0.0"/>
			        </dependentAssembly>
			    </dependency>
			    <dependency>
			        <dependentAssembly>
			            <assemblyIdentity type="win32" name="FlcnHADS.sxs" version="1.0.0.0"/>
			        </dependentAssembly>
			    </dependency>
			</assembly>

	- 这些sxs文件（ADN_GraphicsModule.sxs.manifest）的内容看起来像这样：

			<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
			<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0" >
			     <assemblyIdentity type="win32" name="ADN_GraphicsModule.sxs" version="1.0.0.0" />
			     <file name="ADN_GraphicsModule.dll" > 
			        <comClass clsid="{9D28E5ED-88FC-4583-A5F7-B90187EEE3A9}"
			            threadingModel="apartment"
			            progid="SlbLgAplTM.ADNModule"
			            tlbid="{6DB51EBC-E4C5-4E48-80DE-C89A71221755}">
			            <progid>SlbLgAplTM.ADNModule.1.6</progid>
			        </comClass>
			        <typelib tlbid="{6DB51EBC-E4C5-4E48-80DE-C89A71221755}"
			            version="1.0"
			            helpdir="" />
			    </file>
			</assembly>

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

### 常见问题
- Case 604271417150611
	- 问题现象
		- The application has failed to start because its side-by-side configuration is incorrect. Please see the application event log or use the command-line sxstrace.exe tool for more detail
	- 排查思路
		- 多个sxs COM就有问题，单个就没有问题
		- Procmon，发现问题发生时，csrss.exe进程最后读取的manifest是什么（之后就报错退出），可以发现FlcnHADS有问题
		- 然后单独使用FlcnHADS，发现可以正常Work，于是怀疑他和某个dependence有冲突，然后通过折半排除法可以快速找到FlcnOptics
		- FlcnOptics和FlcnHADS同时存在时，此问题就会发生。两者单独出现，问题不出现。可以推测，这两个manifest应该存在冲突
		- 进一步检查发现：
			- 那两个文件里面有多个重复的定义，我们删除掉这些个重复CoClass定义后，问题解决
			- 但是，把那些重复的再加回来，也可以工作了，问题无法再现。如果，重起机器，问题可以再现
		- 这是因为CSRSS有一个Cache，以exe assembly的manifest文件的path和last modify time作为key，在logon session内Cache有效。
	- 结论
		- 这是因为sxs.manifest文件有误，修改后问题解决
- Case 572393411170611
	- Symptom: vstest.console.exe do not support side by side COM+ manifest
	- Analysis:
		- Steps to reproduce:
			- Prepare a side by side COM dll (testcom.dll & testcom.sxs.manifest), manifest contents as below:

					<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
					<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0" >
					     <assemblyIdentity type="win32" name="testcom.sxs" version="1.0.0.0" />
					     <file name="testcom.dll" > 
					        <comClass clsid="{A49DB898-9EFB-4BAF-A59B-7C9B3521BA1A}"
					            threadingModel="Neutral"
					            progid="HelloCOM.ProgID.1"
					            tlbid="{5477E1D7-54CF-478A-9128-02E1528BE67B}">
					            <progid>HelloCOM.ProgID.1.1</progid>
					        </comClass>
					        <typelib tlbid="{5477E1D7-54CF-478A-9128-02E1528BE67B}"
					            version="1.0"
					            helpdir="" />
					    </file>
					
					    <comInterfaceExternalProxyStub name = "IHelloCOM"
					        iid = "{3E199ABE-6551-4D2D-9FCD-BB03E5A9E6E6}"
					        proxyStubClsid32 = "{00020424-0000-0000-C000-000000000046}"
					        tlbid = "{5477E1D7-54CF-478A-9128-02E1528BE67B}" />
					</assembly>

			- Write a .NET DLL (test.dll) assembly with unittest, make sure create instance for the side-by-side COM object in the test cases.

					using Microsoft.VisualStudio.TestTools.UnitTesting;
					using System;
					using System.Runtime.InteropServices;
					using TestCOMLib;
					namespace TestCOMManifest
					{
					     [TestClass]
					     public class UnitTest1
					     {
					         [TestMethod]
					         public void TestMethod1()
					         {
					             Assert.IsNotNull((HelloCOM)Activator.CreateInstance(Marshal.GetTypeFromCLSID(new Guid("A49DB898-9EFB-4BAF-A59B-7C9B3521BA1A"))));
					         }
					     }
					}

			- Then call: `.\vstest.console.exe test.dll`, CreateInstance() call will fail due to cstest.console.exe couldn't find COM object A49DB898-9EFB-4BAF-A59B-7C9B3521BA1A in registry. vstest.console.exe don't know A49DB898-9EFB-4BAF-A59B-7C9B3521BA1A is a side-by-side COM object
			- If we `regsrv32` this test.dll, this test could pass

		- If we build a console EXE assembly without manifest, then add embed manifest to it with mt.exe (`mt.exe -manifest ".\vstest.console.exe.manifest" "-outputresource:.\vstest.console.exe;1"`), or use external manifest as below, the console EXE could run CreateInstance() successfully

				<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
				<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0" >
				    <assemblyIdentity type="win32" name="11111111" version="1.0.0.0" />
				    <dependency>
				        <dependentAssembly>
				            <assemblyIdentity type="win32" name="testcom.sxs" version="1.0.0.0"/>
				        </dependentAssembly>
				    </dependency>
				</assembly>

		- After confirm this issue, we
			- Raise a DTS to PG, to ask them if they could provide a private build vstest.console.exe. PG is pending discussion if they should remove manifest in vstest.console.exe in next release.
			- Suggestion:
				- If your test cases mainly focus on Activator.CreateInstance(<GUID>), to test if the sxs manifest for tescom correct or not, you can write an exe assembly to do this job, rather than using vstest.console.exe (Skip them in unit test cases temporarily)
				- If your test cases mainly focus on the functions in testcoms, you can use a script to reg them in setup()  & unreg them in teardown()
			- Customer also find a Work Around method for this issue:
				- Activate Context in .NET dll assembly to load manifest dynamically.

	- Solution
		- If PG confirm they would remove manifest in vstest.console.exe in next release, we’ll info you
		- You find a work around method to break fix this issue now
