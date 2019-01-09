---
title: 在这做一些日常搜索的笔记
date: 2019-01-09 11:47:11
tags: Python
---
![](https://images.unsplash.com/photo-1484480974693-6ca0a78fb36b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1200&q=20)
<!-- more -->
## 在这做一些日常搜索的笔记

#### 当使用makemigrations时报错No changes detected：
(转https://blog.csdn.net/qq_39291784/article/details/78397589)

在修改了models.py后，有些用户会喜欢用python manage.py makemigrations生成对应的py代码。

但有时执行python manage.py makemigrations命令（也可能人比较皮，把migrations文件夹给删了），会提示"No changes detected." 可能有用的解决方式如下：

先 python manage.py makemigrations --empty yourappname 生成一个空的initial.py

再 python manage.py makemigrations 生成原先的model对应的migration file
