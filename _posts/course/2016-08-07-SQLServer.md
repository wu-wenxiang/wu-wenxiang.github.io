---
layout:         post
title:          SQLServer-2012
category:       course
description:    本文整理了SQLServer 2012的基本知识点
---

## Hello, SQLServer

### Introduction
- 同类产品：Oracle / DB2(IBM)
- 优势：关系数据库平台 / 数据集成 / 分析和报表设计
- 组成：
	- SQL Server DB Engine (Relational Engine)，SQL语言用于向Engine描述问题。
		- Algebrizer：代数器，检查语法，并将查询转换成内部表达式
		- Query Optimizer：查询优化器。使用Management Studio或者SQL Profiler以图形的方式或在XML中查看估计的和实际的查询-执行计划。
		- Query Engine/Processor：根据Query Optimizer生成的计划进行查询
		- Storage Engine：为Query Engine服务，并处理磁盘的实际读写
		- Buffer Manager：分析正在使用的数据页，从数据文件中预提取数据并存人内存，从而减少对磁盘I/O的依赖性
		- Checkpoint：将内存中的脏数据页（修改的数据页）写入文件数据的进程 
		- Resources Monitor：响应内存压力，智能地从缓存区删除较早的查询计划来优化查询计划
		- Lock Manager：用锁的大小动态管理锁定的范围以平衡所需锁的数量
		- SQLOS：负责管理所有内部资源（直接控制可用资源：内存、线程、I/O请求）
	- Services
		- SQL Server Agent（可选进程），执行SQL作业并处理其它自动任务，可配置为系统启动时该进程自动运行，或者从SQL Server配置管理器（SQLServerManager11.msc）或者SSMS（SQL Server Management Studio）中的Object Explorer中启动。
		- DB Mail：SMTP，T-SQL代码、作业、警报、集成服务、维护计划都可以发出邮件
		- MSDTC（MS分布式事务处理协调器，Distributed Transaction Coordinator）
	- BI，Business Intelligence：管理数据以进行分析，研究，报表设计，挖掘，可视化
		- SSIS：Integration Services，可以在任何类型的数据源之间移动数据，是SQL Server ETL（提取-转换-加载，Extract-Tranform-Load）工具。
			- 采用图形化界面来说明数据如何从一个连接移动到另一个连接。
			- 可用于：
				- 复制数据列
				- 执行复杂的数据转化
				- 查找
				- 数据移动过程中的异常处理
			- 优势：
				- 移动和转换数据用自定义编程和T-SQL也可以，但SSIS与之相比，速度快，易跟踪
		- SSAS(Analysis Services)：
			- OLAP(Online Analytical Processing，联机分析处理)，多维数据库。SQL关系数据库使用T-SQL，而多维数据库采用多维表达式MDX(Multidimensional Expressions，MDX)这是专用于检索多维数据的基于集的查询语言。
			- 数据挖掘：挖掘算法
		- SSRS(Reporting Services)：基于Web的托管报表解决方案，可产生PDF、Excel、其它格式的文件。
	- 工具和附加组建
		- SSMS(Management Studio)：
			- 核心是Object Explorer
			- Filter，筛选器
			- 能浏览所有SQLServer服务（DB Engine、AS、RS）
			- Query Editor支持原始的T-SQL代码，集成Solution Explorer用于管理项目
		- SQL Server配置管理器，SQLServerManager11.msc
			- Start菜单或者SSMS中启动
			- 可显示在特定服务器上运行的所有服务和服务器
			- 启动和停止任意服务器
			- 设置启动选项
			- 配置连接性 
		- SQL Profiler/Trace/扩展事件
			- Profiler用于实时跟踪或查看跟踪文件，SQL流量、SQL Server事件
			- 扩展事件是Profiler的替代品
		- 性能监视器：
			- 可视化窗口
			- 监控选中的性能计数器的当前状态
		- DB Engine Tuning Advisor，DETA可以：
			- 分析来自Profiler的一批查询
			- 推荐索引和分区修改操作
			- 可以配置该工具推荐的修改范围，并且在分析时或者分析完成后部分或整体应用修改
		- 命令行实用程序
			- sqlcmd：执行sql代码
			- bcp：执行大容量复制操作
			- IS和SQL Server Agent已经淘汰他们
			- SSMS有一个模型，可以像使用sqlcmd那样使用查询编辑器
 
### Installation

### Client

### SSMS

## Database Design & SQL Syntax

### SQL Query

### DB Design

### Data Type

### Join

### Aggregation

### View

### CURD & More

## T-SQL

### Hierarchy

### XML Data Type

### 分布式查询

### T-SQL Programming

### 存储过程

### Custom Function

## Management & Configuration

### Config SQLServer

### Policy

### Backup & Recovery

### 维护

### 传输数据库

### Snapshot

### Service Broker & Async

### Log传送

### 数据库镜像

### 复制数据

### 群集

### Powershell

### Azure

## Security

### Authentication

### Authorization

### Encrypt

### 行级安全性

## Monitor & Audit

### 触发器

### 性能监视器 & PAL

### Profiler & SQL Trace

### 等待状态

### Extended Events

### 数据更改的跟踪和捕获

### SQL Audit

### 数据仓库

## Performance Tunning

### 查询执行计划

### Index

### Reuse查询计划

### 事务、锁定和阻塞

### 数据压缩

### 分区

### Resource Governor

## BI

### BI数据库设计

### SSIS & ETL

### MDX

### Analysis Services

### Reporting Services

### 数据挖掘

### PowerPivot

### PowerView