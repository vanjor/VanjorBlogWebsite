---
title: TF-IDF统计
date: 2010-11-09 13:04:21
updated: 2010-11-09 13:04:21
categories: 知识积累
tags:
  - TF-IDF
  - Information Retrieval
---

**TF-IDF**（Term Frequency – Inverse Document Frequency）

TF-IDF是一种用于[资讯检索](http://zh.wikipedia.org/zh-cn/%E8%B3%87%E8%A8%8A%E6%AA%A2%E7%B4%A2)与[文本挖掘](http://zh.wikipedia.org/zh-cn/%E6%96%87%E6%9C%AC%E6%8C%96%E6%8E%98)的常用加权技术。TF-IDF是一种统计方法，用以评估一字词对于一个文件集或一个语料库中的其中一份文件的重要程度，也是建立在[向量空间模型理论](/2010/11/vector-space-model/)中的一种统计技术。

字词的重要性随着它在文件中出现的次数成正比增加，但同时会随着它在语料库中出现的频率成反比下降。TF-IDF加权的各种形式常被搜索引擎应用，作为文件与用户查询之间相关程度的度量或评级。除了TF-IDF以外，互联网上的搜寻引擎还会使用基于链接分析(PR)的评级方法，以确定文件在搜寻结果中出现的顺序。

<!-- more -->

# 公式概念

在一个文本集中，对于一份给定的文件

**词频**（Term Frequency，TF）指的是某一个给定的词语在该文件中出现的次数。这个数字通常会被正规化，以防止它偏向长的文件。（同一个词语在长文件里可能会比短文件有更高的词频，而不管该词语重要与否。）对于在某一特定文件里的词语 _t__i_ 来说，它的词频(重要性)可表示为：

![tfidf](https://asset.vanjor.com/images/006tNbRwly1fynys23fwdj303v01qmwx.jpg)

(以上式子中 ni,j 是该词在文件dj中的出现次数，而分母则是在文件dj中所有字词的出现次数之和)

**逆向文件频率**（Inverse Document Frequency，IDF）是一个词语普遍重要性的度量。某一特定词语的IDF，可以由总文件数目除以包含该词语之文件的数目，再将得到的商取对数得到：

![tfidf](https://asset.vanjor.com/images/006tNbRwly1fynysdlg06j305t01vwe9.jpg)

(其中:

* | D |：语料库中的文件总数
* ![tfidf](https://asset.vanjor.com/images/006tNbRwly1fynyuhu0g4j302t00k3y9.jpg)包含词语_t__i_的文件数目（即 **ni,j != 0**的文件数目）,如果关键词不在语料库中，这会导致除零错误，这种情况通常用1+![tfidf](https://asset.vanjor.com/images/006tNbRwly1fynyvjgwckj302t00k3y9.jpg)来代替。

# TF-IDF权重：

由此，TF，IDF权重为TF，IDF的乘积

![tfidf](https://asset.vanjor.com/images/006tNbRwly1fynyzr2vxuj30g402gt8l.jpg)

为在整个文档集中，关键词Term i（最小单元）在文档j中的权重。某一特定文件内的高词语频率，以及该词语在整个文件集合中的低文件频率，可以产生出高权重的TF-IDF。因此，TF-IDF倾向于过滤掉常见的词语，保留重要的词语。

# 基于TF-IDF余弦相似度

查询q与文档dj的余弦相似度可以表示为:

![tf-idf-cosine](https://asset.vanjor.com/images/006tNbRwly1fynyphga3bj30ci02g0sj.jpg)

其中，i为q与文档dj 把q视作一个文档向量，i为dj 与q中的每一个元关键词标量。

# TF-IDF的理论依据与不足

TF-IDF算法是建立在这样一个假设之上的:

> 对区别文档最有意义的词语应该是那些在文档中出现频率高，而在整个文档集合的其他文档中出现频率少的词语，所以如果特征空间坐标系取TF词频作为测度，就可以体现同类文本的特点。
>
> 另外考虑到单词区别不同类别的能力，TF-IDF法认为一个单词出现的文本频数越小，它区别不同类别文本的能力就越大。因此引入了逆文本频度IDF的概念，以TF和IDF的乘积作为特征空间坐标系的取值测度，并用它完成对权值TF的调整，调整权值的目的在于突出重要单词，抑制次要单词。

但是在本质上IDF是一种试图抑制噪音的加权 ，并且单纯地认为文本频数小的单词就越重要，文本频数大的单词就越无用，显然这并不是完全正确的。IDF的简单结构并不能有效地反映单词的重要程度和特征词的分布情况，使其无法很好地完成对权值调整的功能，所以TF-IDF法的精度并不是很高。

此外，在TFIDF算法中并没有体现出单词的位置信息，这也是空间向量模型的不足点。对于Web文档而言，权重的计算方法应该体现出HTML的结构特征。特征词在不同的标记符中对文章内容的反映程度不同，其权重的计算方法也应不同。因此应该对于处于网页不同位置的特征词分别赋予不同的系数，然后乘以特征词的词频，以提高文本表示的效果。

# 参考

1. WIKI,TF-IDF: [http://en.wikipedia.org/wiki/Tf–idf](http://en.wikipedia.org/wiki/Tf–idf "http://en.wikipedia.org/wiki/Tf–idf")
2. BAIDU,TF-IDF: [http://baike.baidu.com/view/1228847.html](http://baike.baidu.com/view/1228847.html "http://baike.baidu.com/view/1228847.html")