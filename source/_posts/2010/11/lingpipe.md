---
title: Java开源自然语言处理-LingPipe
date: 2010-11-08 20:00:00
updated: 2010-11-08 20:00:00
categories: 知识积累
tags:
  - machine learning
  - LingPipe
---

[LingPipe](http://alias-i.com/lingpipe/) 是[Alias-i](http://alias-i.com/)公司开发的一款自然语言处理开源Java软件包，目前最高版本是4.0.1

<!-- more -->

# LingPipe的优势

* 比较全面的覆盖自然语言处理的各个分支，文本分词，聚类，语义情感分析，领域知识学习等等
* 具有全套在research上免费的源码，样列代码，测试代码(商业与非商业均同一套代码)，并且文档详细，对于其中模型所参考的论文都引用出来，适合研究学习.
* 作为相对开源资源缺少的领域，项目一直持续更新中.

# LinePipe包含的模块

* **主题分类（Top Classification）**: 基于文本语言模型训练，归类
* **命名实体识别（Named Entity Recognition）**: 基于first-best, n-best and per-entity confidence modes识别，以及训练与评估识别器
* **聚类（Clustering）**: 基于single-link and complete-link多层聚类，包裹一些聚类评估技术
* **词性标注（Part-of Speech Tagging）**
* **句题检测（Sentence Detection）**
* **拼写更正（Spelling Correction）**:基于"你要找的是" 风格的检查引擎
* **数据库文本挖掘（Database Text Mining）**
* **字符串比较(String Comparison)**: 基于距离与相似度测量，包括权重距离，TF/IDF距离，Jaccard distance, Jaro-Winkler distance, 等
* **兴趣短语检测（Interseting Phrase Detection）**
* **字符语言建模（Character Language Modeling）**
* **中文分词（Chinese Word Segmentation）**: 基于空格分割类似训练库，机器学习，发现认知新词
* **数据库文本挖掘（Database Text Mining）**
* **情感分析（Sentiment Analysis）**: 基于文本聚类
* **断字识音（Hyphenation and Syllabification）**
* **语言辨别（Language Identification）**
* **奇异值分解（Singular Value Decomposition）**
* **逻辑回归 （Logistic Regression）**
* **期望最大化（Expectation Maximization）**
* **词义排歧（Word Sense Disambiguation）**

# LingPipe包含资源

* Source： [http://alias-i.com/lingpipe/web/download.html](http://alias-i.com/lingpipe/web/download.html)
* API Docs: [http://alias-i.com/lingpipe/docs/api/index.html](http://alias-i.com/lingpipe/docs/api/index.html)
* Tutorials: [http://alias-i.com/lingpipe/demos/tutorial/read-me.html](http://alias-i.com/lingpipe/demos/tutorial/read-me.html)
* Papaer&language material : source，介绍中均包含有所引用资源

目前个人应用LingPipe包中的中文分词，结合情感分析模块研究中文情感检测与辨别。API接口均已高度概括化，便于快速实现，不过所运用的算法需要详尽的分析。

# 参考

* [**LingPipe**](http://alias-i.com/lingpipe/): [http://alias-i.com/lingpipe](http://alias-i.com/lingpipe)