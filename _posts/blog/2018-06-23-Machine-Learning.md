---
layout:         post
title:          机器学习算法归类
category:       blog
description:    整理了sklearn模块中的机器学习算法
---

## 简介
- sklearn基于Numpy、SciPy和Matplotlib，大致分为6个子模块：
- ![Sklearn-Table.png](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/b65bd14fca944fbcbf582a5e0c60b3f5-Sklearn-Table.png)
- 其中，分类、聚类、回归、降维四类算法各自适用于场景：
- ![Sklearn-ML-Map.png](http://scikit-learn.org/stable/_static/ml_map.png)

## 预处理（Preprocessing）
- 特征提取和归一化
- 应用: 把输入数据（如文本）转换为机器学习算法可用的数据

### 标准化和正态化
- 标准化1：将数据转化为均值0，方差1
- 标准化2：离差标准化：`每个数据 - 数据集最小值 / (数据集最大值 - 数据集最小值)`
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

### 随机森林：random forest

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