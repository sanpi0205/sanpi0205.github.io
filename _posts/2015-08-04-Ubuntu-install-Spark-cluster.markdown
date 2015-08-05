---
layout: post
title:  "Ubuntu install Spark cluster"
date:   2015-08-04 16:44:40
categories: Spark
---

### 安装Scala

{% highlight bash %}
# 下载Scala，并解压
wget http://downloads.typesafe.com/scala/2.11.7/scala-2.11.7.tgz
tar xvzf scala-2.11.7.tgz

cd scala-2.11.7/

# 新建安装目录，并赋予权限
sudo mkdir /usr/lib/scala
sudo chown -R hduser:hadoop /usr/lib/scala/

# 将安装文件复制到安装目录下
mv * /usr/lib/scala/

# 将scala添加到路径中
sudo vim ~/.bashrc
# 末尾增加路径
export SCALA_HOME=/usr/lib/scala
export PATH=$PATH:$SCALA_HOME/bin

# 时路径生效
source ~/.bashrc

scala -version # 查看scala版本
{% endhighlight %}

### 安装Spark 


