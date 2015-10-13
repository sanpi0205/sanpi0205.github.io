---
layout: post
title:  "使用 sublime3 进行前端开发"
date:   2015-10-12 22:44:40
categories: 开发工具
---

### sublime3 使用笔记

#### 安装sublime 3

从官网下载 [sublime3], 当前稳定版本为2，但3增加很多性特性，建议使用sublime3.

#### 安装 emmet 插件

emmet前身为大名鼎鼎的zen Coding， 安装 emmet 可以从 Package control
中进行安装。

* ` ctrl + ～ `
{% highlight html %}
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp =   sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
{% endhighlight %}
* Package ctrol 中选择 `Install package`
* 安装 `emmet`

sublime 安装插件介绍 [package]

[sublime3]: http://www.sublimetext.com
[package]: http://jingyan.baidu.com/article/4d58d541caeeaa9dd4e9c093.html