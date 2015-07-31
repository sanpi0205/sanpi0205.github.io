---
layout: post
title:  "Use R to scrape the web"
date:   2015-07-30 14:28:40
categories: R
---

### 利用R抓取web并解析

本文使用R中web解析包`rvest`来抓取和解析web，`rvest`借鉴了`Pyhon`和`Ruby`中web解析包`Beautiful Soup`，
并支持`%>%`语法，使程序非常简洁，可读性非常强。

#### 加载R包
{% highlight ruby %}

library(rvest)
library(stringr)
library(dplyr)
library(ggplot2)
library(GGally)

{% endhighlight %}
