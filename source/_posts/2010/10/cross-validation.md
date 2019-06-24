---
title: 交叉验证
date: 2010-10-27 23:49:20
updated: 2010-10-27 23:49:20
categories: 知识积累
tags:
  - Machine Learning
  - Cross Validation
---

> **交叉验证(Cross-Validation):** 有时亦称循环估计， 是一种统计学上将数据样本切割成较小子集的实用方法。于是可以先在一个子集上做分析， 而其它子集则用来做后续对此分析的确认及验证。 一开始的子集被称为训练集。而其它的子集则被称为验证集或测试集。[WIKI](http://zh.wikipedia.org/zh-sg/%E4%BA%A4%E5%8F%89%E9%A9%97%E8%AD%89)

交叉验证对于人工智能，机器学习，模式识别，分类器等研究都具有很强的指导与验证意义。  
基本思想是把在某种意义下将原始数据(dataset)进行分组,一部分做为训练集(train set),另一部分做为验证集(validation set or test set),首先用训练集对分类器进行训练,在利用验证集来测试训练得到的模型(model),以此来做为评价分类器的性能指标.

<!-- more -->

# 三大CV的方法

## Hold-Out Method**

### 方法

将原始数据随机分为两组,一组做为训练集,一组做为验证集,利用训练集训练分类器,然后利用验证集验证模型,记录最后的分类准确率为此Hold-OutMethod下分类器的性能指标.。Hold-OutMethod相对于K-fold Cross Validation 又称Double cross-validation ，或相对K-CV称 2-fold cross-validation(2-CV)

### 优缺点

* **优点**: 好处的处理简单,只需随机把原始数据分为两组即可
* **缺点**: 严格意义来说Hold-Out Method并不能算是CV,因为这种方法没有达到交叉的思想,由于是随机的将原始数据分组,所以最后验证集分类准确率的高低与原始数据的分组有很大的关系,所以这种方法得到的结果其实并不具有说服性.(主要原因是 训练集样本数太少，通常不足以代表母体样本的分布，导致 test 阶段辨识率容易出现明显落差。此外，2-CV 中一分为二的分子集方法的变异度大，往往无法达到「实验过程必须可以被复制」的要求。)

## K-fold Cross Validation(记为K-CV)

### 方法

**作为**1)**的演进，将原始数据分成K组(一般是均分),将每个子集数据分别做一次验证集,其余的K-1组子集数据作为训练集,这样会得到K个模型,用这K个模型最终的验证集的分类准确率的平均数作为此K-CV下分类器的性能指标.K一般大于等于2,实际操作时一般从3开始取,只有在原始数据集合数据量小的时候才会尝试取2. 而K-CV 的实验共需要建立 k 个models，并计算 k 次 test sets 的平均辨识率。在实作上，k 要够大才能使各回合中的 训练样本数够多，一般而言 k=10 (作为一个经验参数)算是相当足够了。

![K-fold Cross Validation - A 5-fold cross validation method](https://asset.vanjor.com/images/006tNbRwly1fynsizoaylg30bp07xdfv.gif)

### 优缺点

* **优点**: K-CV可以有效的避免过学习以及欠学习状态的发生,最后得到的结果也比较具有说服性.
* **缺点**: K值选取上

## Leave-One-Out Cross Validation(记为LOO-CV)

### 方法

如果设原始数据有N个样本,那么LOO-CV就是N-CV,即每个样本单独作为验证集,其余的N-1个样本作为训练集,所以LOO-CV会得到N个模型,用这N个模型最终的验证集的分类准确率的平均数作为此下LOO-CV分类器的性能指标.

### 优缺点

* **优点**: 相比于前面的K-CV,LOO-CV有两个明显的优点：**a.**每一回合中几乎所有的样本皆用于训练模型,因此最接近原始样本的分布,这样评估所得的结果比较可靠。   **b.** 实验过程中没有随机因素会影响实验数据,确保实验过程是可以被复制的.
* **缺点**: 计算成本高,因为需要建立的模型数量与原始数据样本数量相同,当原始数据样本数量相当多时,LOO-CV在实作上便有困难几乎就是不显示,除非每次训练分类器得到模型的速度很快,或是可以用并行化计算减少计算所需的时间.

在模式识别与机器学习的相关研究中，经常会将 数据集分为 训练集与测试集 这两个子集，前者用以建立 模式，后者则用来评估该 模式对未知样本进行预测时的精确度，正规的说法是 generalization ability(泛化能力)

# 交叉验证核心原则

Cross-validation 是为了有效的估测 generalization error 所设计的实验方法

> 只有训练集才可以用在 模式的训练过程中，测试集 则必须在模式完成之后才被用来评估 模式优劣的依据。

## 常见的错误运用

许多人在研究都有用到 Evolutionary Algorithms(EA,遗传算法)与 classifiers，所使用的 Fitness Function (适应度函数)中通常都有用到 classifier 的辨识率，然而把Cross-Validation 用错的案例还不少。前面说过，只有 training data 才可以用于 model 的建构，所以只有 training data 的辨识率才可以用在 fitness function 中。而 EA 是训练过程用来调整 model 最佳参数的方法，所以只有在 EA结束演化后，model 参数已经固定了，这时候才可以使用 test data。

* **EA 与 CV结合研究方法：** Cross-Validation 的本质是用来估测某个 classification method 对一组 dataset 的 generalization error，不是用来设计 classifier 的方法，所以 Cross-Validation 不能用在 EA的 fitness function 中，因为与 fitness function 有关的样本都属于 training set，那试问哪些样本才是 test set 呢？如果某个 fitness function 中用了Cross-Validation 的 training 或 test 辨识率，那么这样的实验方法已经不能称为 Cross-Validation .
* **EA 与 k-CV 正确的搭配方法**: 是将 dataset 分成 k 等份的 subsets 后，每次取 1份 subset 作为 test set，其余 k-1 份作为 training set，并且将该组 training set 套用到 EA 的 fitness function 计算中(至于该 training set 如何进一步利用则没有限制)。因此，正确的 k-CV 会进行共 k 次的 EA 演化，建立 k 个classifiers。而 k-CV 的 test 辨识率，则是 k 组 test sets 对应到 EA 训练所得的 k 个 classifiers 辨识率之平均值.

# 数据集分割原则

交叉验证在，原始数据集分割为训练集与测试集，必须遵守两个要点：

> 1. 训练集中样本数量必须够多，一般至少大于总样本数的 50%。
> 2. 两组子集必须从完整集合中均匀取样。

其中第 **2** 点特别重要，均匀取样的目的是希望减少 训练集/测试集 与完整集合之间的偏差(bias)，但却也不易做到。一般的作法是随机取样，当样本数量足够时，便可达到均匀取样的效果。然而随机也正是此作法的盲点，也是经常是可以在数据上做手脚的地方。举例来说，当辨识率不理想时，便重新取样一组训练集 与测试集，直到测试集的辨识率满意为止，但严格来说便算是作弊。

# 参考

* MATLAB中文: [http://www.ilovematlab.cn/viewthread.php?tid=49143](http://www.ilovematlab.cn/viewthread.php?tid=49143)
* SHAMO: [http://www.shamoxia.com/html/y2010/2245.html](http://www.shamoxia.com/html/y2010/2245.html)
* ProClassify: [http://genome.tugraz.at/proclassify/help/pages/XV.html](http://genome.tugraz.at/proclassify/help/pages/XV.html)