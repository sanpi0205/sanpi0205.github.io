---
layout: post
title:  "R连接MySQL"
date:   2015-07-24 15:28:40
categories: MySQL
---

## RODBC
在Windows中用自带的ODBC建立数据库连接，然后在R中安装RODBC包，就可以读取和操作各种数据库。
当然也可以再MAC中使用ODBC，但相对更加复杂一些，MAC中安装ROBDC包需要编译，可能会出现错误信息：
{% highlight ruby %}
	configure: error: "ODBC headers sql.h and sqlext.h not found"
	ERROR: configuration failed for package ‘RODBC’
{% endhighlight %}



R读取Mysql
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve --watch`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
