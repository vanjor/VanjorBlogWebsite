---
title: 向量空间模型VSM
date: 2010-11-09 13:18:09
updated: 2010-11-09 13:18:09
categories: 知识积累
tags:
  - Information Retrieval
  - Vector Space Model
---

**向量空间模型 (VSM：Vector Space Model)** 是一个应用于资讯过滤, 资讯撷取, 索引以及评估相关性的代数模型。由Salton等人于60年代提出，并成功地应用于著名的SMART文本检索系统。

<!-- more -->

# VSM概念

文件(语料)被视为索引词(关键字)形成的多次元向量空间， 索引词的集合通常为文件中至少出现过一次的词组。在文本检索中，文档与查询词可以表示为以下向量空间模型[1] :

```math
dj = (w1,j,w2,j,...,wt,j)

q = (w1,q,w2,q,...,wt,q)
```

搜寻时，输入的检索词q会被转换成类似于文件的向量，这个模型假设，文件和搜寻词的相关程度，可以经由比较每个文件(向量)和检索词(向量)的夹角偏差程度而得知。 由此两个文档向量空间的夹角余弦为:

![向量空间模型（VSM）](https://asset.vanjor.com/images/006tNbRwly1fynz2hn4byj304s01pmwx.jpg)

而对应检索词q与文档集中文档d2的向量空间夹角余弦为 :

![vector](https://asset.vanjor.com/images/006tNbRwly1fynz2tu1ddj304k01umwx.jpg)

在坐标系中如图:

![向量空间模型-Vector Space Model](https://asset.vanjor.com/images/006tNbRwly1fynz35rqdlj306805emx1.jpg)

(余弦为零表示检索词向量垂直于文件向量，即没有符合，也就是说该文件不含此检索词)

# VSM优势

向量空间模型相对标准布尔模型，优势在于:

1. 基于线性代数的简单模型
2. 权重非简单的二值化
3. 可以在查询与文档集见计算一个连续的相似度
4. 可以按照文档集间的关联度做排序
5. 可以进行局部匹配

# VSM局限性

1. 不适合处理过长的文件，因为近似值不理想（过小的[标量积](http://zh.wikipedia.org/zh-cn/%E6%A0%87%E9%87%8F%E7%A7%AF)以及过高的次元)。
2. 检索词组必须要完全符合文件中出现的词组；不完整词组(子字串)会会生[false positive](http://zh.wikipedia.org/w/index.php?title=False_positive&action=edit&redlink=1)。
3. 语言敏感度不佳；情境相同但使用不同语汇的文件无法被关连起来，这产生所谓的[false negative](http://zh.wikipedia.org/w/index.php?title=False_negative&action=edit&redlink=1)
4. 无法反应词语见的出现的顺序关联性
5. 模型假设在词语特性均各自独立上
6. 权重计算比较偏直觉经验上，而非十分正式

**VSM通俗来说缺点**:

* 它的缺点是相似度的计算量大，当有新文档加入时，则必须重新计算词的权值；
* 不适合处理过长的文件，因为近似值不理想；
* 检索词组必须要完全符合文件中出现的词组，不完整词组(子字串)会会生false positive；
* 语言敏感度不佳，情境相同但使用不同语汇的文件无法被关联起来，这产生所谓的false negative。

# VSM中的关键词(Term)

向量空间模型是基于关键词标量模型的，关键词对于区分文档的作用是不同的。例如一些虚词对于区分文档的内容与查询是否相关并没有多大的意义。

对于概率模型而言，可以有完备的理论来估计每篇文档生成某个词的概率，因而其主要工作集中于如何确定较好的概率估计方法。而对于向量 空间模型来说，确定关键词权重在很大程度上依赖于研究者的经验及对文档特性的分析。

目前，对关键词权重的确定方法一般都需要获取一些关于关键词的统计量，而后根据这些统计量，应用某种认为规定的计算公式来得到权重。 最常用的统计量包括：

* **tf**，(Term Frequency), 表示某个关键词在某个文档中出现的频率。
* **qtf**，(Query Term Frequency). 表示查询中某关键词的出现频率。
* **N**，(Num), 集合中的文档总数
* **df**，(Document Frequency), 的缩写，表示文档集合中，出现某个关键词的文档个数。
* **idf**，(Inversed Document Frequency), 的缩写。
* **dl**，(Document Length), 文档长度
* **adl**，(Average Document Length), 平均文档长度

# VSM指导思想

在向量空间模型下，构造关键词权重计算公式有三个基本原则：

1. 如果一个关键词在某个文档中出现次数越多，那么这个词应该被认为越重要。
2. 如果一个关键词在越多的文档中出现，那么这个词区分文档的作用就越低，于是其重要性也应当相应降低。
3. 一篇文档越长，那么其出现某个关键词的次数可能越高，而每个关键词对这个文档的区分作用也越低，相应的应该对这些关键词予以一定的折扣。

# 扩展模型

基于向量空间模型上的扩展模型有

Models based on and extending the vector space model include:

* [Generalized vector space model](http://en.wikipedia.org/wiki/Generalized_vector_space_model)
* [(enhanced) Topic-based Vector Space Model](http://en.wikipedia.org/wiki/Topic-based_vector_space_model)
* [Latent semantic analysis](http://en.wikipedia.org/wiki/Latent_semantic_analysis)
* [Latent semantic indexing](http://en.wikipedia.org/wiki/Latent_semantic_indexing)
* [DSIR model](http://en.wikipedia.org/w/index.php?title=DSIR_model&action=edit&redlink=1)
* [Term Discrimination](http://en.wikipedia.org/wiki/Term_Discrimination)
* [Rocchio Classification](http://en.wikipedia.org/wiki/Rocchio_Classification)

**参考:**

1. WIKI, Vector Space Model：[http://en.wikipedia.org/wiki/Vector\_space\_model](http://en.wikipedia.org/wiki/Vector_space_model)
2. SEO-VSM: [http://www.dugutianjiao.com/post/vector-space-model-seo.html](http://www.dugutianjiao.com/post/vector-space-model-seo.html)
3. COGSYS,VSM:[http://cogsys.imm.dtu.dk/thor/projects/multimedia/textmining/node5.html](http://cogsys.imm.dtu.dk/thor/projects/multimedia/textmining/node5.html)