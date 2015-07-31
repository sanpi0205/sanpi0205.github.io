---
layout: post
title:  "Use R to scrape the web"
date:   2015-07-30 14:28:40
categories: R
---

## 利用R抓取web并解析

本文使用R中web解析包`rvest`来抓取和解析web，`rvest`借鉴了`Pyhon`和`Ruby`中web解析包`Beautiful Soup`，
并支持`%>%`语法，使程序非常简洁，可读性非常强。

### 加载R包
{% highlight ruby %}

library(rvest)
library(stringr)
library(dplyr)
library(ggplot2)
library(GGally)

{% endhighlight %}

### 抓取web
本文以参考文献中实例为基础，改编而来。以抓取暴雪游戏中英雄属性为例，
抓取英雄的属性包括：名字，角色，攻击范围，HP，Mana，attack damage, attack speed

{% highlight ruby %}

heroes = html("http://www.heroesnexus.com/heroes")
hero_name = heroes %>% html_nodes(".hero-champion h3") %>% html_text()
hero_type = heroes %>% html_nodes(".hero-champion .hero-type .role") %>% html_text()
hero_hp = heroes %>% html_nodes(".hero-champion .hero-hp") %>% html_text() %>% 
  str_extract("(HP: \\d*)") %>% str_replace("HP: ", "") %>% as.numeric()
hero_Mana = heroes %>% html_nodes(".hero-champion .hero-mana") %>% html_text() %>% 
  str_extract("(Mana: \\d*)") %>% str_replace("Mana: ", "") %>% as.numeric()
hero_attack_damage = heroes %>% html_nodes(".hero-champion .hero-atk") %>% html_text() %>% 
  str_extract("(Attack Damage: \\d*)") %>% str_replace("Attack Damage: ", "") %>% as.numeric()

# R中正则表达式的写法是\\d，而不是\d
hero_attack_speed = heroes %>% html_nodes(".hero-champion .hero-atk") %>% html_text() %>% 
  str_extract("(Attack Speed: \\d?(.\\d*))") %>% str_replace("Attack Speed: ", "") %>% as.numeric()

{% endhighlight %}

需要注意的是`R`中使用正则表达式与`Pyhotn`和`Ruby`中略有不同，详细区别需查询帮助。


### 生成最终数据
{% highlight ruby %}

final_data = data.frame(name = hero_name, type = hero_type, hp = hero_hp,
                        mana = hero_Mana, atk = hero_attack_damage, atk_spd = hero_attack_speed)

{% endhighlight %}

### 可视化数据

{% highlight ruby %}

final_data %>% 
  select(hp, atk, atk_spd, type) %>% 
  ggpairs(data=., color = "type", title="英雄类型")

{% endhighlight %}



参考文献：[r got good at scraping][ref]

[ref]: http://koaning.stfi.re/posts/r-got-good-at-scraping.html?sf=klnbl