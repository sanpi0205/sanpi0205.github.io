---
layout: post
title:  "R get baidu coordinate by address"
date:   2015-08-18 16:44:40
categories: R
---

### R程序获取百度地图坐标

{% highlight r %}

# 加载程序包
library(rvest)
library(rjson)
library(stringr)

# 设定url，其中百度大厦为地址
url = paste0("http://api.map.baidu.com/","geocoder/v2/?", 
               "ak=fulYyTzPOK6ag6vksDMWdCf9&", "callback=showLocation&",
               "output=json&", "address=", "百度大厦", "&city=北京市")

# 解析地址
address_html = html(url)
address_json = address_html %>% html_nodes("p") %>% html_text()

# 提取地址的json数据
address_json = str_extract(address_json, "\\(.*\\)")
address_json = str_replace(address_json, "\\(", "")
address_json = str_replace(address_json, "\\)", "")  # 这里处理方式可以优化为一步处理

# 解析json
address_json = fromJSON(address_json)
print(address_json)

{% endhighlight %}

参考文献:

* [百度地图API][API]
* [R parse Json][json]

[API]: http://developer.baidu.com/map/index.php?title=webapi/guide/webservice-geocoding
[json]: http://gastonsanchez.com/work/webdata/getting_web_data_r5_json_data.pdf



