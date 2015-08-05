---
layout: post
title:  "Ubuntu install Hadoop cluster"
date:   2015-08-04 14:44:40
categories: Hadoop
---

## Ubuntu配置Hadoop集群

本文中主要介绍如何在Ubuntu环境下，安装配置Hadoop集群。文中使用的Ubuntu版本为14.02，Hadoop版本为2.6.0，
本文假定集群中有三台机器，并且已经分别独立安装配置好Hadoop。

### 配置SSH无密码登陆

从Slave节点中获取登陆公钥，并将其加入到Master中
{% highlight }
# 从Slave1中复制公钥并添加至Master
scp hduser@Slave1:/home/hduser/.ssh/id_rsa.pub /home/hduser/id_rsa.pub.slave1
cat /home/hduser/id_rsa.pub.slave1 >> /home/hduser/.ssh/authorized_keys

# 从Slave2中复制公钥并添加至Master
scp hduser@Slave2:/home/hduser/.ssh/id_rsa.pub /home/hduser/id_rsa.pub.slave2
cat /home/hduser/id_rsa.pub.slave1 >> /home/hduser/.ssh/authorized_keys

{% endhighlight %}

将Master的认证文件复制到每个Slave中

