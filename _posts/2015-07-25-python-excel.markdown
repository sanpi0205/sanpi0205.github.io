---
layout: post
title:  "Python读取excel"
date:   2015-07-25 15:28:40
categories: Python
---

### 安装openpyxl
本文使用Python中读取excel包openpyxl，安装 openpyxl
{% highlight ruby %}
pip install openpyxl   # 或者
conda install openpyxl # 使用 Anaconda IDE
{% endhighlight %}

### 加载excel文件
{% highlight ruby %}
from openpyxl import load_workbook
wb = load_workbook('test.xlsx')
{% endhighlight %}

列出所有excel中所有表：`print( wb.get_sheet_names() )`

### 读取数据
读取数据表data
{% highlight ruby %}
ws = wb['data']
{% endhighlight %}

读取单元格数据
{% highlight ruby %}
c = ws['A4']  #等同于
c = ws.cell(row = 4, column = 1)
print( c.value )
{% endhighlight %}

读取spreadsheet区域数据
{% highlight ruby %}
cell_range = ws['A1':'C2']
for row in cell_range:
	pass
{% endhighlight %}

[参考文献：openpyxl][参考文献]

[参考文献]: https://openpyxl.readthedocs.org/en/latest/index.html
