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
	b. 计算误判率
	$$
		err_{m}  = \frac{ \sum_{i}^N w_{i} I \left( y_{i} \not G_{m}(x_{i}) \right) }
	$$
