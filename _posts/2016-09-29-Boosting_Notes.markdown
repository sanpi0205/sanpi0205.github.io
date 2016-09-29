---
layout: post
title:  "boosting algorithm notes"
date:   2016-09-20 16:44:40
categories: Algorithm
---

关于Boosting算法的介绍中，个人认为最为经典的教程非《*The Elements of Statistical Learning*》 莫属。其将Boosting算法的细节都讲的非常透彻，非研读三五遍不可掌握，本文即为boosting算法的学习笔记。


##可加模型 (Additive Model)

Boosting的本质是一系列**基函数**的可加形式，通常形式如下：

$$
f(x) = \sigma_{M}
$$





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



