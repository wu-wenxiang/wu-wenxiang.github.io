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
	- 这个KB是为了修复一个WPAD漏洞，这个漏洞是在使用代理时有潜在的权限提升的可能，请参考文档：
		- [KB 3161949](https://support.microsoft.com/en-sg/kb/3161949)：The default behavior of PAC file download is changed so that the client's domain credentials are not automatically sent in response to an NTLM or Negotiate Authentication challenge when WinHTTP requests the PAC file. This occurs regardless of the value of the fAutoLogonIfChallenged flag that is specified in WINHTTP\_AUTOPROXY\_OPTIONS.
		- [Security Patch](https://technet.microsoft.com/library/security/MS16-077)：This security update resolves vulnerabilities in Microsoft Windows. The vulnerabilities could allow elevation of privilege if the Web Proxy Auto Discovery (WPAD) protocol falls back to a vulnerable proxy discovery process on a target system.
 	- 客户PAC代理脚本，在勾选上自动检测设置后，会偶尔出现不能访问外网的情况。
 	- 处理方法：
	 	- 不要勾选“自动检测设置”项
	 	- 在已安装更新中删除KB3161949补丁

