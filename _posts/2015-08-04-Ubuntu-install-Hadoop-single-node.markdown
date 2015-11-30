---
layout: post
title:  "Ubuntu install Hadoop single node"
date:   2015-08-04 12:44:40
categories: Hadoop
---

## Ubuntu安装hadoop单机

本文中主要介绍如何在Ubuntu环境下，安装配置Hadoop单机。文中使用的Ubuntu版本为14.02，Hadoop版本为2.7.0，

### 更新并安装vim
{% highlight bash %}
sudo apt-get update
sudo apt-get install vim
{% endhighlight %}

### 新建Hadoop用户hduser与用户组hadoop
{% highlight bash %}
sudo addgroup hadoop
sudo adduser --ingroup hadoop hduser
sudo adduser hduser sudo
# 切换用户
su hduser
{% endhighlight %}

### 配置SSH无密码登陆
{% highlight bash %}
#install ssh
sudo apt-get install ssh
# generate ssh key
ssh-keygen -t rsa -P ""
# check if ssh works
ssh localhost
exit
# add public key in to authorized keys
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
{% endhighlight %}




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

参考文献:
[hadoop install on a single node]

[hadoop install on a single node]: http://www.bogotobogo.com/Hadoop/BigData_hadoop_Install_on_ubuntu_single_node_cluster.php
