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







## 参考资料
[ssh]:http://sanpi0205.github.io/hadoop/2015/08/04/Ubuntu-install-Hadoop-cluster.html