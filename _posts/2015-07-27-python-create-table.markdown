---
layout: post
title:  "Python创建数据表"
date:   2015-07-27 10:28:40
categories: Python
---

### Python创建本地数据表

{% highlight python3 %}

# -*- coding: utf-8 -*-
"""
Created on Sun Jul 26 20:32:16 2015

@author: zhangbo
"""

# python操作数据库

import mysql.connector

# 连接数据库
cnx = mysql.connector.connect(user='root', password='',
                              host='127.0.0.1',
                              database='energy')
cursor = cnx.cursor()

# 数据表结构 
sector_table = ( 
    "CREATE TABLE `sector` ("
    "`id` int NOT NULL PRIMARY KEY AUTO_INCREMENT, "
    "`sector_id` int(10), "
    "`type_id` int(10), "
    "`data_year` int(10), "
    "`sector_name` varchar(50), "
    "`sector_name_en` varchar(50), "
    "`type_name` varchar(50), "
    "`data_value` double, "
    "`data_unit` varchar(50), "
    "`data_category` varchar(50)"
    ")"
  )

# 执行数据库
cursor.execute(sector_table)

# 删除数据表
# cursor.execute("drop table `sector`")

cursor.close()
cnx.close()

{% endhighlight %}


[Mysql命令参考][参考文献]

[参考文献]: http://dev.mysql.com/doc/connector-python/en/connector-python-example-ddl.html
