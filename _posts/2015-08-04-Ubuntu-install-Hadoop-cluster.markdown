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
{% highlight bash %}
# 从Slave1中复制公钥并添加至Master
scp hduser@Slave1:/home/hduser/.ssh/id_rsa.pub /home/hduser/id_rsa.pub.slave1
cat /home/hduser/id_rsa.pub.slave1 >> /home/hduser/.ssh/authorized_keys

# 从Slave2中复制公钥并添加至Master
scp hduser@Slave2:/home/hduser/.ssh/id_rsa.pub /home/hduser/id_rsa.pub.slave2
cat /home/hduser/id_rsa.pub.slave1 >> /home/hduser/.ssh/authorized_keys
{% endhighlight %}

将Master的认证文件复制到每个Slave中

{% highlight  bash %}
scp ~/.ssh/authorized_keys hduser@Slave1:/home/hduser/.ssh/authorized_keys
scp ~/.ssh/authorized_keys hduser@Slave2:/home/hduser/.ssh/authorized_keys
{% endhighlight %}

### 修改Hadoop配置文件

需要在集群中每个节点修改Hadoop配置文件，包括`core-site.xml`，`mapred-site.xml`和`hdfs-site.xml`
这三个文件。

{% highlight  bash %}
vim /usr/local/hadoop/etc/hadoop/core-site.xml
# 将其中 localhost 修改为 Master

vim /usr/local/hadoop/etc/hadoop/mapred-site.xml
# 将其中 localhost 修改为 Master

vim /usr/local/hadoop/etc/hadoop/hdfs-site.xml
# 将其中备份数量1 修改为设定的备份数，比如3个备份
{% endhighlight %}

返回至Master节点，配置`masters`和`slaves`文件，进入Hadoop文件目录`cd /usr/local/hadoop/etc/hadoop/`

{% highlight  bash %}
vim masters
# 输入Master，作为Master节点

vim slalves
# 输入Master, Slave1, Slave2作为Slave节点，这里将Master节点也作为一个DataNode，也可以不加入Master，使Master节点只作为NameNode。
{% endhighlight %}

将配置好的`masters`和`slaves`文件复制所有Slave节点中

{% highlight  bash %}
# 复制到Slave1节点中
scp masters hduser@Slave1:/usr/local/hadoop/etc/hadoop/
scp slaves  hduser@Slave1:/usr/local/hadoop/etc/hadoop/

# 复制到Slave2节点中
scp masters hduser@Slave2:/usr/local/hadoop/etc/hadoop/
scp slaves  hduser@Slave2:/usr/local/hadoop/etc/hadoop/
{% endhighlight %}

之后就可以启动Hadoop集群了，但如果我们之前已经对单机的Hadoop进行过格式化处理，此时需要重新格式化。

{% highlight  bash %}
# 删除Hadoop中文件
rm -r /app/hadoop/tmp/*
rm -r /usr/local/hadoop_store/hdfs/datanode/*
rm -r /usr/local/hadoop_store/hdfs/namenode/*

# 格式化Hadoop
hadoop namenode -format

# 启动Hadoop集群
start-all.sh

# Hadoop的WebUI
localhost:50070

# 关闭Hadoop
stop-all.sh
{% endhighlight %}

至此，我们已经部署好Hadoop集群，可以在Master和Slave节点中输入`jps`来查看进行，但该命令可能因计算机架构而不同。