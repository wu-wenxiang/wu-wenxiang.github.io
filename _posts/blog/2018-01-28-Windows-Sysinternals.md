---
layout:         post
title:          Windows-Sysinternals工具一句话简介
category:       blog
description:    一句话解释Sysinternals工具集中各个工具在Windows平台上的应用范围
---

Sysinternals工具集是Windows平台上排查问题的“瑞士军刀”，下文简单描述了各个工具的应用场景。

## 进程和诊断工具
- [Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer)：显示进程和线程的详细信息：父子关系，加载的DLL，打开的对象句柄（比如使用中的文件）
- [Autoruns](https://docs.microsoft.com/en-us/sysinternals/downloads/autoruns)：分组显示（系统启动时/用户登陆时/运行IE时）自动运行的软件，可以启用或禁用之
- [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)：实时记录文件系统、注册表、网络、进程、线程、Image加载活动的细节信息
- [Procdump](https://docs.microsoft.com/en-us/sysinternals/downloads/procdump)：为进程创建用户态Full内存转储，可以设定条件自动抓取
- [VMMap](https://docs.microsoft.com/en-us/sysinternals/downloads/vmmap)：显示进程虚拟和物理内存使用情况的详细信息
- [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)：监控本地或远程计算机生产的，用户态或内核态的调试输出信息
- [LiveKd](https://docs.microsoft.com/en-us/sysinternals/downloads/livekd)：无需重启至调试模式，针对运行中的本地系统或HyperV来宾系统快照运行标准的内核调试器，亦可对系统内存转储
- [ListDLLs](https://docs.microsoft.com/en-us/sysinternals/downloads/listdlls)：在控制台窗口中显示已载入系统的DLL详细信息
- [Handle](https://docs.microsoft.com/en-us/sysinternals/downloads/handle)：在控制台窗口中显示被系统中进程打开的对象句柄的相信信息

## PsTools
- [PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)：远程执行进程，或以LocalSystem身份执行进程并对输出结果重定向
- [PsFile](https://docs.microsoft.com/en-us/sysinternals/downloads/psfile)：远程列出或关闭打开的文件
- [PsGetSid](https://docs.microsoft.com/en-us/sysinternals/downloads/psgetsid)：显示计算机、用户、组、或服务等安全主体的SID
- [PsInfo](https://docs.microsoft.com/en-us/sysinternals/downloads/psinfo)：列出有关系统的信息
- [PsKill](https://docs.microsoft.com/en-us/sysinternals/downloads/pskill)：根据名称或PID终止进程
- [PsList](https://docs.microsoft.com/en-us/sysinternals/downloads/pslist)：列出有关线程和进程的详细信息
- [PsLoggedOn](https://docs.microsoft.com/en-us/sysinternals/downloads/psloggedon)：列出本地登陆或者通过远程连接登陆的账户
- [PsLogList](https://docs.microsoft.com/en-us/sysinternals/downloads/psloglist)：转储事件日志记录
- [PsPasswd](https://docs.microsoft.com/en-us/sysinternals/downloads/pspasswd)：为用户账户设置密码
- [PsService](https://docs.microsoft.com/en-us/sysinternals/downloads/psservice)：列出并控制Windows服务
- [PsShutdown](https://docs.microsoft.com/en-us/sysinternals/downloads/psshutdown)：关闭、注销本地或远程系统，或更改其电源状态
- [PsSuspend](https://docs.microsoft.com/en-us/sysinternals/downloads/pssuspend)：挂起和恢复进程

## 安全工具
- [SigCheck](https://docs.microsoft.com/en-us/sysinternals/downloads/sigcheck)：验证文件签名，查询Assembly版本和其它信息，通过VirusTotal.com查询恶意软件
- [AccessChk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk)：搜索为特定用户或组授予权限的对象，提供所授权限的详细信息
- [Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)：监视并报告系统活动，借此发现攻击行为
- [AccessEnum](https://docs.microsoft.com/en-us/sysinternals/downloads/accessenum)：搜索文件或注册表层次结构，找出权限可能有变化的位置
- [ShareEnum](https://docs.microsoft.com/en-us/sysinternals/downloads/shareenum)：枚举网络中的文件和打印机共享，并显示可以访问这些共享的人
- [ShellRunAs](https://docs.microsoft.com/en-us/sysinternals/downloads/shellrunas)：类似图形界面上的runas
- [Autologon](https://docs.microsoft.com/en-us/sysinternals/downloads/autologon)：配置用户账户在系统启动后自动登录
- [LogonSessions](https://docs.microsoft.com/en-us/sysinternals/downloads/logonsessions)：枚举计算机上活跃的的LSA登陆登陆回话
- [SDelete](https://docs.microsoft.com/en-us/sysinternals/downloads/sdelete)：安全的删除文件或目录结构，对硬盘所有未分配区域执行数据擦除操作

## AD工具
- [AdExplorer](https://docs.microsoft.com/en-us/sysinternals/downloads/adexplorer)：显示和编辑AD对象
- [AdInsight](https://docs.microsoft.com/en-us/sysinternals/downloads/adinsight)：追踪AD LDAP API调用
- [AdRestore](https://docs.microsoft.com/en-us/sysinternals/downloads/adrestore)：枚举并还原已删除的AD对象

## 桌面工具
- [BgInfo](https://docs.microsoft.com/en-us/sysinternals/downloads/bginfo)：在桌面墙纸上显示计算机配置信息
- [Desktops](https://docs.microsoft.com/en-us/sysinternals/downloads/desktops)：在单独的虚拟桌面上运行应用程序
- [ZoomIt](https://docs.microsoft.com/en-us/sysinternals/downloads/zoomit)：放大屏幕上显示的内容，并提供屏幕画面标注功能

## 文件工具
- [Strings](https://docs.microsoft.com/en-us/sysinternals/downloads/strings)：在文件中搜索ASCII或Unicode文本
- [Streams](https://docs.microsoft.com/en-us/sysinternals/downloads/streams)：找出包含可选数据流的文件系统对象，并删除这些数据流
- [Junction](https://docs.microsoft.com/en-us/sysinternals/downloads/junction)：列出/删除NTFS目录交接点
- [FindLinks](https://docs.microsoft.com/en-us/sysinternals/downloads/findlinks)：列出NTFS硬链接
- [DU](https://docs.microsoft.com/en-us/sysinternals/downloads/du)：列出目录层次结构的逻辑大小和磁盘空间占用大小
- [PendMoves](https://docs.microsoft.com/en-us/sysinternals/downloads/pendmoves)：列出计划在下一次重启系统时执行的文件操作
- [MoveFile](https://docs.microsoft.com/en-us/sysinternals/downloads/movefile)：安排在下一次重启系统时所要执行的文件操作

## 磁盘工具
- [Disk2Vhd](https://docs.microsoft.com/en-us/sysinternals/downloads/disk2vhd)：针对物理磁盘创建VHD映像
- [Sync](https://docs.microsoft.com/en-us/sysinternals/downloads/sync)：将磁盘缓冲未写入的改动写入物理磁盘
- [DiskView](https://docs.microsoft.com/en-us/sysinternals/downloads/diskview)：以图形化的方式显示卷中每个簇的映射信息，每个簇保存了哪些文件，特定文件使用了哪些簇
- [Contig](https://docs.microsoft.com/en-us/sysinternals/downloads/contig)：对特定文件进行碎片整理，或显示特点文件的碎片化程度
- [DiskExt](https://docs.microsoft.com/en-us/sysinternals/downloads/diskext)：显示有关磁盘范围的信息
- [LDMDump](https://docs.microsoft.com/en-us/sysinternals/downloads/ldmdump)：显示逻辑磁盘管理器LDM数据库中有关动态磁盘的详细信息
- [VolumeID](https://docs.microsoft.com/en-us/sysinternals/downloads/volumeid)：更改卷ID

## 网络和通信工具
- [PsPing](https://docs.microsoft.com/en-us/sysinternals/downloads/psping)：测量TCP/UDP数据包的单向和轮询时间、延迟，以及带宽
- [TCPView](https://docs.microsoft.com/en-us/sysinternals/downloads/tcpview)：列出活跃的TCP和UDP端点
- [Whois](https://docs.microsoft.com/en-us/sysinternals/downloads/whois)：查看互联网域名注册信息，或执行反向DNS查找

## 系统信息工具
- [RAMMap](https://docs.microsoft.com/en-us/sysinternals/downloads/rammap)：物理存储使用情况的详细视图
- [RU](https://docs.microsoft.com/en-us/sysinternals/downloads/ru)：列出所选注册表键的注册表空间使用情况
- [CoreInfo](https://docs.microsoft.com/en-us/sysinternals/downloads/coreinfo)：查看处理器和Windows系统是否支持各种功能：如不可执行内存页。显示逻辑处理器与核心、处理器插槽、NUMA节点、处理器组之间的映射关系
- [WinObj](https://docs.microsoft.com/en-us/sysinternals/downloads/winobj)：显示Windows对象管理器名称空间
- [LoadOrder](https://docs.microsoft.com/en-us/sysinternals/downloads/loadorder)：显示Windows加载各类设备驱动并启动不同服务的大致顺序
- [PipeList](https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist)：列出正在侦听的命名管道
- [ClockRes](https://docs.microsoft.com/en-us/sysinternals/downloads/clockres)：显示系统时钟的当前、最大和最小精度

## 其它工具
- [RegJump](https://docs.microsoft.com/en-us/sysinternals/downloads/regjump)：启动regedit，并定位至指定的reg path
- [Hex2Dec](https://docs.microsoft.com/en-us/sysinternals/downloads/hex2dec)：十六进制和十进制的双向转换
- [RegDelNull](https://docs.microsoft.com/en-us/sysinternals/downloads/regdelnull)：搜索/删除名称中包含嵌入NUL字符的注册表键
- [Ctrl2Cap](https://docs.microsoft.com/en-us/sysinternals/downloads/ctrl2cap)：将Caps Lock键击事件转换成Control键击事件