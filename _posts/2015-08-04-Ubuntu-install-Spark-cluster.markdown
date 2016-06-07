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

本文假定已经安装Hadoop，因而在安装Spark时选择已经编译好但不带Hadoop的Spark安装包。

#### 下载Spark
{% highlight bash %}
wget http://mirrors.cnnic.cn/apache/spark/spark-1.4.1/spark-1.4.1-bin-without-hadoop.tgz
tar xvzf spark-1.4.1-bin-without-hadoop.tgz
cd spark-1.4.1-bin-without-hadoop/
{% endhighlight %}

#### 创建Spark安装目录
{% highlight bash %}
sudo mkdir /usr/local/spark
sudo chown -R hduser:hadoop /usr/local/spark/
{% endhighlight %}

#### 复制文件至安装目录
{% highlight bash %}
mv * /usr/local/spark/
{% endhighlight %}

#### 添加Spark路径
{% highlight bash %}
vim ~/.bashrc
export SPARK_HOME=/usr/local/spark
export PATH=$PATH:$SPARK_HOME/bin

source ~/.bashrc
{% endhighlight %}

#### 修改Spark配置文件
{% highlight bash %}
cd /usr/local/spark/conf
cp spark-env.sh.template spark-env.sh
vim spark-env.sh


export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export SCALA_HOME=/usr/lib/scala
export SPARK_MASTER_IP=10.58.21.225
export SPARK_WORKER_MEMORY=4g
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
export SPARK_DIST_CLASSPATH=$(/usr/local/hadoop/bin/hadoop classpath)
#export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
#export SPARK_DIST_CLASSPATH=$(hadoop classpath)
{% endhighlight %}

#### 配置Spark节点
{% highlight bash %}
vim slaves
# 加入Worker节点
Master
Slave1
Slave2
#单机 localhost
{% endhighlight %}

#### 启动Spark
{% highlight bash %}
# 启动Hadoop
start-all.sh

# 启动Spark
cd /usr/local/spark/sbin/
./start-all.sh
{% endhighlight %}

#### 测试Spark
{% highlight bash %}
cd /usr/local/spark
./bin/spark-shell
{% endhighlight %}

在进入Spark shell后，测试Spark实例
{% highlight scala %}
# 上传文件至HDFS
hadoop fs -copyFromLocal README.md /

# 在Spark shell中读取
val file = sc.textFile("hdfs://Master:54310/README.md") #注意单机和集群的地址可能不同
val sparks = file.filter(line => line.contains("Spark"))
sparks.count

{% endhighlight %}
