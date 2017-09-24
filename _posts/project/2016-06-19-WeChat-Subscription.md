---
layout:         post
title:          微信公众号-聊天机器人
category:       project
description:    一个微信公众号的项目原型（带聊天机器人）
---

## 项目简介
- 本项目参考[MyPlatform](https://github.com/littlecodersh/MyPlatform)，[Deploy Wiki](https://github.com/littlecodersh/MyPlatform/wiki/Deploy)。
- 该项目的源码托管在[Github](https://github.com/wu-wenxiang/wwxPOC)
- Demo：
	- ![qrcode_for_gh_6d13cd886e31_258.jpg](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/Common-9fa9e352b59943ecaeee659bec62c519-qrcode_for_gh_6d13cd886e31_258.jpg)
	- ![Wechat-Robot.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-Wechat-Robot.png)

## 准备工作
- **实名认证**过的 [SAE账号](http://sae.sina.com.cn/)（实名认证需要一些时间，3个工作日）
	- ![SAE-Auth.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-SAE-Auth.png)
	- ![SAE-Auth-OK.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-SAE-Auth-OK.png)
- [微信公众平台](https://mp.weixin.qq.com/)**个人**账号（其他类型其实也可以啦）
	- 准备好微信公众号之后，你已经可以扫描二维码进行关注了。只是还欠缺后台功能，这个公众号还不能跟你聊天 :)
	- ![Wechat-Sub-Init.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-Wechat-Subscription-Init.PNG)
- [图灵机器人](http://tuling123.com/) API Key，图灵官网获取（如果没有也没关系，空着就好，只影响自动回复）
	- ![Tuling123.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-Tuling123.png)

## 开始动手

### Wechat-SAE-Tuling
- 配置微信，启用**服务器配置**
	- ![Wechat-Sub-Controller.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-Wechat-Sub-Controller.png)
- 在SAE上新建应用，将代码上传到SAE
	- ![SAE-App-Controller.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-SAE-App-Controller.png)
- 部署完成后，应该可以访问站点：
	- ![SAE-Site-Root.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-SAE-Site-Root.png)
	- ![SAE-Site-Articles.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-SAE-Site-Articles.png)
- 部署完成后，要过几分钟才生效。如果不生效的话，可以打开SAE站点的实时Log，看微信有没有把请求发过来。
	- ![SAE-Realtime-Log-Show.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/c5b735854b244f629923cd3c4b2715cc-SAE-Realtime-Log-Show.png)
	- 上图中可以看到，第一次通过向微信公众号发请求时，微信公众号会发两条GET请求，随后每次请求都是POST。这些HTTP请求的顺序，方法类型，以及数据内容，都是微信开发者接口的一部分，你可以在[这里](http://mp.weixin.qq.com/debug/)调试确认你的后台服务是否满足这些借口。

### Wechat-个人站点-Tuling
- 接下来我们要研究怎么把SAE上的项目搬到我们自己的站点上来...
