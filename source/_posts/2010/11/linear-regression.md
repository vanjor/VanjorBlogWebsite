---
title: 线性回归
date: 2010-11-10 16:06:21
updated: 2010-11-10 16:06:21
categories: 知识积累
tags:
  - Linear Regression
  - Machine Learning
  - Classifier
---

# 线性回归-Linear regression

> 在统计学中，线性回归是利用称为线性回归方程的[最小二乘](http://zh.wikipedia.org/zh-cn/%E6%9C%80%E5%B0%8F%E4%BA%8C%E4%B9%98%E6%B3%95)函数对一个或多个自变量和因变量之间关系进行建模的一种回归分析。这种函数是一个或多个称为回归系数的模型参数的线性组合\[1\]

<!-- more -->

![一元线性回归-Linear_regression](https://asset.vanjor.com/images/006tNbRwly1fynsw1b1adj30b807tdfu.jpg)

带一个自变量的的线性回归：一元线性回归

通俗来说：所谓线性回归模型就是指因变量和自变量之间的关系是直线型的。

# 回归分析

[回归分析](http://wiki.mbalib.com/wiki/%E5%9B%9E%E5%BD%92%E5%88%86%E6%9E%90)是对客观事物数量依存关系的分析．是[数理统计](http://wiki.mbalib.com/wiki/%E6%95%B0%E7%90%86%E7%BB%9F%E8%AE%A1)中的一个常用的方法．是处理多个变量之间相互关系的一种数学方法\[3\]

> 回归分析是一种统计学上对数据进行分析的方法，主要是希望探讨数据之间是否有一种特定关系。回归分析是建立因变量Y（或称依变量response variables, dependent variables）与自变量X（或称独变量，predictors, independent variables）之间关系的模型。目的在于了解两个或多个变量间是否相关、相关方向与强度，并建立数学模型以便观察特定变量来预测研究者感兴趣的变量\[4\]

而线性回归是回归分析中的首要的一种分析研究方法，并广泛应用在各项实践分析领域。

# 多元线性回归预测模型

给定一个n组统计单元样本数据集 ![math](https://asset.vanjor.com/images/006tNbRwly1fynthkoen3j304g00la9t.jpg)，一个线性回归模型假设因变量yi与p纬回归变量xi之间近似线性关系，这个近似关联模型建立通过引入一个误差项_εi_ (也是一个随机变量），来捕获除了x自变量之外任何对_Y__i_的影响。这样模型可以建立为：

![math](https://asset.vanjor.com/images/006tNbRwly1fyntirsan6j30dc00nq2q.jpg)

以向量方式可以表示为如下：

![math](https://asset.vanjor.com/images/006tNbRwly1fyntqatoh5j302w00i0sh.jpg)

其中：

![math](https://asset.vanjor.com/images/006tNbRwly1fyntoav203j30ho031jrc.jpg)

一般在科学论文研究中，做如下统一：

yi 称为因变量或从属变量（_regressand_）, xii称为回归量，自变量（_regressor_）

注：其中X通常会包含一个常数项,(不同于误差项_εi_ )，这时，观测值矩阵为\[1\] ：

![math](https://asset.vanjor.com/images/006tNbRwly1fynts0r0ksj305z031dfn.jpg)

(如果_X_列之间存在线性相关，那麽参数向量β就不能以最小二乘法估计除非β被限制，比如要求它的一些元素之和为0)

# 古典假设

在线性回归模型理论中，样本是在总体之中随机抽取出来的。因变量在实直线上是连续的，误差项是独立同分布的，也就说，残差是i.i.d.(独立同分布，independent and identically distributed)且服从高斯分布。这些假设意味着残差项不依赖自变量的值，所以和自变量（预测变量）之间是相互独立的。

在这些假设下，建立一个显示线性回归作为条件预期模型的简单线性回归，可以表示为：

![math](https://asset.vanjor.com/images/006tNbRwly1fynttlwcykj306100lq2p.jpg)

# 最小二乘法估计(OLS)

最小二乘法([Ordinary least squares](http://en.wikipedia.org/wiki/Ordinary_least_squares)\[6\])，是一种简洁并且常用的线性回归估计方法，回归分析的最初目的是估计模型的参数以便达到对数据的最佳拟合。在决定一个最佳拟合的不同标准之中，普通最小二乘法是非常优越的。

![math](https://asset.vanjor.com/images/006tNbRwly1fyntvejl8sj30ab00tjr7.jpg)

其中，最小二乘法是建立在无偏一致估计，建立在古典假设，认为![math](https://asset.vanjor.com/images/006tNbRwly1fyntwxaae1j302u00k3y9.jpg)之上。

在得到参数的最小二乘法的估计值之后，需要进行必要的检验与评价，以决定模型是否可以应用。关于最小二乘法估计的回归推断见[线性回归](http://zh.wikipedia.org/zh-cn/線性回歸)\[1\] ，多元回归模型的检验见多元线性回归分析预测法[3]

其他估计方法有：[Generalized least squares](http://en.wikipedia.org/wiki/Generalized_least_squares) (GLS)，[Iteratively reweighted least squares](http://en.wikipedia.org/wiki/Iteratively_reweighted_least_squares) (IRLS)等\[2\]

具体一元线性回归分析预测法分析见[一元线性回归预测法](http://wiki.mbalib.com/wiki/一元线性回归预测法)\[5\]

# 参考

1. [http://zh.wikipedia.org/zh-cn/線性回歸](http://zh.wikipedia.org/zh-cn/線性回歸 "http://zh.wikipedia.org/zh-cn/線性回歸")
2. [http://en.wikipedia.org/wiki/Linear_regression](http://en.wikipedia.org/wiki/Linear_regression "http://en.wikipedia.org/wiki/Linear_regression")
3. [http://wiki.mbalib.com/wiki/线性回归预测法](http://wiki.mbalib.com/w/index.php?title=%E7%BA%BF%E6%80%A7%E5%9B%9E%E5%BD%92%E5%88%86%E6%9E%90%E9%A2%84%E6%B5%8B%E6%B3%95 "http://wiki.mbalib.com/wiki/线性回归预测法")
4. [http://zh.wikipedia.org/zh-cn/迴歸分析](http://zh.wikipedia.org/zh-cn/迴歸分析 "http://zh.wikipedia.org/zh-cn/迴歸分析")
5. [http://wiki.mbalib.com/wiki/一元线性回归预测法](http://wiki.mbalib.com/wiki/一元线性回归预测法 "http://wiki.mbalib.com/wiki/一元线性回归预测法")
6. [http://en.wikipedia.org/wiki/Ordinary\_least\_squares](http://en.wikipedia.org/wiki/Ordinary_least_squares)