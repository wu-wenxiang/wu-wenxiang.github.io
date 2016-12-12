---
layout:         post
title:          当前MS问题常用的Hotfix和KB
category:       blog
description:    整理了一些当下常见的Hotfix和KB，会持续更新
---

## .NET

### Rollup Hotfix
- [KB 2974336](https://support.microsoft.com/en-us/kb/2974336)
	- .NET 4.5/4.5.1/4.5.2
	- OS: 08R2SP1/08SP2/Win7SP1/VistaSP2

## AD

### WPAD (Web Proxy Auto Discovery) 
- [KB 3161949](https://support.microsoft.com/en-sg/kb/3161949)
	- 问题：
		- 客户PAC代理脚本，在勾选上自动检测设置后，会偶尔出现不能访问外网的情况
		- 删除KB-3161949后问题解决
	- 这个KB是为了修复一个WPAD漏洞，这个漏洞是在使用代理时有潜在的权限提升的可能，请参考文档：
		- [The default behavior of PAC file download is changed so that the client's domain credentials are not automatically sent in response to an NTLM or Negotiate Authentication challenge when WinHTTP requests the PAC file. This occurs regardless of the value of the fAutoLogonIfChallenged flag that is specified in WINHTTP\_AUTOPROXY\_OPTIONS.](https://support.microsoft.com/en-sg/kb/3161949)
		- [This security update resolves vulnerabilities in Microsoft Windows. The vulnerabilities could allow elevation of privilege if the Web Proxy Auto Discovery (WPAD) protocol falls back to a vulnerable proxy discovery process on a target system.](https://technet.microsoft.com/library/security/MS16-077)
	- 处理方法：
		- 不要勾选“自动检测设置”项
		- 在已安装更新中删除KB3161949补丁

### TLS/SSL
- [KB 3163018](https://support.microsoft.com/en-us/kb/3163018)
	- 问题
		- 这个KB用于：Cumulative Update for Windows 10 version 1511 and Windows Server 2016 Technical Preview 4: June 14, 2016
		- 其中包含了[KB 3161639](https://support.microsoft.com/en-us/kb/3161639)：Update to add new cipher suites to Internet Explorer and Microsoft Edge in Windows

				# 新的加密套件顺序
				TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256
				TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P384
				TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256
				TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P384
				TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256
				TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P384
				TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256
				TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P384
				TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
				TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
				TLS_DHE_RSA_WITH_AES_256_CBC_SHA
				TLS_DHE_RSA_WITH_AES_128_CBC_SHA
				TLS_RSA_WITH_AES_256_GCM_SHA384
				TLS_RSA_WITH_AES_128_GCM_SHA256
				TLS_RSA_WITH_AES_256_CBC_SHA256
				TLS_RSA_WITH_AES_128_CBC_SHA256
				TLS_RSA_WITH_AES_256_CBC_SHA
				TLS_RSA_WITH_AES_128_CBC_SHA
				TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P384
				TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256
				TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P384
				TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384_P384
				TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256_P256
				TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256_P384
				TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P256
				TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P384
				TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P256
				TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P384
				TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
				TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
				TLS_DHE_DSS_WITH_AES_256_CBC_SHA
				TLS_DHE_DSS_WITH_AES_128_CBC_SHA
				TLS_RSA_WITH_3DES_EDE_CBC_SHA
				TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA
				TLS_RSA_WITH_RC4_128_SHA
				TLS_RSA_WITH_RC4_128_MD5
				TLS_RSA_WITH_NULL_SHA256
				TLS_RSA_WITH_NULL_SHA
				SSL_CK_RC4_128_WITH_MD5
				SSL_CK_DES_192_EDE3_CBC_WITH_MD5

				To configure the SSL Cipher Suite Order Group Policy setting, follow these steps:
				1.At a command prompt, enter gpedit.msc, and then press Enter. The Local Group Policy Editor is displayed.
				2.Go to Computer Configuration > Administrative Templates > Network > SSL Configuration Settings. 
				3.Under SSL Configuration Settings, select SSL Cipher Suite Order.
				4.In the SSL Cipher Suite Order pane, scroll to the bottom.
				5.Follow the instructions that are labeled How to modify this setting.

		- 此时如果服务器端不能支持TLS，可能会出现问题（协商的时候怎么没说好你不支持？到底谁的问题还不清楚。Case 811663414070611）
	- 解决办法
		- 删除这个KB
		- 或者在服务器端启用TLS
		- 删除这两个Cipher也可以

### IE

- KB 3197868, Case: 050469411211611
- 未安装补丁KB3197868时，可以获取到window.opener,但实际为空，因此逻辑与&&操作中后半部分不会再做判断，未抛出异常。但在您的代码逻辑中if{}代码块中的代码实际未运行，这一点与您的实际调试结果相符；
- 安装补丁KB3197868后，该补丁对跨域操作限制做了加强，禁止访问跨域是的window.opener，因此在作判断是抛出了异常“Permission denied”；
