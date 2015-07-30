---
layout: post
title:  "AdaBoost algorithm"
date:   2015-07-29 14:28:40
categories: Algorithm
---

### AdaBoost
1. 初始化数据权重 $$ w_{i} = 1/N, i=1,2,...,N $$

2. For m = 1 to M:
	
	a. 利用权重 $$w_{i}$$ 对数据建立分类模型 $$G_{m}(x)$$ 	
	
	b. 计算误判率:
$$
err_{m} = \frac{ \sum_{i=1}^{N} w_{i}I( y_{i} \not= G_{m}(x_{i})) }{  \sum_{i}{N} w_{i} }
$$
	
	c. 计算权重调整系数 $$ \alpha_{m} = log( (1-err_{m}) / err_{m} ) $$

	d. 调整样本权重 $$  w_{i} = w_{i} * exp[ \alpha_{m} * I( y_{i} \not= G_{m}(x_{i}) ) ] $$

3. 最终模型 $$ G(x) = sign[ \sum_{m=1}^{M} \alpha_{m} * G_{m}(x) ] $$

### 算法注解
1. 算法中使用弱分类器$$G_m(x)$$在统计学习中通常为决策树

2. AdaBoost算法需要做适当的调整才能被用于回归问题。
