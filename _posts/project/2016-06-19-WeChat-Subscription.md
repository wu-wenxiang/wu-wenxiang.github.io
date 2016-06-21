---
layout:         post
title:          微信公众号POC
category:       project
description:    一个微信公众号的项目原型
---

## 项目简介
- 本项目参考[MyPlatform](https://github.com/littlecodersh/MyPlatform)，[Deploy Wiki](https://github.com/littlecodersh/MyPlatform/wiki/Deploy)。
- 该项目的源码托管在[Github](https://github.com/wu-wenxiang/wwxPOC)
- Demo：

	![qrcode_for_gh_6d13cd886e31_258.jpg](http://7xudfs.com1.z0.glb.clouddn.com/Common-9fa9e352b59943ecaeee659bec62c519-qrcode_for_gh_6d13cd886e31_258.jpg)
	![Wechat-Robot.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-Wechat-Robot.png)

## 准备工作
- **实名认证**过的 [SAE账号](http://sae.sina.com.cn/)（实名认证需要一些时间，3个工作日）

	![SAE-Auth.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-SAE-Auth.png)
	![SAE-Auth-OK.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-SAE-Auth-OK.png)

- [微信公众平台](https://mp.weixin.qq.com/)**个人**账号（其他类型其实也可以啦）

	准备好微信公众号之后，你已经可以扫描二维码进行关注了。只是还欠缺后台功能，这个公众号还不能跟你聊天 :)
	
	![Wechat-Sub-Init.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-Wechat-Subscription-Init.PNG)

- [图灵机器人](http://tuling123.com/) API Key，图灵官网获取（如果没有也没关系，空着就好，只影响自动回复）

	![Tuling123.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-Tuling123.png)

## 开始动手

### Wechat-SAE-Tuling

- 配置微信，启用**服务器配置**

	![Wechat-Sub-Controller.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-Wechat-Sub-Controller.png)

- 在SAE上新建应用，将代码上传到SAE

	![SAE-App-Controller.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-SAE-App-Controller.png)
	
	部署完成后，应该可以访问站点：
	
	![SAE-Site-Root.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-SAE-Site-Root.png)
	![SAE-Site-Articles.png](http://7xudfs.com1.z0.glb.clouddn.com/c5b735854b244f629923cd3c4b2715cc-SAE-Site-Articles.png)

	部署完成后，要过几分钟才生效。如果不生效的话，可以打开SAE站点的实时Log，看微信有没有把请求发过来。

### Wechat-个人站点-Tuling

接下来我们要研究怎么把SAE上的项目搬到我们自己的站点上来...
