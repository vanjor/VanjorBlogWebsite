---
title: 情感计算-基于规则分类
date: 2010-12-27 16:58:00
updated: 2010-12-27 16:58:00
categories: 知识积累
tags:
  - Classifier
  - Machine Learning
  - Sentiment Analysis
---
![sentiment analysis](https://ws3.sinaimg.cn/large/006tNbRwly1fyo064xl7ij308g03ca9y.jpg)

情感计算中，很重要的一个问题就是为目标情感数据进行分类，先行的很多文本分类技术与文本聚类技术SVM，K-means等，背后都是利用一些先决的条件，比如SVM的先决条件是建立文本向量空间VSM上。而这些文本分类技术都很少涉及到语义语法分析，或者建立十分微弱的语义语法分析基础上，比如上篇文章《[评论潜在方面观点计算](/2010/12/latent-aspect-rating-analysis/)》。

本文为学习论文《[Sentiment Analysis: A Combined Approach](http://linkinghub.elsevier.com/retrieve/pii/S1751157709000108)》，提取其中主要的方法-基于规则分类。 着重为基于规则分类RBC与基于统计分类SBC。

<!-- more -->

# 基于规则分类基础分析

一个规则包含一个先行词（antecedent）和关联的后项（consequent），并有着“如果-那么”的关联：

> 先行词 =》 后项

一个先行词定义一个条件（condition），并且由一个标记（token）或一组由^连接的标记序列。

一个标记可以是一个单词，“？”表示一个专有名词，“#”表示一个目标术语。一个目标术语是一个表示文档集中出现的内容，例如一个政客的名字，一个政策主张，一个公司名称，一个产品平台或者电影标题。一个连续术语表示一个情感是术语积极或者消极的一种，并且条件最后的结果由先行词定义：

```math
{token1 ^ token2 ^ . . . ^ tokenn} =) {+|−}
```

如下这两个简单的规则取决于两个情感支撑词，每个代表一个先行词。

```math
{excellent} =) {+}  
{absurd} =) {−}
```

假设我们有两个句子.

1. Laptop-A is more expensive than Laptop-B.
2. Laptop-A is more expensive than Laptop-C.

并且这些句子的目标词为Laptop-A ，有这些句子推导出的规则为：

```math
{# ^ more ^ expensive ^ than^?} =) {−}
```

这个规则可以解释为：目标词：Laptop-A相对另两个laptops在价格方面相对更少受喜爱的，由以上规则表达，这里，聚焦在Laptop-A的价格特性上。

最为比对，假设目标此为Laptop-B和Laptop-C，有以上句子推导出的规则如下：

```math
> {? ^ more ^ expensive ^ than ^ #} =) {+}
```

这个规则可以解释为：两个目标词，Laptop-B和Laptop-C相对价格方面比LaptopA更受欢迎。由以上规则表达。这里，聚焦在Laptop-B和Laptop-C的价格特性上。

很明显，一个目标词是一个决定情感中的先行词的一个重要因素。在这个程度上，我们可以关注在获取和定义先行词和后项集来形成一组规则集，反应一组代表文档集内容的目标词，评估4个不同的分类器，不同的都应用了一套规则。我们同样考虑否定在内，'not’, ‘neither nor’和‘no’,相对来说，我们扫描文档中的每一个句子，也就是，基于句子层级。每个先行词由句子推导出来。

# 通用基于获取分类器（General Inquirer Based Classifier (GIBC)）

首先，简单的规则集是基于在General Inquirer Lexicon(Stone et al. 1966)中的3672个预先分类好的词，其中1598个预先归类为积极，另2074个预先归类为消极。这里，每个规则唯一取决于代表每个先行词的情感支撑词。我们应用General Inquirer Based Classifier (GIBC)到规则集，来分类文档集。

通用基于获取分类器其实也就是一些自动分类机的概念，大致为假定对应的文档中出现情感词的频率可以反映概括为文档总体情感。

# 基于规则分类器（Rule-Based Classifier (RBC)）

给定一个预先分类好的文档集，第二个规则集由将每个专有名词取代为“？”或“#”来形成一组先行词，并且为每个先行词赋予情感（规则的形成）。这样，基本的假设是为每一个先行词赋予的情感等同于为对应文档赋予情感。这样我们基于RBC实现第二种规则来对文档分类。

有争议的一点是，先行词表达的情感可能与文档的情感不同。因此，我们采用一个情感计算工具（Sentiment Analysis Tool (SAT)），可以以半自动方式来矫正情感。

如下步骤用来生成先行词集。

The Montylingua (Liu 2004) chunker用来解析文档集中句子，给定这些解析后的句子，一组专有名词，也就是，所有标有NNP和NNPS的术语，被自动鉴别出，并替换“？”，为了减少解析的错误率，我们自动的扫描，并测试所有由Montylingua识别出的专有名词，比对在WordNet 2.0 (Miller 1995)中的所有名词（NN，NNS），当Montylingua中的专有名词在WordNet中为标准名词，这个专有名词将被视为不正确的标注，将不会被“？”所替代。另外，所有目标词都被替代为“#”，这样，一组先行词由此生成。一个后缀数组(Manber & Myers 1990)用来加速先行词匹配。

# 基于统计分类器（SBC）

基于统计分类机（SBC）使用一组规则集，基于如下假设：糟糕的表达式与词“poor”具有更高的**共现率（co-occur frequency）**，并且好的表达式与词语“excellent”具有更好个共现率(Turney 2002).

1. 我们计算一个由先行词组成的表达与情感支撑词集见的紧密度，如下步骤用来定义先行词的结果：
2. 从General Inquirer Lexicon中挑选120个积极的词语，比如amazing, awesome, beautiful，以及120个消极词语，比如absurd, angry, anguish
3. 为每一个先行词，分别与这240个词语组合一个查询，提交给搜索引擎，每个查询包含有一个先行词与情感支撑词。
4. 收集所有提交给Google与Yahoo搜索引擎的查询结果中的命中数，两个搜索引擎用来分析命中数是或由搜索引擎各自的覆盖性与精确性所决定。对于每个查询，我们预期搜索引擎返回同时包含有先行词与情感支撑词的网页文档数目。这里，紧密程度计算是基于网页级别。一个更好的精确计算可能从基于句子级别获得。目前来说比较困难，我们需要从搜索引擎下载并存储每一篇网页以备分析。
5. 为每个情感支撑词与先行词收集命中数
6. 使用四种测量度来测量每个先行词分别于120个积极词（S+）以及消极词（S-）间的紧密度。

![sentiment analysis](https://ws4.sinaimg.cn/large/006tNbRwly1fyo07n4lp7j3072030wec.jpg)

如果先行词共现率S+ \> S- ,那么，以为这先行词为积极的，反之S+ < S-则判定先行词为消极的，而S+ = S-则判定为中性，

紧密测量度涉及到如下概念：

## 文档频率（Document Frequency (DF)）

包含目标词的文档占总共文档的比率：如下：

一个包含N个文档的文档集集的共现矩阵

![rbc](https://ws3.sinaimg.cn/large/006tNbRwly1fyo0bovdxlj30cm03c3yf.jpg)

## 互信息（Mutual Information (MI)）

一个先行词与对应情感支撑词的互信息MI表示如下：

![mi](https://ws2.sinaimg.cn/large/006tNbRwly1fyo0c4h6lsj30cm01mjra.jpg)

更大的MI值，代表先行词与word见更高强度的关联，并且MI(antecedent,word)必须大于

0，意味着联合概率，P(antecedent,word)必须大于P(antecedent)与P(word)概率组合。在Conrad & Utt(1994) 和Church & Hanks (1989)两篇论文中均运用到MI测量术语间的关联度。

## 卡方分布（Chi-square (χ2)）

给定2*2列联表，一个表包含观察到的频率，另一包含预期的频率，那么词语对应先行词的卡方值计算如下：

![chi](https://ws3.sinaimg.cn/large/006tNbRwly1fyo0cb9po8j304a01m742.jpg)

其中 i = {a . . . d} 表示2*2列联表中的每个方格值。[耶茨连续性校正](http://www.nciku.cn/search/zh/detail/%E8%80%B6%E8%8C%A8%E8%BF%9E%E7%BB%AD%E6%80%A7%E6%A0%A1%E6%AD%A3/645089)（The Yates continuity correction）被应用到每个开放计算中，并且自由度为1。更大的卡方值，意味着更有利证明来剔除虚假设，并排除之，意味着词语和先行词之间互相独立，对于卡方测试，为了可信接受或拒绝H0，预期的值应该大于5，否则，趋向于低于小概率可能性，将不正确的结果纳入H1(Cochran 1954).

## 对数似然比（Log Likelihood Ratio）(−2 · log)

对数似然比计算如下：

![log](https://ws2.sinaimg.cn/large/006tNbRwly1fyo0cnfay4j309u03jt8l.jpg)

其中i = {a, b, c, d}并且 j = {c1, c2, r1, r2}. 对数似然比(Dunning 1993)与卡方假设类似，也就是，更大的对数似然比，意味着更有利证明来剔除虚假设。不同于卡方，对数似然比是比卡方更为精确的处理稀有事件。

举例说明，对于如下具体的数据：

![rbc](https://ws4.sinaimg.cn/large/006tNbRwly1fyo0ctbv2ej308g02pjr9.jpg)

* DF=40，也就是文档中同时出现先行词与词语的文档数。![rbc](https://ws1.sinaimg.cn/large/006tNbRwly1fyo0d31i1oj309h011dfn.jpg)
* 为了计算卡方，预期的频率，E计算出如下: ![rbc](https://ws4.sinaimg.cn/large/006tNbRwly1fyo0dbphmhj308g02rmx0.jpg) ![rbc](https://ws2.sinaimg.cn/large/006tNbRwly1fyo0duzipej305k01a3ya.jpg)
* 极大似然比为 ![rbc](https://ws3.sinaimg.cn/large/006tNbRwly1fyo0ehviukj30bb018743.jpg)

# 基于规则归纳分类器（Induction Rule Based Classifier (IRBC)）

给定两组规则集RBC与SBC，我们应用已有的两种归纳算法ID3 (Quinlan 1986) 和由Weka (Witten & Frank 2005)提供的RIPPER (Cohen 1995)的归纳算法。来生成两组归纳规则几，并建立一个分类器采用这两个归纳规则分类机来对文本集进行分类。

这两个归纳规则集可以表明归纳算法对于无控制的先行词集的效果，也就是这些先行词所表示的属性没有预定义，但是可以简单的从预先分类文档中归纳出。并且可以预期这种归纳规则集的效果将会拥有更好的准确率与召回率。

最后，论文中提出结合SVM等方法的一种组合分类方法流程图如下：
![rbc](https://ws2.sinaimg.cn/large/006tNbRwly1fyo0fpprsbj30ec08q3ys.jpg)