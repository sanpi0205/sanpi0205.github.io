---
layout: post
title:  "R连接MySQL"
date:   2015-07-24 15:28:40
categories: MySQL
---

### RODBC
在Windows中用自带的ODBC建立数据库连接，然后在R中安装RODBC包，就可以读取和操作各种数据库。
当然也可以再MAC中使用ODBC，但相对更加复杂一些，MAC中安装ROBDC包需要编译，可能会出现错误信息：

{% highlight r %}

configure: error: "ODBC headers sql.h and sqlext.h not found"
ERROR: configuration failed for package 'RODBC'

{% endhighlight %}

错误信息显示需要缺少头文件`sql.h`，此时安装unixodbc，使用命令 `brew instal unixodbc`之后，
就可以成功安装RODBC了。[参见][参见]


### RMySQL
如果是用R连接MySQL的话，可以使用RMySQL包，更加灵活方便。

{% highlight r %}

install.packages('RMySQL')
library('RMySQL')

db_address = "localhost" #数据库地址，可以是本地也可以是url
db_user = "root"
db_pass = ""
db_name = "database"

con = dbConnect(RMySQL::MySQL(), host = db_address, user = db_user, 
                password = db_pass, dbname = db_name)

# 有时由于编码原因，会出现乱码，解决方案是设定编码，[如下命令:][R_mysql_encoding]
dbSendQuery(con, "SET NAMES utf8")

{% endhighlight %}


### RMySQL命令
列出数据库中所有表：
{% highlight r %}
dbListTables(con)
{% endhighlight %}

列出表中所有字段：
{% highlight r %}
dbListFields(con, 'some_table')
{% endhighlight %}

R操作数据库：
{% highlight r %}
# 建立连接
db_address = "host"
db_user = "user"
db_pass = "password"
db_name = "database name"

con = dbConnect(RMySQL::MySQL(), host = db_address, user = db_user, 
                password = db_pass, dbname = db_name)
# 设置编码格式
dbSendQuery(con, "SET NAMES utf8")
# 查询数据
query = "SELECT * FROM table_name;"
# 提取数据
data = dbGetQuery(con, query)
# dbGetQuery实际上执行了一系列操作，其等同于如下代码
res = dbSendQuery(con, query)
data = dbFetch(res)
# 关闭连接和清除查询
dbClearResult(res)
dbDisconnect(con)

# 写入数据：可以直接将data frame数据写入数据库成为数据表
dbWriteTable(con, "table_name", data_frame)

{% endhighlight %}

具体可以参考[RMySQL帮助][MySQL]。



[参见]: http://superuser.com/questions/283272/problem-with-rodbc-installation-in-ubuntu
[MySQL]: https://cran.r-project.org/package=RMySQL
[R_mysql_encoding]: http://stackoverflow.com/questions/12869778/fetching-utf-8-text-from-mysql-in-r-returns