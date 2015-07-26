---
layout: post
title:  "MySQL命令"
date:   2015-07-26 15:28:40
categories: MySQL
---

### 安装MySQL
Mac中安装MySQL使用命令`brew install mysyql`，在Linux中安装使用命令`sudo apt-get install mysql`

### 进入MySQL
{% highlight ruby %}
for mac:
mysql.server start

for linux:
/etc/init.d/mysqld start

# 进入mysql
mysql -u root -p
{% endhighlight %}


### 新建用户
{% highlight ruby %}
SELECT User FROM mysql.user;  # 查看当前用户
 create user test;            # 新建用户
{% endhighlight %}

### 新建数据库


### 权限管理




[参考文献：openpyxl][参考文献]

[参考文献]: https://openpyxl.readthedocs.org/en/latest/index.html
