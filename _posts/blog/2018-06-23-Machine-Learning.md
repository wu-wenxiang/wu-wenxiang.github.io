---
layout:         post
title:          科学计算与机器学习
category:       blog
description:    整理了一些涉及科学计算与机器学习的Python模块
---

# Numpy
- 强大的N维数组对象：数组array和列表list类似，但是数据array可以定义维度，且适合做数学代数运算
- 精密的函数
- 连接C/C++和Fortran代码的工具 
- 常用的线性代数，傅里叶变换和随机数生成

## 一元运算函数
![Numpy-1.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/b65bd14fca944fbcbf582a5e0c60b3f5-Numpy-1.png)

## 二元运算函数
![Numpy-2.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/b65bd14fca944fbcbf582a5e0c60b3f5-Numpy-2.png)

## 随机数产生
![Numpy-Random.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/b65bd14fca944fbcbf582a5e0c60b3f5-Numpy-Random.png)

## 其它常见运算
- 集合运算
- 切片进阶
- 排序
- 数组拼接
- 数组分解
- 矩阵运算
- 多项式拟合

## 特殊数组的产生和常量
- np.arange(起始数,终止数,间隔)
- np.linspace(起始数,终止数,产生数的个数)
- 常用矩阵：
	- np.ones((3,3))
	- np.zeros((3,3))
	- np.eye(3)
- np.tile()：该函数的作用是重复某个对象为一定的结构
- np.meshgrid()函数，在画三维图时常用，其含义是利用两个坐标轴的点形成一个平面
- numpy常用常量
	- 自然底数：np.e
	- 缺失值：np.NaN
	- 无穷大：np.inf
	- 圆周率：np.pi

# Pandas

# Scipy

# Sklearn
- sklearn建立于Numpy、SciPy和Matplotlib的基础上
- ![Sklearn-Table.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/b65bd14fca944fbcbf582a5e0c60b3f5-Sklearn-Table.png)
- ![Sklearn-ML-Map.png](http://scikit-learn.org/stable/_static/ml_map.png)

## 预处理（Preprocessing）
- 特征提取和归一化
- 应用: 把输入数据（如文本）转换为机器学习算法可用的数据

### 标准化和正态化
- 标准化1：将数据转化为均值0，方差1
- 标准化2：每个数据 - 数据集最小值 / (数据集最大值 - 数据集最小值)
- 规范化：处理后范数为1
- 二值化：连续值 => 离散值
- 缺失处理：插值法
- 多项式变型：用于回归预测
- ![polynomial_interpolation](http://scikit-learn.org/stable/_images/sphx_glr_plot_polynomial_interpolation_001.png)

## 分类（Classification）
- 识别某个对象属于哪个类别
- 应用: 垃圾邮件检测，图像识别

### 决策树（Decision Tree）
- 决策树是分类中最为基础的算法之一，其核心是依据特征的重要程度，依次使用单个特征进行分类
- 决策树(Decision Tree）是在已知各种情况发生概率的基础上，通过构成决策树来求取净现值的期望值大于等于零的概率，评价项目风险，判断其可行性的决策分析方法，是直观运用概率分析的一种图解法。
- 由于这种决策分支画成图形很像一棵树的枝干，故称决策树。

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