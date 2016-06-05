---
layout: post
title:  "Python-shell-hive-数据指标"
date:   2016-06-04 10:28:40
categories: Hive
---

首次激活时间、每日新增用户以及应用留存在衡量一个应用或者服务的重要宏观指标。但在实际收集中，数据一般存储的是动作数据，即某个用户在某个特定的时间触发了某个特定的事件，因而在获取首次激活时间、每日新增用户以及用户留存等宏观指标时，需要基于事件数据计算各个宏观指标。


### 首次激活时间

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

循环执行过程有很多方法，可以用`shell`也可以用其他程序，比如`python`就需要用到 [python 日期循环][pthon_date_loop]。

{% highlight python3 %}
# -*- coding: utf-8 -*-
"""
Created on Fri Jun  3 23:51:44 2016

@author: zhangbo
"""

import datetime
import os
import time

def main():
    
    date_start = datetime.date(2016,1,1)
    date_end = datetime.date(2016,5,30)
    
    n_days = (date_end - date_start).days
    
    for i in range(n_days+1):
        data_date = (date_start + datetime.timedelta(days=i)).strftime("%Y%m%d")
        print "开始抽取 %s 数据" %data_date
        sql = """insert into db.first_use
        select * from
        (
        select pid, app, dt
        from db.event_table
        where dt=%s and 
        app="app_name"
        group by pid, app, dt
        ) a
        where a.pid not in (
        select pid 
        from db.first_use 
        where app="app_name"
        );
        """ %data_date
        sql = sql.replace('\n', ' ')
        command = "hive -e '%s'" %sql
        os.system(command)
        
        print "完成 %s 抽取" %data_date
        time.sleep(10)
        
if __name__ == '__main__':
    main()


{% endhighlight %}

这里唯一需要指出的问题是在使用`python`时，需要将程序提交至服务器后台运行，否则退出`shell`时，程序也会自动中断，方法如下：
{% highlight bash %}
nohup python process.py &
{% endhighlight %}

参见:[PYTHON的程序在LINUX后台运行][PYTHON的程序在LINUX后台运行]


### 当期、累计和新增

完成数据处理后，首次激活即可以非常容易的计算。当然在实际统计中，不仅需要统计特定日期的激活量，还需要统计累计截止到某一日期累计激活量。在有了上述历史表就可以很容计算。

{% highlight sql %}


{% endhighlight %}







### 




### 参考文献

1. [Hive统计新增][Hive统计新增]




[Hive统计新增]: http://blog.itpub.net/29254281/viewspace-2097338
[pthon_date_loop]: http://blog.csdn.net/wusuopubupt/article/details/29606481
[PYTHON的程序在LINUX后台运行]: http://blog.csdn.net/chenyulancn/article/details/8152966
