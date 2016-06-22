---
layout: post
title:  "Ambari install on Ubuntu cluster"
date:   2016-06-22 16:44:40
categories: big data
---

Ambari提供一站式大数据组件安装方案，使得大数据应用部署和监控非常容易，本文着重探讨Ambari安装的全流程，
以期未来避免踩坑。

### 集群权限

ambari在集群配置hadoop组件时会安装很多应用，如hive, spark等，需要root权限，
因为ubutu中root密码的随机确定的，因而首先需要给root用户设置一个密码。

{% highlight bash %}
sudo passwd
#输入当前用户密码后，　enter

#输入root密码后，切换到root
su root
{% endhighlight %}

切换到root用户后，默认情况下不能ssh登陆root用户，解决办法如下：
{% highlight bash %}
vim /etc/ssh/sshd_config

#找到如下
# Authentication:
LoginGraceTime 120
PermitRootLogin without-password
StrictModes yes

#修改为
LoginGraceTime 120
#PermitRootLogin without-password
PermitRootLogin yes
StrictModes yes

#之后重启ssh服务即可
service ssh restart
{% endhighlight %}

当然集群中不同机器还需要设置ssh免密码登陆，参见(ssh免密码登陆)[ssh]




{% highlight bash %}
# 下载并解压 Hive
wget http://mirrors.cnnic.cn/apache/hive/hive-2.0.1/apache-hive-2.0.1-bin.tar.gz
tar xvzf tar xvzf apache-hive-2.0.1-bin.tar.gz
cd apache-hive-2.0.1-bin/
{% endhighlight %}


### 安装Hive
{% highlight bash %}
# 新建安装目录，并赋予权限
sudo mkdir /usr/local/hive
sudo chown -R hduser:hadoop /usr/local/hive

# 将安装文件复制到安装目录下
mv * /usr/local/hive

# 将hive添加到路径中
sudo vim ~/.bashrc
# 末尾增加路径
export HIVE_HOME=/usr/local/hive
export PATH=$HIVE_HOME/bin:$PATH

# 时路径生效
source ~/.bashrc
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


## 参考资料
[ssh]:http://sanpi0205.github.io/hadoop/2015/08/04/Ubuntu-install-Hadoop-cluster.html