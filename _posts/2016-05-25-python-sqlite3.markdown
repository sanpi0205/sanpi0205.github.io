---
layout: post
title:  "Python创建Sqlites数据库"
date:   2016-05-25 10:28:40
categories: Python
---

Sqlite是常用的本地数据库，将数据保存为本地文件，非常易于不同平台和应用之间读取。

### Python操作Sqlite数据库

{% highlight python3 %}

import sqlite3

import pandas as pd

app_code = pd.read_table('/home/zhangbo/data/data_support/app_code', sep='\t',
                         header=None)
app_code = app_code.drop([0,5,6], axis=1)                        
app_code.columns=['name','package_name','category','sub_category']
app_usage = pd.read_table('/home/zhangbo/data/data_support/app_user_category.rs',
                          header=None)
app_usage.columns = ['package_name','users_count']

app_usage.shape

tt = pd.merge(app_usage, app_code, how='left', left_on='package_name', 
              right_on='package_name')


tt = tt.sort_values(['users_count'], ascending=False)

conn = sqlite3.connect('app_category.db')
cursor = conn.cursor()

sql_table = 'drop table  if exists app_category'
cursor.execute(sql_table)

sql_table = '''create table app_category (package_name varchar(100), 
category varchar(40), sub_category varchar(40))
'''

cursor.execute(sql_table)

aa = tt.drop(['users_count','name'], axis=1)
aa.package_name = aa.package_name.astype('str')
aa.category = aa.category.astype('str')
aa.sub_category = aa.sub_category.astype('str')

aa = aa[:10000]
add_data = (
    "INSERT INTO app_category "
    "(package_name, category, sub_category"
    ") "
    "VALUES (?, ?, ?)"
    )

for i in range(10000):
    cursor.execute(add_data, (aa.iloc[i][0].decode('utf8'), aa.iloc[i][1].decode('utf8'),
                              aa.iloc[i][2].decode('utf8')) )

conn.commit()
cursor.close()

conn.close()

{% endhighlight %}


###参考文献
1. [使用SQLite][ref1]
2. [SQLite教程][ref2]

[ref1]: http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001388320596292f925f46d56ef4c80a1c9d8e47e2d5711000

[ref2]: http://www.runoob.com/sqlite/sqlite-tutorial.html

