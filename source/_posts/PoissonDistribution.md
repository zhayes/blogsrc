---
title: Poisson Distribution
date: 2019-9-01 19:33:31
category: 数学
tags: 泊松分布
mathjax: true
---

![](http://sasnrd.com/wp-content/uploads/2017/08/Poisson_PMF.png)

<!-- more -->

转自 [如何理解泊松分布？](https://www.matongxue.com/madocs/858/)

#### 1 甜在心馒头店
公司楼下有家馒头店：

![](https://img-blog.csdn.net/20180720091046462?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

每天早上六点到十点营业，生意挺好，就是发愁一个事情，应该准备多少个馒头才能既不浪费又能充分供应？

老板统计了一周每日卖出的馒头（为了方便计算和讲解，缩小了数据）：


![](https://www.zhihu.com/equation?tex=%5Cbegin%7Barray%7D%7Bc%7Cc%7D%20%5Cqquad%5Cqquad%26%5Cqquad%E9%94%80%E5%94%AE%5Cqquad%5C%5C%5Chline%5Ccolor%7BSkyBlue%7D%7B%E5%91%A8%E4%B8%80%7D%26%203%20%5C%5C%20%5Chline%20%5Ccolor%7Bblue%7D%7B%E5%91%A8%E4%BA%8C%7D%26%207%20%5C%5C%20%5Chline%20%5Ccolor%7Borange%7D%7B%E5%91%A8%E4%B8%89%7D%264%5C%5C%5Chline%20%5Ccolor%7BGoldenrod%7D%7B%E5%91%A8%E5%9B%9B%7D%266%5C%5C%20%5Chline%20%5Ccolor%7Bgreen%7D%7B%E5%91%A8%E4%BA%94%7D%265%5C%5C%5Cend%7Barray%7D%5C%5C)

均值为：
$$\overline{X}=\frac{3+7+4+6+5}{5}=5$$

按道理讲均值是不错的选择（参见“如何理解最小二乘法？”），但是如果每天准备5个馒头的话，从统计表来看，至少有两天不够卖，$40\%$的时间不够卖：


![](https://www.zhihu.com/equation?tex=%5Cbegin%7Barray%7D%7Bc%7Cc%7D%5Cqquad%5Cqquad%26%5Cqquad%E9%94%80%E5%94%AE%5Cqquad%26%5Cquad%E5%A4%87%E8%B4%A7%E4%BA%94%E4%B8%AA%5C%5C%5Chline%5Ccolor%7BSkyBlue%7D%7B%E5%91%A8%E4%B8%80%7D%26%203%20%5C%5C%5Chline%20%5Ccolor%7Bblue%7D%7B%E5%91%A8%E4%BA%8C%7D%26%207%26%5Ccolor%7Bred%7D%7B%E4%B8%8D%E5%A4%9F%7D%20%5C%5C%20%5Chline%20%5Ccolor%7Borange%7D%7B%E5%91%A8%E4%B8%89%7D%264%5C%5C%20%5Chline%20%5Ccolor%7BGoldenrod%7D%7B%E5%91%A8%E5%9B%9B%7D%266%26%5Ccolor%7Bred%7D%7B%E4%B8%8D%E5%A4%9F%7D%5C%5C%5Chline%20%5Ccolor%7Bgreen%7D%7B%E5%91%A8%E4%BA%94%7D%265%5C%5C%5Cend%7Barray%7D%5C%5C)

你“甜在心馒头店”又不是小米，搞什么饥饿营销啊？老板当然也知道这一点，就拿起纸笔来开始思考。

#### 2 老板的思考
老板尝试把营业时间抽象为一根线段，把这段时间用T来表示：

![](https://img-blog.csdn.net/2018072009112033?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后把$\color{red}{周一}$的三个馒头（甜在心馒头是有褶子的馒头）按照销售时间放在线段上：

![](https://img-blog.csdn.net/20180720091134821?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

把T均分为四个时间段：

![](https://img-blog.csdn.net/2018072009114717?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

此时，在每一个时间段上，要不卖出了（一个）馒头，要不没有卖出：


![](https://img-blog.csdn.net/20180720091156857?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

在每个时间段，就有点像抛硬币，要不是正面（卖出），要不是反面（没有卖出）：

![](https://img-blog.csdn.net/20180720091209939?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

T内那么卖出3个馒头的概率，就和抛了4次硬币（4个时间段），其中3次正面（卖出3个）的概率一样了。



这样的概率通过二项分布来计算就是：

$$\binom{4}{3}p^3(1-p)^1$$

但是，如果把$\color{red}{周二}$的七个馒头放在线段上，分成四段就不够了：

![](https://img-blog.csdn.net/2018072009122362?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从图中看，每个时间段，有卖出3个的，有卖出2个的，有卖出1个的，就不再是单纯的“卖出、没卖出”了。不能套用二项分布了。

解决这个问题也很简单，把T分为20个时间段，那么每个时间段就又变为了抛硬币：

![](https://img-blog.csdn.net/20180720091237705?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这样，T内卖出7个馒头的概率就是（相当于抛了20次硬币，出现7次正面）：

$$\binom{20}{7}p^7(1-p)^{13}$$

为了保证在一个时间段内只会发生“卖出、没卖出”，干脆把时间切成n份：

$$\binom{n}{7}p^7(1-p)^{n-7}$$

越细越好，用极限来表示：

$$\lim_{n\to\infty}\binom{n}{7}p^7(1-p)^{n-7}$$

更抽象一点，T时刻内卖出k个馒头的概率为：

$$\lim_{n\to\infty}\binom{n}{k}p^k(1-p)^{n-k}$$

#### 3 $p$的计算
“那么”，老板用笔敲了敲桌子，“只剩下一个问题，概率$p$怎么求？”

在上面的假设下，问题已经被转为了二项分布。二项分布的期望为：

$$E(X)=np=\mu$$

那么：

$$p=\frac{\mu}{n}$$

#### 4 泊松分布
有了$P=\frac{u}{n}$了之后，就有：

$$\lim_{n\to\infty}\binom{n}{k}p^k(1-p)^{n-k}=\lim_{n\to\infty}\binom{n}{k}\frac{\mu}{n}^k(1-\frac{\mu}{n})^{n-k}$$

我们来算一下这个极限：


$$\lim_{n\to\infty}\binom{n}{k}\frac{\mu}{n}^k(1-\frac{\mu}{n})^{n-k}$$
$$= \lim_{n\to\infty}\frac{n(n-1)(n-2)\cdots(n-k+1)}{k!}\frac{\mu^k}{n^k}\left(1-\frac{\mu}{n}\right)^{n-k}$$
$$=\lim_{n\to\infty}\frac{\mu^k}{k!}\frac{n}{n}\cdot\frac{n-1}{n}\cdots\frac{n-k+1}{n}\left(1-\frac{\mu}{n}\right)^{-k}\left(1-\frac{\mu}{n}\right)^n$$

其中：


$$\lim_{n\to\infty}\frac{n}{n}\cdot\frac{n-1}{n}\cdots\frac{n-k+1}{n}\left(1-\frac{\mu}{n}\right)^{-k}=1$$

$$\lim_{n \to \infty}\left(1-\frac{\mu}{n}\right)^n = e^{-\mu}$$

所以：


$$\lim_{n\to\infty}\binom{n}{k}\frac{\mu}{n}^k(1-\frac{\mu}{n})^{n-k}=\frac{\mu^k}{k!}e^{-\mu}$$

上面就是泊松分布的概率密度函数，也就是说，在T时间内卖出k个馒头的概率为：

$$P(X=k)=\frac{\mu^k}{k!}e^{-\mu}$$

一般来说，我们会换一个符号，让$\mu=\lambda$，所以：

$$P(X=k)=\frac{\lambda^k}{k!}e^{-\lambda}$$

这就是教科书中的泊松分布的概率密度函数。

#### 5 馒头店的问题的解决
老板依然蹙眉，不知道$\mu$啊？

没关系，刚才不是计算了样本均值：

$$\overline{X}=5$$

可以用它来近似：

$$\overline{X}\approx\mu$$

于是：

$$P(X=k)=\frac{5^k}{k!}e^{-5}$$

画出概率质量函数的曲线就是：

![](https://img-blog.csdn.net/20180720091300482?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看到，如果每天准备8个馒头的话，那么足够卖的概率就是把前8个的概率加起来：

![](https://img-blog.csdn.net/20180720091309679?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjbnRfMjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这样$93\%$的情况够用，偶尔卖缺货也有助于品牌形象。

老板算出一脑门的汗，“那就这么定了！”

