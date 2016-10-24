---
layout: post
title: "Statistical learning Week 2 评估模型精度"
date: 2016-10-18
author: Liu
category: Statistical Learning
tags: UTS Sydney ML SL
location: UTS Library
finished: true
---

Week 2 评估模型正确率
---

## Outline

> 评估模型精度
>
> - 拟合效果评估
> - 偏差与方差的权衡
> - 分类模型
> - 估计误差与近似误差

## 拟合效果评估

一种评估精度的方法是 __均方误差__ (Mean Squared Error, MSE)。

$$
MSE=\frac 1n\sum _{i=1}^n(y_i-\hat y_i)^2
$$

其中，$\hat y_i$是指模型$\hat f$在训练数据$x_i$上的预测结果。

MSE越小，说明效果越好。在训练数据上效果好，在测试数据上未必好(过拟合)。我们主要关注在 _测试数据_ 上的效果。

## 偏差与方差的权衡

__偏差__ (bias)是指模型的结果相对于真实函数的误差。一般来说，越复杂的模型bias越小。

$$
B(x)=|f_n(x)-f_p(x)|
$$

__方差__ (Variance)是指用一个不同的训练数据集估计$f$ 时，估计函数的改变量。一般来说，越复杂的模型，Variacne越大。

对任意给定的$X=x_0$,得到的$Y$的 __期望测试均方误差__ (expected test MSE)为

$$
ExpectedTest MSE=E(Y-f(x_0))^2=Bias^2+Var+\sigma ^2
$$

其中，$\sigma ^2$指的是不可避免的误差(Irreducible Error)。也就是说，期望测试均方误差能够分解为三个基本量的和，分别为$\hat f(x_0)$的偏差的平方和、方差和误差。

当一个模型越来越复杂时，它的bias将减小，variance将增大，而它的 expected test MSE 可能变大也可能变小。

![偏差与方差](/img/blog/20161014/2.PNG)

## 分类模型

对于回归问题，使用MSE来评价统计学习方法的精度。(原来上面都是针对的回归问题。。。)

对于分类问题，使用错误率(error rate)来评估。

$$
Error Rate = \sum _{i=1}^nI(y_i \neq \hat y_i)/n
$$

其中，$I(y_i \neq \hat y_i)$是一个指示性变量(Indicator Variable)，当不等时，值为1，否则等于0。

## 贝叶斯分类器(简单思想)

这个书上讲的不是很详细，主要资料来自网上以下链接。

[算法杂货铺——分类算法之朴素贝叶斯分类(Naive Bayesian classification)](http://www.cnblogs.com/leoo2sk/archive/2010/09/17/naive-bayesian-classifier.html)  
[朴素贝叶斯分类器和一般的贝叶斯分类器有什么区别？](https://www.zhihu.com/question/20138060)  
[朴素贝叶斯理论推导与三种常见模型](http://blog.csdn.net/u012162613/article/details/48323777)  

__条件概率__

$P(A|B)$表示事件B已经发生的前提下，事件A发生的概率，叫做事件B发生下事件A的 _条件概率_ 。基本公式为：

$$
P(A|B)=\frac {P(AB)}{P(B)}
$$

__全概率公式__  $P(A)= \sum _{i=1}^n P(B_i)P(A|B_i)$

__贝叶斯定理__

可能会遇到这样的问题，有$P(A|B)$，要求$P(B|A)$。贝叶斯定理如下

$$
P(B|A)=\frac {P(A|B)P(B)}{P(A)}
$$

__朴素贝叶斯分类(Naive Bayes Classifier)__ 

简单来说朴素贝叶斯就是：对于给出的待分类项，求解在此项出现的条件下各个类别出现的概率，哪个最大，就认为此待分类项属于哪个类别。

朴素贝叶斯法是基于 __贝叶斯定理__ 和 __特征条件独立假设__ 的分类方法，对于给定的训练数据集，首先基于特征条件独立假设学习输入/输出的联合分布概率；然后基于此模型，对给定的输入x，再利用贝叶斯定理求出其后验概率最大的输出y。

## 欧式距离、马式距离

都是用来计算相异度的。

__欧式距离__ 又叫欧几里得距离。计算公式如下：

$$
d(X,Y)= \sqrt {(x_1-y_1)^2+...+(x_n-y_n)^2}
$$

__马式距离__ 与欧式距离不同的是考虑到各种特性之间的联系，并且与尺度无关。对于一个均值为$\mu = (\mu _1, \mu _2,...,\mu _n)^T$，协方差矩阵为$\sum$的多变量向量$x=(x_1,x_2,...x_n)^T$,其马式距离为

$$
D_M(x)=\sqrt{(x-\mu)^T{\sum} ^{-1}(x-\mu)}
$$


## K均值算法(K-means)(简单思想)

[数据挖掘十大算法--K-均值聚类算法](http://blog.csdn.net/u011067360/article/details/24383051)

k-means算法，也被称为k-平均或k-均值，是一种得到最广泛使用的聚类算法。 它是将各个聚类子集内的所有数据样本的均值作为该聚类的代表点。

算法的主要思想是通过迭代过程把数据集划分为不同的类别，使得评价聚类性能的准则函数达到最优，从而使生成的每个聚类内紧凑，类间独立。这一算法不适合处理离散型属性，但是对于连续型具有较好的聚类效果。

k-means算法:  
输入：簇的数目k和包含n个对象的数据库。  
输出：k个簇，使平方误差准则最小。  
算法步骤：   

 1. 为每个聚类确定一个初始聚类中心，这样就有K 个初始聚类中心。 
 2. 将样本集中的样本按照最小距离原则分配到最邻近聚类  
 3. 使用每个聚类中的样本均值作为新的聚类中心。
 4. 重复步骤2.3直到聚类中心不再变化。
 5. 结束，得到K个聚类

## K近邻算法(KNN)(简单思想)

[数据挖掘十大算法--K近邻算法](http://blog.csdn.net/u011067360/article/details/23941577)  
[数据挖掘十大算法——kNN](https://www.douban.com/note/176119064/)

kNN算法则是从训练集中找到和新数据最接近的k条记录，然后根据他们的主要分类来决定新数据的类别。该算法涉及3个主要因素：训练集、距离或相似的衡量、k的大小。

计算步骤如下：  

    1. 算距离：给定测试对象，计算它与训练集中的每个对象的距离
    2. 找邻居：圈定距离最近的k个训练对象，作为测试对象的近邻
    3. 做分类：根据这k个近邻归属的主要类别，来对测试对象分类

## Estimation Error and Approximation Error (较难理解)

太难了。。没看懂。。。网上没找到合适的信息ORZ。

![估计误差与近似误差](/img/blog/20161014/3.jpg)

只记得前两种代表理想情况下最好的情况，后两种代表实际情况。


> 就是从这开始。。。就开始按PPT讲了@-@