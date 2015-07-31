---
layout: post
title:  "R to scrape web"
date:   2015-07-30 14:44:40
categories: R
---

### Python上传数据至数据库

{% highlight ruby %}


import mysql.connector
import xlrd

#使用dict数据
db_config = {'user':'root', 'password':'', 'host':'localhost', 'database':'db_name'}

cnx = mysql.connector.connect(**db_config)
cursor = cnx.cursor()

add_data = (
    "INSERT INTO table_name "
    "(sector_id, type_id, data_year, sector_name, sector_name_en, "
    "type_name, data_value, data_unit, data_category"
    ")"
    "VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s)"
    )

def read_data(file_name, sheet_name):
    book = xlrd.open_workbook(file_name) #打开数据文件
    sheet = book.sheet_by_name(sheet_name) #提取数据表
    nrows = sheet.nrows                      #提取表行数
    for row_index in range(1, nrows):
        row = sheet.row_values(row_index)
        if row[7] == "" :
            row[7] = 0
        upload_to_database(row[1:], row_index)
                
def upload_to_database(row, row_index):
    cursor.execute(add_data, row)
    if row_index % 1000 == 0:
        print(row_index, " 行数据已经上传")
    cnx.commit()

read_data("data.xlsm", 'data')

cursor.close()
cnx.close()

{% endhighlight %}