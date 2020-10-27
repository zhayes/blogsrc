---
title: 几何平均数
date: 2019-08-18 12:48:56
category: 数学
tags: 几何平均数
description: The Geometric Mean is a special type of average where we multiply the numbers together and then take a square root (for two numbers), cube root (for three numbers) etc.
---

原文链接：https://www.mathsisfun.com/numbers/geometric-mean.html

The Geometric Mean is a special type of average where we multiply the numbers together and then take a square root (for two numbers), cube root (for three numbers) etc.

Example: What is the Geometric Mean of 2 and 18?
First we multiply them: 2 × 18 = 36
Then (as there are two numbers) take the square root: √36 = 6
In one line:

```
Geometric Mean of 2 and 18 = √(2 × 18) = 6
```

It is like the area is the same!

![](https://www.mathsisfun.com/numbers/images/geometric-mean-2.svg)

Example: What is the Geometric Mean of 10, 51.2 and 8?

First we multiply them: 10 × 51.2 × 8 = 4096
Then (as there are three numbers) take the cube root: 3√4096 = 16
In one line:

```
Geometric Mean = 3√(10 × 51.2 × 8) = 16
```

It is like the volume is the same:

![](https://www.mathsisfun.com/numbers/images/geometric-mean-3.svg)

Example: What is the Geometric Mean of 1, 3, 9, 27 and 81?
First we multiply them: 1 × 3 × 9 × 27 × 81 = 59049
Then (as there are 5 numbers) take the 5th root: 5√59049 = 9
In one line:

```
Geometric Mean = 5√(1 × 3 × 9 × 27 × 81) = 9
```

I can't show you a nice picture of this, but it is still true that:

```
1 × 3 × 9 × 27 × 81  =  9 × 9 × 9 × 9 × 9
```

Example: What is the Geometric Mean of a Molecule(分子) and a Mountain

![](https://www.mathsisfun.com/measure/images/length-continuum.svg)

Using [scientific notation](https://www.mathsisfun.com/numbers/scientific-notation.html):

A molecule of water (for example) is 0.275 × 10-9 m
Mount Everest (for example) is 8.8 × 103 m

```
Geometric Mean	= √(0.275 × 10-9 × 8.8 × 103)
    = √(2.42 × 10-6)
    ≈ 0.0016 m
```

Which is 1.6 millimeters, or about the thickness of a coin.

We could say, in a rough kind of way,

### "a millimeter is half-way between a molecule and a mountain!"

So the geometric mean gives us a way of finding a value in between widely different values.

### Definition
For n numbers: multiply them all together and then take the nth root (written `n√` )
More formally, the geometric mean of n numbers a1 to an is:
```
n√(a1 × a2 × ... × an)
```

### Useful
The Geometric Mean is useful when we want to compare things with very different properties.

top view camera
Example: you want to buy a new camera.
One camera has a zoom of 200 and gets an 8 in reviews,
The other has a zoom of 250 and gets a 6 in reviews.
Comparing using the usual arithmetic mean gives `(200+8)/2 = 104 vs (250+6)/2 = 128`. The zoom is such a big number that the user rating gets lost.

But the geometric means of the two cameras are:
```
√(200 × 8) = 40
√(250 × 6) = 38.7...
```
So, even though the zoom is 50 bigger, the lower user rating of 6 is still important.


## 几何平均数的几何意义及与算术平均数的关系的几何解释

原文链接：https://baijiahao.baidu.com/s?id=1608753027046746833&wfr=spider&for=pc

我们看两个数的情况。设有两个数a、b，其算术平均数为`（a +b）/2`，几何平均数为`√ab`，两者关系为`（a+ b）/2≥√ab`。下面我们从几何上来理解几何平均数的含义及算术平均数与几何平均数的关系。

![](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=2608767289,4111623120&fm=173&app=25&f=JPEG?w=370&h=227&s=6B443A625605DAA81B5535DA0000E091)

如图1，⊿ABD为圆内三角形，AB为直径（故⊿ABD必为直角三角形），DD'垂直于AB，并将AB分成长为a、 b的 两段（注意DD'不一定为直径），现在若已知a,、b 两段的长度，问CD长度为几何？与a 、b有何关系？因为⊿ABD、⊿ACD、⊿CBD均为直角三角形，故此有以下关系：
```
CD^2+ a^2 = AD^2；
CD^2 + b^2 = BD^2；
AD^2 + BD^2 = AB^2 =（a+b）^2
```
于是有：`CD^2+ a^2 + CD^2 + b^2= a^2+b^2+2ab`等式两边化简，得：`CD^2= ab`即：`CD= √ab`。另外，由上图可见，`（a +b）/2 ≥ √ab`（仅当DD'为直径时相等）。问题得以证明。



## 算术平均数 vs 几何平均数

原文链接：https://zhuanlan.zhihu.com/p/23809612

首先明确一点，几何平均数和算数平均数其实都是一种衡量平均水平的统计方法，之所以计算方法有差别，是因为数据类型的不同导致。 


无论任何平均数，其意义都是：对于数列用某一个常数A将数列中的每一项替换，形成的新数列结果上与旧数列等效。这个常数A，就是数列An的平均数。 

那么什么时候用算数平均数什么时候用几何平均数呢，我们来考虑以下的情形：你发现最近二师兄（猪肉）身价猛涨，于是投入了10万元本金开始卖猪肉，果然第一年赚得盆满钵满的，赚了5万元，本金变成了10+5=15万元。但是好景不长，商品的供需周期的变化，第二年的时候很多猪肉供应商开始进入猪肉市场，供应的大量增加导致猪肉价格快速下跌，第二年亏了7.5万元，本金变成了15-7.5=7.5万元。 

现在我们想计算下述两个平均数： 

#### 1. 两年利润的平均数 

这种类型的平均数最终结果是一个和，第一年利润是5万元，第二年利润是-7.5万元，最终结果是亏了2.5万元，年均亏1.25万元。 
两年赚的钱构成的数列{A1,A2} 
根据平均数的定义，为构造一个数列与其等效{A,A} 
那么这个等效数列
![](https://www.zhihu.com/equation?tex=A%3D+%5Cfrac%7BA_%7B1%7D%2BA_%7B2%7D%7D%7B2%7D+%3D%5Cfrac%7B5%2B%28-7.5%29%7D%7B2%7D%3D-1.25)
万元
也就是第一年赚5万元并且第二年亏7.5万元和两年都亏1.25万元等效。 

#### 2. 两年本金变化的平均数 

这种类型的平均数最终结果是一个积，第一年初本金是10万元，第一年末变成15万元增长到了1.5倍，第二年末变成7.5万元下降到了0.5倍。 
两年本金增长倍数{A1,A2} 
根据平均数的定义，为构造一个数列与其等效{A,A} 
那么这个等效数列
![](https://www.zhihu.com/equation?tex=A%3D%5Csqrt%7BA_%7B1%7D%5Ctimes+A_%7B2%7D+%7D%3D+%5Csqrt%7B1.5%5Ctimes+0.5+%7D%3D0.87)
也就是第一年本金增长到1.5倍并且第二年下降到0.5倍和两年都下降到0.87倍等效。 


总结来说，当数据最终结果是一个和时，用算术平均数更合适，当数据最终结果是一个积时，用几何平均数更加合适。所以一般在算增长率的时候，用几何平均数更加合适。 

算术平均数
![](https://www.zhihu.com/equation?tex=A%3D%5Cfrac%7BA_%7B1%7D%2BA_%7B2%7D%2B...%2BA_%7Bn%7D+%7D%7Bn%7D+)
几何平均数
![](https://www.zhihu.com/equation?tex=A%3D%5Csqrt%5Bn%5D%7BA_%7B1%7D+%5Ctimes+A_%7B2%7D+%5Ctimes+...%5Ctimes+A_%7Bn%7D+%7D+)

注意几何平均数里如果是增长率的话用的公式是
![](https://www.zhihu.com/equation?tex=A%3D%5Csqrt%5Bn%5D%7B%281%2BA_%7B1%7D%29%5Ctimes+%281%2BA_%7B2%7D%29%5Ctimes+...%281%2BA_%7BN%7D%29%5C+%7D+-1)

例如上述例题中增长率
![](https://www.zhihu.com/equation?tex=A%3D%5Csqrt%7B%281%2B0.5%29%5Ctimes+%281-0.5%29+%7D+-1)
如果平均数用得不合适，容易导致荒谬的结论。举个例子，假如一只股票价格第一年初价格为10元，第一年增长了100%变成了20元，第二年下降了50%变成了10元，在算平均增长率时 

几何平均数
![](https://www.zhihu.com/equation?tex=A%3D%5Csqrt%7B%281%2B0.5%29%5Ctimes+%281-0.5%29+%7D+-1)

算数平均数
![](https://www.zhihu.com/equation?tex=A%3D%5Cfrac%7B1%2B0.5%7D%7B2%7D+%3D0.75)

几何平均数才是更加合理的答案，因为这个股票第一年初价格为10元，第二年末价格也是10元，增长率为0%，算术平均数算出来的75%就显得很荒谬。 



#### 后记： 
1. 平均数还有一种叫做调和平均数，计算公式为：
![](https://www.zhihu.com/equation?tex=A%3D%5Cfrac%7BN%7D%7B%5Cfrac%7B1%7D%7BA_%7B1%7D+%7D%2B%5Cfrac%7B1%7D%7BA_%7B2%7D+%7D%2B...%5Cfrac%7B1%7D%7BA_%7Bn%7D+%7D+%7D+)
适用的例子是我们初中物理中学到的并联电路中各个并联电阻构成的电路总电阻的计算。各电阻为A1,A2,…,An并联和A,A,…,A并联构成的电路总电阻等效。

2. 根据不等式的性质可以证明：`算术平均数 > 几何平均数 > 调和平均数`