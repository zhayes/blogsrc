---
title: Python内置函数zip
date: 2018-12-11 20:17:48
category: 后台
tags: python
---
![](https://images.unsplash.com/photo-1515879218367-8466d910aaa4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1200&q=40)
python内置函数里有一个zip方法。第一次看起来有点不懂，于是查了下资料，找到一篇不错的文章，便想记录分享一下。[原文出处链接点这里](http://www.cnblogs.com/frydsh/archive/2012/07/10/2585370.html)。此外我再补充个[栈溢出上的问题讨论地址](https://stackoverflow.com/questions/13704860/zip-lists-in-python)。
<!-- more -->
------

zip函数接受任意多个（包括0个和1个）序列作为参数，返回一个tuple列表。具体意思不好用文字来表述，直接看示例：

```python
x = [1, 2, 3]

y = [4, 5, 6]

z = [7, 8, 9]

xyz = zip(x, y, z)

print xyz
```

运行的结果是：

[(1, 4, 7), (2, 5, 8), (3, 6, 9)]

从这个结果可以看出zip函数的基本运作方式。

2.示例2：

```python
x = [1, 2, 3]
y = [4, 5, 6, 7]
xy = zip(x, y)
print xy
```
运行的结果是：

[(1, 4), (2, 5), (3, 6)]

从这个结果可以看出zip函数的长度处理方式。

3.示例3：

```python
x = [1, 2, 3]
x = zip(x)
print x
```
运行的结果是：

[(1,), (2,), (3,)]

从这个结果可以看出zip函数在只有一个参数时运作的方式。

4.示例4：

python
```
x = zip()
print x
```
运行的结果是：

[]

从这个结果可以看出zip函数在没有参数时运作的方式。

5.示例5：

```python
x = [1, 2, 3]

y = [4, 5, 6]

z = [7, 8, 9]

xyz = zip(x, y, z)

u = zip(*xyz)

print u
```
运行的结果是：

[(1, 2, 3), (4, 5, 6), (7, 8, 9)]

一般认为这是一个unzip的过程，它的运行机制是这样的：

在运行zip(*xyz)之前，xyz的值是：[(1, 4, 7), (2, 5, 8), (3, 6, 9)]

那么，zip(*xyz) 等价于 zip((1, 4, 7), (2, 5, 8), (3, 6, 9))

所以，运行结果是：[(1, 2, 3), (4, 5, 6), (7, 8, 9)]

#### 注：在函数调用中使用*list/tuple的方式表示将list/tuple分开，作为位置参数传递给对应函数（前提是对应函数支持不定个数的位置参数）

6.示例6：
```python
x = [1, 2, 3]
r = zip(* [x] * 3)
print r
```
运行的结果是：

[(1, 1, 1), (2, 2, 2), (3, 3, 3)]

它的运行机制是这样的：

[x]生成一个列表的列表，它只有一个元素x

[x] * 3生成一个列表的列表，它有3个元素，[x, x, x]

zip(* [x] * 3)的意思就明确了，zip(x, x, x)