---
layout: post
title:  "Hive外部表到Mysql"
date:   2016-07-19 16:44:40
categories: Hive
---

从MySQL中导入 hdfs 或者 hive 都可以使用 sqoop 工具，sqoop提供了非常丰富的参数命令完成此项任务。但是有时我们的基础数据是以hive格式存储的，为了实现数据分析或挖掘的目的，需要从hive中生成聚合数据，然后导入mysql， 这样方便对接可视化的前端。

本文旨在说明如何从hive建立外部表的方式，导入mysql中。

## MySQL中建表
{% highlight sql %}
CREATE TABLE `dau` (
  `dt` varchar(8) NOT NULL COMMENT '日期',
  `mode` varchar(200) COMMENT '机型',
  `version` varchar(200) COMMENT '版本',
  `province` varchar(150)  COMMENT '省份',
  `dau` bigint(20) DEFAULT 0,
  KEY `eui_dau_idx` (`dt`,`mode`,`version`,`province`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

{% endhighlight %}


## Hive中建表
{% highlight sql %}
add jar /home/scloud/eui/lib/qubole-hive-JDBC-0.0.6.jar;
add jar /home/scloud/eui/lib/mysql-connector-java-5.1.39-bin.jar;

DROP TABLE dau;

CREATE EXTERNAL TABLE dau(
  dt string,
  mode string,
  version string,
  province string,
  dau bigint
)
STORED BY 'org.apache.hadoop.hive.jdbc.storagehandler.JdbcStorageHandler'
TBLPROPERTIES (
  "mapred.jdbc.driver.class"="com.mysql.jdbc.Driver",
  "mapred.jdbc.url"="",
  "mapred.jdbc.username"="",
  "mapred.jdbc.input.table.name"="",
  "mapred.jdbc.output.table.name"="",
  "mapred.jdbc.password"="",
  "mapred.jdbc.hive.lazy.split"= "false"
);

{% endhighlight %}


## Hive导入数据
{% highlight sql %}
insert overwrite table scloud_log.dau
select a.dt, b.device_mode, a.version, a.ip_province_name, sum(a.amount)
from database.table
{% endhighlight %}


## 编码问题

用hive的外部表导数据到MySQL的时候，可能存在编码导致乱码的情况出现，
hive中使用utf8编码，而MySQL可以使用其他编码方式，因而需要将MySQL
也设置为utf8编码，参见如下：

{% highlight sql %}
show variables like 'collation_%';

set character_set_connection=utf8;
set character_set_results=utf8;
set character_set_server=utf8;
set character_set_database=utf8;

show variables like 'collation_%'; 

重启后会失效，设置默认编码
sudo vim /etc/mysql/my.cnf

[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci

{% endhighlight %}


## 定时任务
在数据仓库建立好之后，就需要从Hive仓库中抽取相应数据写入已经建好的
数据库中。数据抽取可以有多少办法， 定时任务就是其中一种。

使用`crontab -e`来编辑定时任务，
{% highlight bash %}
0 12 * * *  flock -xn /home/scloud/eui/report/report.lock -c 'bash -l /home/scloud/eui/data_support/daily_report/dau_yesterday.sh > /home/scloud/eui/data_support/daily_report/log.log 2>&1'
{% endhighlight %}


参见:
 
1. [指南][mannul]
2. [MySQL connector][mysql connector]
3. [示例][aaa]
4. [hive to mysql 乱码][乱码]
5. [stack flow MySQL 编码][默认编码]


[mannul]: http://arjon.es/2014/02/12/Integrating-MySQL-RDS-with-Hive/
[mysql connector]: http://www.cloudera.com/documentation/archive/cdh/4-x/4-2-0/CDH4-Installation-Guide/cdh4ig_topic_18_4.html
[aaa]: http://www.toadworld.com/platforms/oracle/w/wiki/11583.creating-a-hive-external-table-over-mysql-database
[乱码]: http://blog.csdn.net/zreodown/article/details/8850222
[默认编码]: http://stackoverflow.com/questions/22572558/how-to-set-character-set-database-and-collation-database-to-utf8-in-my-ini



