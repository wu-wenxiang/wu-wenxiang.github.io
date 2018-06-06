---
layout:         post
title:          Docker + ASP.NET Core 远程调试
category:       blog
description:    介绍 ASP.NET Core + Docker 在Win/Linux环境中的部署和源码级调试方法
---

## 开发
- 开发环境：VS2017，VSCode也可以
	- 本地：Win10 + Docker CE
	- 远端：Server 2016 或者 CentOS 7.4，Ubuntu 18.04 也行。
- [Code Demo](https://github.com/wu-wenxiang/Training-AspDotNetCore-Docker-Public/tree/master/Docker-AspDotNetCore/AspDotNetCore-WebApi-Docker-Linux-RemoteDebug)，放在Github
	- Test: `http://localhost:5000/api/values`
	- Return: `["value1","value2"]`
- 开发时，可以先正常创建ASP.NET或者ASP.NET Core项目，然后再添加Docker Support。Debug按钮后面使用`IISExpress`还是`Docker`，取决于Solution的`Start Project`是ASP.NET/ASP.NET Core项目，还是Docker-Composer。
- Dockerfile中，Asp.net的官方Base Image默认是`FROM microsoft/aspnetcore:2.0`，这里取决于`docker-compose.dcproj`文件，`DockerTargetOS `是Linux还是Windows，类似这样`<DockerTargetOS>Linux</DockerTargetOS>`。如果`DockerTargetOS`是Windows，默认是Nano，反之，是Debian9，Stretch。[参考](https://github.com/aspnet/aspnet-docker/tree/master/2.0)。注意，Latest images for 2.1 and newer are now available on [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/).

## 部署
- 如果不涉及Docker，Windows环境中可以使用 WebDeploy 推送到VM上，参考[官方文档](https://github.com/aspnet/Tooling/blob/AspNetVMs/docs/create-asp-net-vm-with-webdeploy.md)。
- 涉及Docker，推荐通过[Docker Hub](https://hub.docker.com/)，或者Private Registery Server。Docker Hub需要Create Account，**如果在创建用户时Signup按钮一直是灰色的不能点击，请翻墙**，XXX。
- 将Docker Image上传到[Docker Hub](https://hub.docker.com/)或者Private Hub后，可以在远程服务器（Win/Linux）上Pull下来Run，这样就完成了部署。
	- VS Deployment: Container Registry: Docker Hub
	- Publish
	- Then on CentOS, pull: `sudo docker pull maodouzi/aspdotnetcorewebapi: 20180605014624`, not latest
	- Then run: `sudo docker run -p 8080:80 -p 8081:22 -it maodouzi/aspdotnetcorewebapi:20180605014624`, remember open 8080/8081 ACL in Azure portal.
- 也可以将编译好的DLL发送到Remote Linux Server，在Linux Server上完成Docker Build工作，参考[这里](https://www.cnblogs.com/keepcodingforever/p/6698862.html)。

## 本地调试
- Win10 Docker CE非常方便，可以在Linux Container和Win Container之间切换，方法参考[这里](https://stackoverflow.com/questions/46779911/asp-net-core-docker-build-error)。
- Browser访问的起始路径在`docker-compose.dcproj`文件中定义：`<DockerServiceUrl>{Scheme}://localhost:{ServicePort}/api/values</DockerServiceUrl>`
- 参考：[官方视频教程](https://channel9.msdn.com/Events/Visual-Studio/Visual-Studio-2017-Launch/T115)
- 参考：[官方调试文档](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/visual-studio-tools-for-docker?view=aspnetcore-2.1)

## 远程调试
- 对Win-Docker，远程调试可以参考：[Github文档](https://github.com/riskfirst/debugging-aspnet-core-windows-docker)
- 对Linux-Docker环境，远程调试的原理是在Docker Container上开一个SSH端口，VS通过SSH端口Attach到对应的进程，借助[vs-debugger](https://aka.ms/getvsdbgsh)完成调试，参考[文档](https://github.com/Microsoft/MIEngine/wiki/Offroad-Debugging-of-.NET-Core-on-Linux---OSX-from-Visual-Studio)。
- 注意事项1：SSH端口，首先确保Docker Container上可以SSH登录；其次，VS上进行配置，先创建RemoteConnection, Tools > Options > Crossplatform > Connection Manager ，参考[StackOverflow](https://stackoverflow.com/questions/48661857/how-to-debug-a-net-core-app-runnig-in-linux-docker-container-from-visual-studio)。配置完成后，可以在VS上Attach Process，选Remote/SSH即可，参考[文档](https://blogs.msdn.microsoft.com/devops/2017/01/26/debugging-net-core-on-unix-over-ssh/)。
- 注意事项2：Portalable PDB，参考[文档](https://github.com/dotnet/core/blob/master/Documentation/diagnostics/portable_pdb.md)，在ASP.NET Core的.csproj文件里添加`<DebugType>portable</DebugType>`，参考[文档](https://github.com/OmniSharp/omnisharp-vscode/wiki/Portable-PDBs)。完整的写法见[Demo](https://github.com/wu-wenxiang/Training-AspDotNetCore-Docker-Public/tree/master/Docker-AspDotNetCore/AspDotNetCore-WebApi-Docker-Linux-RemoteDebug)，参考了[StackOverflow](https://stackoverflow.com/questions/48661857/how-to-debug-a-net-core-app-runnig-in-linux-docker-container-from-visual-studio)，针对SSH/Root/Passwd部分略作修改。
- 注意事项3：如果在设置断点时报错：`The breakpoint will not currently be hit. No symbols have been loaded for this document.`，参考[MSDN Forum](https://social.msdn.microsoft.com/Forums/sqlserver/en-US/1d06342e-4aca-45bc-bcb3-830bb20faff0/cant-remote-debug-azure-api-app?forum=vsdebug)，我选了"Enable .NET Framework Source Stepping"，从而自动Disable了"Enable Just My Code"，就可以远程调试了。
- 我的远程Docker主机在Azure的东亚服务器上，受限于距离和网速，远程调试起来非常慢，Sigh。
