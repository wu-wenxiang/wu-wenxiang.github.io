---
layout:         post
title:          万矿WindQuant入门
category:       blog
description:    介绍量化投资和万矿WindQuant的一些基本知识
---

## 简介

### 量化投资简介
- 投资大致有两种方法：基本面选股和量化投资
	- **基本面选股**：主要依靠自己在实盘操作的多年经验结合对影响市场价格客观因素的分析来判断并且进行投资
	- **量化投资**：通过模型对整个市场进行扫描，然后根据数据进行投资。
	- 量化投资主要依靠计算机来为投资者处理数据，而基本面选股主要依靠人来进行数据的挖掘、调研以及选股。前者偏重**定量**分析，后者则偏重**定性**分析。
- 主要量化平台：
	- 万矿WindQuant
	- 优矿，通联数据的量化平台
	- 聚宽JoinQuant
	- RiceQuant
	- 掘金量化平台
	- 恒生电子的量化社区
- Python API接口：
	- 万得Wind
	- 同花顺iFinD
	- QuickFix
- 开源回测系统：
	- Zipline
	- pyalgotrade
	- QuantDigger
	- 水木社区上的pyctp
- 数据获取：
	- TuShare财经数据接口包
	- Yahoo Finance的数据
	- 新浪js服务器上的股票数据
	- 雪球的json实时数据，cvs日线数据
	- 搜狐网易
	- 各种行情软件后天下载

### 万矿WindQuant简介
- WindQuant（万矿），是Wind（万得）旗下的**量化分析**平台，支持在线编程。 
- WindQuant主要支持2类操作
	- API数据获取
		- 内容涵盖：股票、债券、基金、衍生品、指数、宏观行业等各类金融市场
		- ![Wind-API-Catalog](http://5b0988e595225.cdn.sohucs.com/images/20171214/f62f4e2614e140f7993f0476b6e6ee60.jpeg)
		- 除此之外，万矿还提供500+因子的量化因子库，全方位量化股票以及指数的多种维度
		- ![Wind-API-量化因子](http://5b0988e595225.cdn.sohucs.com/images/20171214/47eb525deb4646b286dab6e9d5c4dc92.jpeg)
	- WindAlgo策略回测框架
		- 使用该回测框架，可对多因子选股策略、技术面策略、日内择时交易策略进行快速回测，使用简单方便
		- WindAlgo提供了很多方便快速回测的函数，例如：快速下单，多因子选股等

## 万矿入门

### Register & Overview
- 站点：[https://www.windquant.com/qntcloud/](https://www.windquant.com/qntcloud/)
- ![WindQuant-Register.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindQuant-Register.png)
- ![WindQuant-Overview.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindQuant-Overview.png)

### 在线编程
- Hello, world
- ![WindQuant-Debug.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindQuant-CodeBlock.png)
- Debug
- ![WindQuant-Debug.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindQuant-Debug.png)
- 帮助文档
	- [Github](https://github.com/WindQuant/Official/tree/master/WAPIWrapper/WAPIWrapperPython)
	- [帮助文档](http://www.dajiangzhang.com/document)  

### 测试模版案例
- 基于MA均线的回测

		from WindAlgo import * #引入回测框架
		import talib as ta
		
		def initialize(context):#定义初始化函数
		    context.capital = 1000000 #回测的初始资金
		    context.securities = ["600519.SH"] #回测标的
		    context.start_date = "20150101" #回测开始时间
		    context.end_date = "20170501" #回测结束时间
		    context.period = 'd' #策略运行周期, 'd' 代表日, 'm'代表分钟
		
		def handle_data(bar_datetime, context, bar_data):#定义策略函数
		    his= bkt.history('600519.SH',30) #使用history函数获取近期历史行情
		    ma5=ta.MA(np.array(his.get_field('close')), timeperiod=5, matype=0) #使用talib技术指标库计算5日均线
		    ma20=ta.MA(np.array(his.get_field('close')), timeperiod=20, matype=0)
		    position=bkt.query_position() #查询当前持仓
		    if('600519.SH' in position.get_field('code')): #如果当前持仓中有600519.SH
		        if(ma5[-1]<ma20[-1]): #如果5日均线下穿20日均线，则卖出所有股票
		            bkt.batch_order.sell_all()
		    else: #如果当前持仓中没有600519.SH
		        if(ma5[-1]>ma20[-1]): #如果5日均线上穿20日均线，则买入股票
		            res = bkt.order('600519.SH', 4000, 'buy')

		bkt = BackTest(init_func = initialize, handle_data_func=handle_data) #实例化回测对象
		res = bkt.run(show_progress=True) #调用run()函数开始回测,show_progress可用于指定是否显示回测净值曲线图
		nav_df=bkt.summary('nav') #获取回测结果
- 效果
- ![WindQuant-回测](http://5b0988e595225.cdn.sohucs.com/images/20171214/11bc8ab7f3aa4eea92c8a2e18df4bd05.gif)

### API取值
- 在使用API之前，必须先加载`WindPy`，然后执行`w.start()`启动API接口。

		from WindPy import *
		w.start() 
- ![API获取](https://www.windquant.com/qntcloud/ftp/image/20180531/a50c3d1c-4f2a-43c9-9307-a8461d2d9a6e.png)
- 然后粘贴过来

		from WindPy import *
		w.start()
		
		df = w.wsd("000001.SZ", "open,high,low,close", "2016-01-01", "2016-12-30", "")
		df
		
		## Output
		.ErrorCode=0
		.RequestID=756
		.Codes=[000001.SZ]
		.Fields=[OPEN,HIGH,LOW,CLOSE]
		.Times=[20160104,20160105,20160106,20160107,20160108,20160111,20160112,20160113,20160114,20160115,...]
		.Data=[[12.0,11.27,11.42,11.41,11.21,11.0,10.83,10.89,10.59,10.66,...],[12.03,11.57,11.56,11.41,11.29,11.08,10.91,10.94,10.8,10.8,...],[11.23,11.15,11.39,10.91,10.9,10.68,10.64,10.7,10.48,10.42,...],[11.33,11.4,11.53,10.94,11.12,10.76,10.81,10.71,10.77,10.46,...]]
- 使用Pandas对数据进行处理

		from WindPy import *
		w.start()
		wsd_data=w.wsd("000001.SZ", "open,high,low,close,volume,amt,chg,pct_chg", "2016-11-01", "2017-01-03", "")
		print(wsd_data)
		
		from pandas import DataFrame 
		data_df = DataFrame(wsd_data.Data,columns=wsd_data.Times,index=wsd_data.Fields)
		data_df=data_df.T #转置数据表
		data_df.sort(['OPEN'], inplace=True)
		data_df.head(10)
- 使用Matplotlib进行绘图

		import matplotlib.pyplot as plt

		plt.figure(figsize=(15,8))
		idx = list(data_df.index)
		plt.bar(idx, data_df['VOLUME'], width, color='b',edgecolor='w')
		plt.title(u'Market Price')
- ![Matplotlib绘图](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-Matplotlib.png)

## Wind API
- WindQuant起源于Wind API，在WindQuant上的脚本基本都可以用WindAPI在本地运行。
- 安装：
	- 在[大奖章](http://www.dajiangzhang.com/document)下载Wind API，当前是2.0版本，2015年Release
	- ![WindAPI-Download.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindAPI-Download.png)
	- 安装
	- ![WindAPI-Installation.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindAPI-Installation.png)
	- 安装后，会自动弹出登录窗口
	- ![WindAPI-Login.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindAPI-Login.png)
	- 如果要切换用户登陆，可以在登陆
	- ![WindAPI-SwitchUser.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindAPI-SwitchUser.png) 
- 安装好，并完成登陆后，就可以在本地使用WindAPI了
	- 代码

			from pandas import DataFrame
			from WindPy import w
			
			w.start()
			wsd_data=w.wsd("000001.SZ", "open,high,low,close,volume,amt,chg,pct_chg", "2016-11-01", "2017-01-03", "")
			print(wsd_data) 
	- 效果如下
	- ![WindAPI-PyCharm.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/8d2aaccebe604eb4947cd7d2953a5478-WindAPI-PyCharm.png) 