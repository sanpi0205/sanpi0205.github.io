---
layout: post
title:  "Python-shell-hive-数据指标"
date:   2016-06-04 10:28:40
categories: Hive
---

首次激活时间、每日新增用户以及应用留存在衡量一个应用或者服务的重要宏观指标。但在实际收集中，数据一般存储的是动作数据，即某个用户在某个特定的时间触发了某个特定的事件，因而在获取首次激活时间、每日新增用户以及用户留存等宏观指标时，需要基于事件数据计算各个宏观指标。


###首次激活时间

首次激活时间计算（有时也是用户首次使用时间），是用户使用应用或者服务的历史记录数据，只不过是仅记录用户首次的使用时间。

计算首次使用时间的思路是：
1. 创建首次使用时间的空表；
2. 循环日期，如果特定一天的用户不再用户首次使用历史表中，则将该数据插入首次使用历史表；
3. 直到所有时期循环结束

{% highlight sql %}

CREATE TABLE date_of_first_use(user_id string,  date_of_first_use string)
PARTITIONED BY ( package string)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY '\t' 
STORED AS TEXTFILE;

#创建该表时可以增加更多列，比如版本等信息，视实际情况而定
{% endhighlight %}


### 参考文献

1. [Hive统计新增][ref1]

2. [SQLite教程][ref2]


[ref1]: http://blog.itpub.net/29254281/viewspace-2097338

[ref2]: http://www.runoob.com/sqlite/sqlite-tutorial.html

