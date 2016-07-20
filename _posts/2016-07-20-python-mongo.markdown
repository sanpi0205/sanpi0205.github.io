---
layout: post
title:  "Python 操作数据库"
date:   2016-07-19 16:44:40
categories: Python
---

Python有各种各样的库来完成对接不同数据库，如mysql, sqlite, mongo等SQL或NoSQL数据库。
本文作为代码总结，介绍Python中各种数据库的基本操作方法。

## Python操作Mongo
{% highlight python %}
import pymongo

#建立数据库连接
client = pymongo.MongoClient(localhost, 27017) 
db = client.database
collection = db.document

#读数据
#https://api.mongodb.com/python/current/
results = collection.find({})

#写数据
collection.insert_many([ {'_id': user, 'data': data} for user, data in data_dict.items()])

#bulk API方法
#在涉及批量更新时，mongo提供bulk方法实现大批量的插入、更新和删除。

for user, data in data_dict.items():
    bulk.find({'_id': user}).upsert().update({'$set': {'data': data}})
bulk.execute()


{% endhighlight %}




参见: [官网指南][pymongo bulk]


[pymongo bulk]: https://api.mongodb.com/python/current/examples/bulk.html


