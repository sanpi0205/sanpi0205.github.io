---
layout: post
title:  "R连接MySQL"
date:   2015-07-24 15:28:40
categories: MySQL
---

### RODBC
在Windows中用自带的ODBC建立数据库连接，然后在R中安装RODBC包，就可以读取和操作各种数据库。
当然也可以再MAC中使用ODBC，但相对更加复杂一些，MAC中安装ROBDC包需要编译，可能会出现错误信息：

{% highlight ruby %}

configure: error: "ODBC headers sql.h and sqlext.h not found"
ERROR: configuration failed for package ‘RODBC’

{% endhighlight %}

错误信息显示需要缺少头文件'sql.h'，此时安装unixodbc，使用命令 'brew instal unixodbc'之后，
就可以成功安装RODBC了。[参见][参见]


### RMySQL
如果是用R连接MySQL的话，可以使用RMySQL包，更加灵活方便。

{% highlight ruby %}

install.packages('RMySQL')
library('RMySQL')

db_address = "localhost" #数据库地址，可以是本地也可以是url
db_user = "root"
db_pass = ""
db_name = "database"

con = dbConnect(RMySQL::MySQL(), host = db_address, user = db_user, 
                password = db_pass, dbname = db_name)


{% endhighlight %}


### RMySQL命令
列出数据库中所有表：
{% highlight ruby %}
dbListTables(con)
{% endhighlight %}

列出表中所有字段：
{% highlight ruby %}
dbListFields(con, 'some_table')
{% endhighlight %}

具体可以参考RMySQL帮助，[地址][MySQL]



[参见]: http://superuser.com/questions/283272/problem-with-rodbc-installation-in-ubuntu
[MySQL]: https://cran.r-project.org/package=RMySQL