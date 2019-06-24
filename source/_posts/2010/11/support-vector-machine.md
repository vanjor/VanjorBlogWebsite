---
title: 支持向量机SVM
date: 2010-11-11 22:45:51
updated: 2010-11-11 22:45:51
categories: 知识积累
tags:
  - Machine Learning
  - SVM
  - Classifier
---

支持向量机 Support Vector Machine, 简称**SVM**（或SV机），是一种监督是学习的方法，广泛应用于统计分类及回归分析中。

![SVM](https://asset.vanjor.com/images/006tNbRwly1fynxkke2xtj306s04hjrg.jpg)

其中, **机**（machine,机器）实际上是一个算法。在[机器学习](http://zh.wikipedia.org/zh/机器学习)(ML)领域里，常把一些算法看做是一个机器。

<!-- more -->

# SVM定义

参考WIKI定义：

> **支持向量机** 将向量映射到一个更高维的空间裡，在这个空间里建立有一个最大间隔超平面。在分开数据的超平面的两边建有两个互相平行的超平面。分隔超平面使两个平行超平面的距离最大化。假定平行超平面间的距离或差距越大，分类器的总误差越小。

# SVM目的

分类是机器学习的一个常用手法，假设给定数据点（这些点不一定是R2 纬度集合中，可以是任意Rn 纬度中的店），每个都归属于某一个或几个类别，我们希望能够把这些点通过一个n-1维的[超平面](http://zh.wikipedia.org/wiki/%E8%B6%85%E5%B9%B3%E9%9D%A2)分开，通常这个被称为[线性分类器](http://zh.wikipedia.org/wiki/%E7%BA%BF%E6%80%A7%E5%88%86%E7%B1%BB%E5%99%A8)。

有很多分类器都符合这个要求，但是我们还希望找到分类最佳的平面，即使得属于两个不同类的数据点间隔最大的那个面，该面亦称为**最大间隔超平面**。如果我们能够找到这个面，那么这个分类器，就称为最大间隔分类器。**支持向量机目的也就是一种最大间隔分类器。**

![Svm_separating_hyperplanes](https://asset.vanjor.com/images/006tNbRwly1fynxll809ej307i05kmx0.jpg)

（有很多个分类器(超平面)可以把数据分开，但是只有一个能够达到最大分割。H3不能准确分割，H1能分割但不能做到最大分割，H2则是最大分割）

# SVM推理引入

我们考虑以下形式的样本点**D**（n个数量的如下形式点集）：

![svm](https://asset.vanjor.com/images/006tNbRwly1fynxnrltecj309u0163ya.jpg)

其中:

* **_ci_** 为1或−1 ,用以表示数据点属于哪个类.
* **xi** 是一个 **_p_ 维向量** ，其每个元素都被缩放到 [0, 1] 或 [-1, 1]. 缩放的目的是防止方差大的随机变量主导分类过程。

我们可以把这些数据称为“训练数据"，希望我们的支持向量机能够通过一个超平面正确的把他们分开。超平面的数学形式可以写作:

![svm](https://asset.vanjor.com/images/006tNbRwly1fynxoerb4gj303s012gld.jpg)

根据几何知识，我们知道**w**向量垂直于分类超平面。**w**与**x**为**内积**，加入位移**b**的目的是增加间隔。如果没有**b**的话，那超平面将不得不通过原点，限制了这个方法的灵活性

**由于我们要求最大间隔**，因此我们需要知道支持向量以及（与最佳超平面）平行的并且离支持向量最近的超平面。我们可以看到这些平行超平面可以由方程族：

![svm](https://asset.vanjor.com/images/006tNbRwly1fynxp8wykcj303n00zdfl.jpg) and ![svm](https://asset.vanjor.com/images/006tNbRwly1fynxpfvqh9j304600zdfl.jpg)

如果这些训练数据是线性可分的，那就可以找到这样两个超平面，在它们之间没有任何样本点并且这两个超平面之间的距离也最大。通过几何不难得到这两个超平面之间的距离是2/|_**w**_|，因此我们需要最小化 |_**w**_|。同时为了使得样本数据点都在超平面的间隔区以外，我们需要保证对于所有的_**i**_ 满足其中的一个条件：

![svm](https://asset.vanjor.com/images/006tNbRwly1fynxq27ufqj303j00h741.jpg)对于第一类 **xi**

![svm](https://asset.vanjor.com/images/006tNbRwly1fynxqoac9pj303x00h741.jpg) 对于第二类 **xi**

通过类别二元量 **Ci** 可以合并为：

![svm](https://asset.vanjor.com/images/006tNbRwly1fynxr5ih9wj309k00pwe9.jpg)

现在寻找最佳超平面这个问题就变成了在上式这个约束条件下最小化|_**w**_|. 这是一个[二次规划](http://zh.wikipedia.org/wiki/%E4%BA%8C%E6%AC%A1%E8%A7%84%E5%88%92)(QP：Quadratic Programming)最优化中的问题。

![Svm_max_sep_hyperplane_with_margin(最大间隔超平面，两类分割数据集)](https://asset.vanjor.com/images/006tNbRwly1fynxrj1i0uj308w08mt8n.jpg)

更清楚的表示

求最小化![svm](https://asset.vanjor.com/images/006tNbRwly1fynxs8u77yj3013010741.jpg)，并且满足限定条件![svm](https://asset.vanjor.com/images/006tNbRwly1fynxsi4kdbj309g00lq2p.jpg)
_(1/2这个因子是为了数学上表达的方便加上的)_

**解如上问题通常的想法可能是使用非负**[**拉格朗日乘数**](http://zh.wikipedia.org/wiki/%E6%8B%89%E6%A0%BC%E6%9C%97%E6%97%A5%E4%B9%98%E6%95%B0) **α_i_ 于下式**

![svm](https://asset.vanjor.com/images/006tNbRwly1fynxt27qdaj30a3023gle.jpg)

不过这样可能出错. 原因是：假如我们能找到一族超平面将这些点分割开来；那么所有的 ![svm](https://asset.vanjor.com/images/006tNbRwly1fynxzw4kqgj305000lgld.jpg). 因此我们可能通过将所有α_i_趋向正无穷大得到最小值, 此最小值对这一族内所有成员都有效，而不是解决原问题的最优解。

**但是可以将约束问题表示为：**（注：这一部分没弄得十分明白..）

![svm](https://asset.vanjor.com/images/006tNbRwly1fyny2cdc72j30ai01jmwz.jpg)

**从而化解问题为寻找一个鞍点（saddle point）**.

这样所有可以被![svm](https://asset.vanjor.com/images/006tNbRwly1fyny3a4sq3j304z00ldfl.jpg)分离的点就无关紧要了，因为我们必须设置相应的 αi 为零。

**这个问题现在可以用标准二次规划技术标准和程序解决**。结论可以表示为如下**训练向量的线性组合**

![svm](https://asset.vanjor.com/images/006tNbRwly1fyny0q17c3j303f01jjr5.jpg)

只有很少的 **αi** 会大于0. 相应的 **xi** 就是**支持向量**, 这些支持向量在边缘上并且满足 ![svm](https://asset.vanjor.com/images/006tNbRwly1fyny5134vfj304z00ldfl.jpg). 由此可以推导出支持向量也满足: ![svm](https://asset.vanjor.com/images/006tNbRwly1fyny5so8bpj30a000l3ya.jpg)因此允许定义偏移量_b_. 实际上此支持向量比一般_N__S__V_的支持向量鲁棒性更强:

![svm](https://asset.vanjor.com/images/006tNbRwly1fyny77l52ej305w01na9u.jpg)

## SVM改进模型-软间隔（Soft margin）

1995年, [Corinna Cortes](http://zh.wikipedia.org/w/index.php?title=Corinna_Cortes&action=edit&redlink=1)与Vapnik提出了一种改进的最大间隔区方法，这种方法可以处理标记错误的样本。如果可区分正负例的超平面不存在，则“软边界”将选择一个超平面尽可能清晰地区分样本，同时使其与分界最清晰的样本的距离最大化。这一成果使术语“支持向量机”（或“SVM”）得到推广。这种方法引入了松驰参数 **ξ**_**i**_ 以衡量对数据 **x** **_i_** 的误分类度。

![svm](https://asset.vanjor.com/images/006tNbRwly1fyny815832j308k00pt8h.jpg)

# SVM特性

SVM的特点可以总结为：

1. SVM学习问题可以表示为凸优化问题，因此可以利用已知的有效算法发现目标函数的全局最小值。而其他分类方法（如基于规则的分类器和人工神经网络）都采用一种基于贪心学习的策略来搜索假设空间，这种方法一般只能获得局部最优解。
2. SVM通过最大化决策边界的边缘来控制模型的能力。尽管如此，用户必须提供其他参数，如使用核函数类型和引入松弛变量等。
3. 通过对数据中每个分类属性引入一个哑变量，SVM可以应用与分类数据。
4. SVM不仅可以用在二类问题，还可以很好的处理多类问题，比如通过引入决策树模型。

# SVM实现

## SVM实现难点与核心

SVM的关键在于核函数。低维空间向量集通常难于划分，解决的方法是将它们映射到高维空间。但这个办法带来的困难就是计算复杂度的增加，而核函数正好巧妙地解决了这个问题。

也就是说，只要选用适当的核函数，就可以得到高维空间的分类函数。在SVM理论中，采用不同的核函数将导致不同的SVM算法。

## SVM开源实现工具LIBSVM

**LIBSVM**工具主页：[http://www.csie.ntu.edu.tw/~cjlin/libsvm/](http://www.csie.ntu.edu.tw/~cjlin/libsvm/ "http://www.csie.ntu.edu.tw/~cjlin/libsvm/")

是由台湾大学林智仁(Lin Chih-Jen)副教授等开发设计的一个简单、易于使用和快速有效的SVM模式识别与回归的软件包.

项目不但提供了编译好的可在Windows系列系统的执行文件，还提供了源代码，方便改进、修改以及在其它操作系统上应用；该软件对SVM所涉及的参数调节相对比较少，提供了很多的默认参数，利用这些默认参数可以解决很多问题；并提供了交互检验(Cross Validation)的功能。该软件可以解决C-SVM、ν-SVM、ε-SVR和ν-SVR等问题，包括基于一对一算法的多类模式识别问题

LIBSVM拥有Java、Matlab、C#、Ruby、Python、R、Perl、Common LISP、Labview等数十种语言版本。最常使用的是Matlab、Java和命令行的版本。

# 参考：

1. WIKI-SVM: [http://en.wikipedia.org/wiki/Support\_vector\_machine](http://en.wikipedia.org/wiki/Support_vector_machine "http://en.wikipedia.org/wiki/Support_vector_machine")
2. WIKI-支持向量机：[http://zh.wikipedia.org/zh/支持向量机](http://zh.wikipedia.org/zh/支持向量机 "http://zh.wikipedia.org/zh/支持向量机")
3. CSIE：[http://www.csie.ntu.edu.tw/~cjlin/libsvm/](http://www.csie.ntu.edu.tw/~cjlin/libsvm/ "http://www.csie.ntu.edu.tw/~cjlin/libsvm/")
4. 百科-SVM：[http://baike.baidu.com/view/960509.html](http://baike.baidu.com/view/960509.html "http://baike.baidu.com/view/960509.html")
5. 百科-支持向量机：[http://baike.baidu.com/view/541845.htm](http://baike.baidu.com/view/541845.htm "http://baike.baidu.com/view/541845.htm")
6. 百科-LIBSVM: [http://baike.baidu.com/view/598089.htm](http://baike.baidu.com/view/598089.htm "http://baike.baidu.com/view/598089.htm")