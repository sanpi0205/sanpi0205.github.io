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
create user test;             # 新建用户
{% endhighlight %}

### 新建数据库
{% highlight ruby %}
show databases;
create database test; #新建数据库
drop database test;   #删除数据库
{% endhighlight %}

### 权限管理
将test数据库授权于test用户
{% highlight ruby %}
GRANT ALL ON test.* TO test@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES; 
# 也可以指定登陆地址
GRANT ALL ON test.* TO test@'192.168.1.3' IDENTIFIED BY 'password'  WITH GRANT OPTION;
{% endhighlight %}

### 新建数据表
{% highlight ruby %}
# 使用数据库
use test;
# 显示当前数据库中所有表
show tables;
# 新建表
create table test_table ( `id` int, `sector` int)
{% endhighlight %}

### 数据库备份还原
{% highlight ruby %}
# 数据库备份
mysqldump -u root -p energy > energy.sql

# 数据库还原
 mysql -u root -p energy < energy.sql
{% endhighlight %}


[参考文献][参考文献]

[参考文献]: https://www.digitalocean.com/community/tutorials/a-basic-mysql-tutorial
