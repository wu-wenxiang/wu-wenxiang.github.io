---
layout:         post
title:          机器学习算法整理-未完成
category:       blog
description:    整理了一些常见的机器学习算法，基于Python
---

## 分类（Classification）
- 识别某个对象属于哪个类别
- 应用: 垃圾邮件检测，图像识别

### 支持向量机（Support vector machines，SVMs）

### nearest neighbors

### random forest

## 回归（Regression）
- 预测与对象相关联的连续值属性
- 应用: 药物反应，股价

### SVR

### ridge regression

### Lasso

## 聚类（Clustering）
- 将相似对象自动分组
- 应用: 客户细分，分组实验结果

### k-Means
- 原理
	- K-means 通常被称为 Lloyd’s algorithm（劳埃德算法）
	- 第一步是选择initial centroids（初始质心），最基本的方法是从X数据集中选择k个样本
	- 初始化完成后，K-means 由两个其他步骤之间的循环组成
	- 第二步将每个样本分配到其 nearest centroid （最近的质心）
	- 第三步通过取分配给每个先前质心的所有样本的平均值来创建新的质心
	- 计算旧的和新的质心之间的差异，并且算法重复这些最后的两个步骤，直到该值小于阈值
	- 换句话说，算法重复这个步骤，直到质心不再显著移动
- 适用场景
	- K-means算法通常可以应用于维数、数值都很小且连续的数据集
	- 比如：从随机分布的事物集合中将相同事物进行分组
- 用法

		#-*- coding: utf-8 -*-
		#使用K-Means算法聚类消费行为特征数据
		
		import pandas as pd
		from sklearn.cluster import KMeans
		import matplotlib.pyplot as plt
		plt.rcParams['font.sans-serif'] = ['SimHei'] #用来正常显示中文标签
		plt.rcParams['axes.unicode_minus'] = False #用来正常显示负号
		
		#参数初始化
		inputfile = '../data/consumption_data.xls' #销量及其他属性数据
		outputfile = '../tmp/data_type.xls' #保存结果的文件名
		k = 3 #聚类的类别
		iteration = 500 #聚类最大循环次数
		data = pd.read_excel(inputfile, index_col = 'Id') #读取数据
		data_zs = 1.0*(data - data.mean())/data.std() #数据标准化
		
		model = KMeans(n_clusters = k, n_jobs = 4, max_iter = iteration) #分为k类，并发数4
		model.fit(data_zs) #开始聚类
		
		#简单打印结果
		r1 = pd.Series(model.labels_).value_counts() #统计各个类别的数目
		r2 = pd.DataFrame(model.cluster_centers_) #找出聚类中心
		r = pd.concat([r2, r1], axis = 1) #横向连接（0是纵向），得到聚类中心对应的类别下的数目
		r.columns = list(data.columns) + [u'类别数目'] #重命名表头
		print(r)        #打印分类中心和分类数量
		
		#详细输出原始数据及其类别
		r = pd.concat([data, pd.Series(model.labels_, index = data.index)], axis = 1)
		r.columns = list(data.columns) + [u'聚类类别'] #重命名表头
		r.to_excel(outputfile) #保存分类结果
		
		def density_plot(data): #自定义作图函数  
		    p = data.plot(kind='kde', linewidth = 2, subplots = True, sharex = False)
		    [p[i].set_ylabel(u'密度') for i in range(k)]
		    plt.legend()
		    return plt
		
		pic_output = '../tmp/pd_' #概率密度图文件名前缀
		for i in range(k):
		    density_plot(data[r[u'聚类类别']==i]).savefig(u'%s%s.png' %(pic_output, i))

### spectral clustering

### mean-shift

## 降维（Dimensionality reduction）
- 减少要考虑的随机变量的数量
- 应用: 可视化，提高效率

### PCA

### feature selection

### non-negative matrix factorization

## 模型选择（Model selection）
- 比较，验证，选择参数和模型
- 目标: 通过参数调整提高精度

### grid search

### cross validation

### metrics

## 预处理（Preprocessing）
- 特征提取和归一化
- 应用: 把输入数据（如文本）转换为机器学习算法可用的数据

### preprocessing

### feature extraction