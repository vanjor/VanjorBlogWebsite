---
title: 评论潜在方面观点计算
date: 2010-12-04 03:46:23
updated: 2010-12-04 03:46:23
categories: 知识积累
tags:
  - NLU
  - Machine Learning
---

![lara](https://ws1.sinaimg.cn/large/006tNbRwly1fyo032faiwj306s046t8u.jpg)

本文为国外09年的最新文本挖掘类别论文：

原文：Latent Aspect Rating Analysis on Review Text Data: A Rating Regression Approach

链接：[原文Paper](http://scholar.google.com.hk/scholar?q=Latent+Aspect+Rating+Analysis+on+Review+Text+Data:+A+Rating+Regression+Approach&hl=zh-CN&btnG=%E6%90%9C%E7%B4%A2&lr=)，[展示PPT](http://sifaka.cs.uiuc.edu/~wang296/paper/hongning-KDD10-v2.pptx)

个人三天时间完整翻译而成，本文对于理解话题识别，用户潜在观点挖掘，情感计算方面都有很好的借鉴意义。

目前在用户观点情感挖掘方面属于一个十分前沿的话题，广泛应用在产品研究，用户行为分析，推荐系统上。比现行的许多基于文本分类论文都是更为细致的研究，本文中大量运用统计概率学方面知识对话题识别，情感词的渐进识别，权重推断，以及结果估计验证，与应用探讨，值得深入学习。

个人认为一个最重要的不足的是，论文中还是主要通过挖掘文本中词语间的关联，类似tf/idf词频统计，先验概率推断等进行文本挖掘分析，而对于语义的理解，句法的解读分析仍然没有考虑在内，这样必然导致结果仍然存在很多偏差与误判，而鉴于语义理解，句法分析尚属一个十分困难的前沿研究领域。文本尤为可佳。

<!-- more -->

# 摘要

在本文，定义观点评估分析（LARA： Latent Aspect Rating Analysis）问题并针对含有观点意见的文本进行分析。 旨在分析在线评论中的实体，基于话题方面（topical aspects）来挖掘每个评论者在实体的每一个方面(Aspect)的潜在观点，以及分析不同方面对于评价者形成关于实体的总体评价所占权重。我们提出一种新颖概率回归评估模型来尝试以通法来解决这类文本挖掘问题。

基于酒店评论数据的经验分析实验表明这种提论可以有效的解决 LARA 问题，并且基于评论的具体挖掘与分析具有广泛的应用价值，包括类别观点概括，基于方面的实体评分，分析评论者评分表现行为。

* **分类：**信息搜索与检索：文本挖掘（_Text Mining_）
* **主要形式：**算法，实验
* **关键词：**意见与情感分析（_Opinion and sentiment analysis_），评论挖掘（_Review mining_），潜在评估分析（_Latent rating analysis_）

# 一、简介

随着 Web2.0 的发展，越来越多的人可以对各种各样的产品和服务自由的表达观点，这些评论信息对于其他用户做出决策以及产品服务的改进具有很大价值。然而，随着评论信息快速增长，海量的信息让用户难以快速查找到所需要的信息，很多工作就是来减轻这个评价文本信息抽取的问题[18,16,26],提炼总结用户的观点，根据意见的极性分类[20,6,7],并从评论中抽取相应的观点句。尽管如此，在现有的技术下，用户仍然难以方便的从海量的评论信息中挖掘与发现信息，来支撑实体主题方面的观点。

![lara](https://ws3.sinaimg.cn/large/006tNbRwly1fynzf0co6cj30dg05kt94.jpg)

以一个典型的酒店评价信息为例，如上图（1） ，这个评论信息涉及到了酒店的多个方面特点，包括价格，房屋条件以及服务，但是评论者只给出了宾馆的总体评分，没有提供每个单独方面的评分，其他用户就难以方便的了解到这个评论者在方面上的评级（_latent rating_）。透过整体评价进一步挖掘每一方面的评价是十分重要的，因为不同的评论者对于同一家酒店会有相同的总体评价，但是因为不同的方面原因。比如：一个评论者可能喜欢酒店的位置，另一个喜欢房间条件。

为了帮助用户发现这些不同，十分有必要挖掘并分析评论者在酒店的几个大的方面上的评级。此外，即使我们可以挖掘发现方面上的评价信息诸如“价格”，但仍然不够充分，因为“便宜”对于不同评论者有着不同的价格标准。而且及时同一个评论者可能因为其他方面的条件因素的诉求高低不同，而对"便宜"产生的不同的标准。为了理解如此微妙的差别，十分有必要挖掘发现评价者每一方面的评价与总体评价之间的权重关系。

为了进行对评论的更深入具体的的理解，我们尝试来研究这种新颖的文本挖掘问题 (LARA).

> **LARA任务目的： 给定一个含有总体评价信息的评论数据集，LARA旨在分析每个评论在不同话题方面的评论信息，来挖掘个体用户在每一个方面上的评级，以及不同的方面的评级对于形成总体评级的权重大小。**

LARA的广泛应用价值表现在：潜在方面的评级（aspect rating）可以用来进行面向方面的意见概述；每个方面的权重（aspect weight）可以方便于分析用户的评分行为；潜在方面评价与方面评分权重可以作为实体的个性化面向方面级别评估 - 通过汇集在对应方面具有相同权重的偏好的评论的评价信息。

现有的观点概括工作将 LARA 问题挖掘到一定程度，尚没有人在单个评论在方面层级上的潜在评价信息挖掘做过研究，也没有人考虑挖掘评论者在方面上的评级与总体评级间的权重关系。

在尝试解决这个新型文本挖掘时，我们提出一种基于新颖潜在评价回归模型的两阶段处理模型（_a novel latent rating regression model_）。

1. **第一阶段**: 我们采用 自引导算法（_bootstrapping-based algorithm_）来确定主要的方面（通过一些方面的特征词语做指引学习），并将评论集分段切割处理。
2. **第二阶段**: 我们提出一个通用潜在评价回归模型（LRR：_Latent Rating Regression_），旨在通过分析每个评论的文本与对应评论的总体评级数据信息，来确定每个评论的方面的评级与权重。

更具体的说：**LRR 的基本思想 就是假定总体评价完全产生来自于每个方面的潜在评级的有权重的组合。**

而且我们假定每个方面的潜在评级取决于对应评价中的讨论涉及到这一方面的文本片段。换句话说，我们将挖掘这些方面的潜在评级演化为一组特性词语的权重组合，这些词语权重对应为情感极性(Sentimental Polarities)。而因为我们无法直接观察到每一个方面的评级数据，所以这个回归模型的输出 - 响应结果是挖掘潜在的信息。

我们基于从TripAdviser ([www.tripadvisor.com](http://www.tripadvisor.com))爬取到的一组酒店评价数据集来评估 LRR 模型。实现结果表明 LRR 模型可以有效的从对应评价的总体评级中分析挖掘出方面上的评级，以及发现这些评论者者对于每个方面的评级相对于总体评级的权重。我们也将展示 LRR 模型中所分析到的结果说支持的不同应用，包括方面观点概括，个性化实体评级排序以及评论者的评级行为表现。

# 二、相关工作

在我们目前的调研中，还没有人做出关于LARA问题的研究，不过有些相关的工作。

分析评价文本的总体情感观点已经被广泛的研究。相关的研究基于文本的积极与消极的二元分类[6,7,20,5,14]。随后，这个定义被延伸到多元评级模型[10,9]。并且有提出的解决途径，包括监督，无监督，以及半监督的方法，但是他们都是尝试预测总体情感分类与评级，没有对潜在方面观点进行挖掘。

由于在线评论经常包含多个方面的的评价意见，一些近来工作开始挖掘基于方面层次的评级，来代替简单的总体评级。比如，Snyder et al.[23] 对于多元方面的建模，使用好的可信算法（_good grief algorithm_）可以改善对方面评级的预测。在[24]中，Titov et al. 尝试同时进行评论方面抽取与对应的评级计算：他们使用话题来描述方面，并且通过一个基于事实评级（ground-truth ratings）的回归模型。尽管如此，他们假定方面上的评级是明确包含在说提供的训练数据中。于此不同的是，我们假定方面评级是潜在的，并且是更普遍并面向现实场景的。

概括（_Summarization_）是一个十分广泛有用的应对信息过载的技术。一个最近的人类评估（_human evaluation_）[15] 指出情感与情绪概括具有很强的个体偏好性，相比于非情感方面，提出了情感建模以及方面观点概括的重要性。仍然，现有的基于方面的观点概括工作[10,21,18,26]只是目标停留在从评论集中聚合和展示话题上每个方面的主流观点。虽然聚合观点可以展现话题的概况信息，却导致个体评论的细节信息的丢失；此外，评论/评价者间的不同特性也没有在分析考虑范围内，这样情感的聚合是基于不同习性偏好的评论者间。Lu et al.[17]最近的一项工作是最接近于我们的，但是他们的目标仍然是从总体评级生成方面评级上的聚合概要。更为重要的是，没有已做工作考虑到评论者在不同方面上的强调程度的差异。

# 三、问题定义

在本章节里，我们规范定义潜在观点评级挖掘（LARA：_Latent Aspect Rating Analysis_）问题。划归为一个可计算的问题：

**LARA定义输入**：一些有价值实体（如:酒店）的一组评价数据集，并且每一条评价数据都有对应的总体评级信息（这些数据在现在的web2.0中广泛存在，中国的如时光网中电影的评论，大众点评）

**公式化定义**：假定  D={d1 , d2  , ...，d|D|}  作为一个实体（entity）或话题（topic）的一组评论文本集，并且每个评论文本 d∈D 都关联一个总体评级 **rd** 。我们也假定有一个含有 n 个互异的词汇集V={w1 ,w2 , ..., wn } 。

**定义：总体评级(_Overall Rating_)**

> 一个评价文本 **d** 总体评级 **rd** 是按级别的量化数值，也就是 rd∈[rmin , rmax]，其中**rmin**和  **rmax** 是评级的最大值与最小值。

最后我们假定我们已经知道给定 **k** 方面类别，并且每个方面评级都潜在影响给定话题的总体评级。例如：对于酒店的评级，可能的方面将包括“价格”和“位置”。每个方面都通过一组关键词界定关联，作为 LARA 问题的基础。

**定义：方面 (_Aspect_)**

> 一个方面 **Ai** ，并包含有评论中一组能对评级产生影响的描述词（通常数目不大）。

比如：词“price”，“value”和“worth”可以作为酒店实体的价格这个方面的特征词。我们把一个方面记做 **Ai = { w|w∈V, A(w)=i}** ,其中 A( . ) 是一个从词（word）到（aspect）的映射函数。

**定义：方面权重（_Acpect Weights_）**

> 方面权重 **αd** 是一个 **k** 维向量，并且对于每一维 \- 如第 **i** 维是一个数字量化刻度，指明对应评论文本 d 中的在方面 **Ai** 上的权重。

我们定义αdi ∈[0,1] ，且∑i=1k αdi =1 使得权重更加容易理解并且在评论集中具有更好的可比性。一个高的权重意味对应方面的具有更加重要的强调与偏好。

**定义：潜在观点评级分析（_Latent Aspect Rating Analysis_ (LARA))**

> 给定一个关于话题 **T** 的评论集 **D** ，每个 评论 **d** 关联对应的总体评级 **rd** ，并且话题 **T** 涵盖 **k** 个待分析方面 {A1 , A2  , ...，Ak} ，LRAR 问题目标在发现每个独立的评论文本在  **k**维方面的每一方面的评级 **s****di** ，以及对应的方面上的权重 ****α**di**

LARA 问题也包含发现那些可能存在的未知方面，并挖掘潜在方面评级/权重的工作。在本篇论文中，我们假定，对某个实体（_entity_）类型的每一个方面都有一些关键词描述特征。这个假设对于任何实体类型都具有很好现实可操作性，它的寄语人工标注主要方面特性是可行的；另外，这种初始化模式让用户能够灵活控制他所想要分析的方面。

# 四、方法

LARA 问题的一个主要挑战在于，对于每个方面的评级，我们没有详细的监督学习方法，即使我们有对应方面的一组特征关键词。另一个挑战在于如何发现方面的权重。为了解决这些问题，我们提出LRR 模型，并且假定评论者对于实体的总体评级完全取决于他对于每个方面的潜在评级的权重聚合，以及每个方面的评级取决于他的评论中对应含有的一组具有权重的描述词。

在对这些评论数据采用这个二折回归模型后，我们便可以获取潜在方面评级与权重，以此解决LARA 问题。

因为 LRR 模型中界定了评论中初始词语集与方面的对应关系是已知的，我们按照给定的描述方面关键词对一个评论的基于方面进行分割，以此为每个方面提取对应的切割后的评论文本片段。这样，我们的总体方法包括两阶段，将在之后作详细介绍:

## 4.1 基于方面的文本分割

第一步的目标是将评论中的句子与对应的方面建立映射关系。由于我们假定只有少部分的方面描述关键词指定出，我们设计出 自启动算法来为每个方面获取更多关联词语。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-

算法 : 基于方面的文本分割算法（_Aspect Segmentation Algorithm_）

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-

* **输入：**一个评论集合D={d1 , d2  , ...，d|D|}  ，一组方面的关键词={T1 , T2  , ...，Tk}，词汇表V，选定界限值 **p** 以及 迭代步骤上限 **I** .
* **输出：**评论集按照方面分割的句子组。
* **步骤 0：**把所有评论分割为句子，X={x1 , x2  , ...，xM}；
* **步骤 1：**在每个句子 X 中匹配出方面关键词，并为每个记录匹配命中 i 计数 Count( i )；
* **步骤 2：**为句子与对应方面建立标记 αi =argmaxi Count( i ) 。如果得出有多个 i 值，则将对应句子关联到多个方面。【**注：[argmax](http://en.wikipedia.org/wiki/Arg_max)**条件参数，αi 为使得Count( i )最大时的 i 值】
* **步骤 3：**对词汇表集合 V 中的每个词语计算 χ2 ；
* **步骤 4：**对与每个方面的词语按照计算出的 χ2 进行排序，并将前 p 个单词 加入到对应方面关键词集合 Ti 中；
* **步骤 5：**如果方面关键词不再变化，或迭代步数超过 I ，则转到 步骤 6 ，否则回到 步骤 1；
* **步骤 6：**输出带有方面标识的句子集合。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-

具体来说，基于方面的文本分割算法 基本工作流程如下：给定每个方面的一组种子关键词 以及 所有评论文本作为输入，我们假定每个句子与对应方面共享一个最大量的形式重叠；基于这个初始方面标注，我们通过卡方统计（_Chi-Square_ (χ2)）计算每个方面与词语的独立性，并将具有高独立性的词语加入到对应方面关键词列表中。这些步骤被重复执行知道方面关键词列表集收敛不在变化或者超出最大迭代步数。

**词语 w 与 方面 Ai 的独立性计算量 χ2 定义如下**:

![LARA](https://ws3.sinaimg.cn/large/006tNbRwly1fynzk9ro9yj30dg01e0sl.jpg)

其中:

* C1 为归属方面 Ai 的句子集中出现 w 次数总和
* C2 为 **不** 归属方面 Ai 的句子集中 w 出现次数总和
* C3 为归属方面 Ai 而没有包含 w 的句子集的 句子数目
* C4 为 既 **不** 归属方面  Ai **又**没有包含 w 的句子集的 句子数目
* C 为 w 在所有句子集中出现的总数 （C?）

【思想类似于标准文本统计tf/idf】

在基于方面的文本分割后，我们可以得到对于每个评论 d 的 k 元分割子集，表示为 k × n 特性矩阵 Wd ，其中**Wdij** 是关键词 wj 在文本标记为方面Ai 上的频度（以对应方面的词语总数作为分母来标准化这个频度）。

## 4.2 潜在观点评级回归模型（LRR）

第二个环节是基于为文本按方面的分割后的延续，我们采用一种新颖的潜在观点评级回归（LRR）模型来同时分析方面评级 sd 与 方面评级权重 αd 。

### 4.2.1 泛化假设（The Generation Assumption）

我们假设评价者的评级行为如下：

* 为了形成一篇观点性的评论，评价者首先需要决定哪些方面她需要做出评论；
* 然后对于每个选定方面，评价者仔细的选择词语来表达她的观点。
* 评价者是基于她为所评论到的每一个方面所采用的情感词语进行评级。
* 最后评价者为实体进行总体评级，基于她所评论的方面按权重的加权聚合结果，这些权重反应了她在各自方面的强调重视程度。

### 4.2.2 LRR 模型

LRR 模型是一个回归模型包含如下通用步骤：

对于基于方面分割后的评论文本，对于每个评论 d ，我们有一个频度矩阵 Wd 一个标准化的方面词频矩阵。 LRR 模型将 Wd 作为独立变量集（比如：评论 d 的特性），把对应总体评级 r 作为响应变量（比如：待预测变量）

为了进行不同方面的评级与权重建模，LRR模型进一步假定总体评级不是直接由词频特性决定，而是基于一组在潜在方面上的评级所决定，而这些潜在方面上的评级又直接的由词频特性所决定。

我们定义过，sd 和 αd 是一个 k 维方面对应的 权重向量  和方面评级向量。评价者对于文评价文本 d 会假定 先为每个方面 Ai 做出评级，基于线性组合 Wdi 和 βi ,比如下：公式(1)

![lara](https://ws3.sinaimg.cn/large/006tNbRwly1fynzlehycsj304v01sdfm.jpg)

其中 βi ∈ R 暗示词语在对应方面Ai 的情感极性。

随后，这个评论者可以通过 sd 和 αd 的权重和来形成对实体的总体评价。

也就是：![lara](https://ws3.sinaimg.cn/large/006tNbRwly1fynzlswpftj305900x3ya.jpg)

总体评级假定是一个采样服从高斯分布（_Gaussian distribution_），以αdTsd 和变量δ2 ,可以从这推断总体评级预测的不确定性。将这些汇合，我们得到如下公式(2)

![lara](https://ws1.sinaimg.cn/large/006tNbRwly1fynzm52sfkj307j01sglf.jpg)

直观的核心思想是通过潜在观点权重 αd  与情感词权重 β ，为可观察到的总体评级与具体的文本描述建立关联，使得我们可以建立基于具体方面评级与总体评级关联模型。

进一步的观察评级行为，我们发现评价者在不同方面强调程度是一个复杂的行为：不同的评价者可能对不同方面就有不同的偏好程度，例如商业旅行者可能着重考虑互联网接入服务，相对正在度蜜月的夫妇可来说可能更加注重房屋环境。

方面之间不是独立的，尤其当方面之间有重叠，例如一个强调清洁（cleanliness aspect）的也可能表示偏向于间（room apect）

为了考虑评价者偏好的差异性，我们考虑每个评价文本 d 的方面权重 αd 作为从整个语料库的先验分布中产生一组随机变量。

接下来，为了计算不同方面的独立性，我们采用多变量的高斯分布（_Multivariate  
Gaussian Distribution_）作为先验方面权重, 也就是如下公式(3)

![lara](https://ws1.sinaimg.cn/large/006tNbRwly1fynzmknfu7j3043014we9.jpg)

其中 μ 与 ∑ 是平均值与方差参数。

结合公式(2)(3)，我们的得到贝叶斯回归问题，在给定评论中应用LRR模型后，总体评级的可能性为：

![lara](https://ws2.sinaimg.cn/large/006tNbRwly1fynzmpdh4qj30dg02ka9z.jpg)

其中 rd 与 Wd 为评论文本 d 中可观察到的数据，θ =(μ，∑，σ2，β) 是一组基于语料库级别的模型参数。αd 是评论文本 d 潜在方面权重。注意到我们假定 σ2 与 β 不依赖于单个的评论者，同时它们也是语料库级别的模型参数。LRR 的一种图形化模型见下图(3)

![lara](https://ws2.sinaimg.cn/large/006tNbRwly1fynzn7wm49j30b805qmx4.jpg)

图3：LRR图形化表示，外盒代表评论集，内盒代表单个评论文本的潜在观点评级和词语描述的组成。

假定我们已经有给定 LRR 模型参数 θ =(μ，∑，σ2，β) ，可以应用这个模型计算出每个评论文本的潜在观点评级与权重，步骤如下：

(1) 在给定评论文本 d 中的潜在观点评级sd 可以按照 公式(1) 进行计算；

(2) 我们采用最大后验证概率 (MAP：_Maximum a posteriori_）来计算评论文本中最可能的值 αd 。

评论文本d 的目标估计MAP函数定义为：公式(5)

![lara](https://ws4.sinaimg.cn/large/006tNbRwly1fynznjpnmej30bw01s745.jpg)

我们希望扩展这个公式，并为每个评论文本（记作_L_( ad )）关联这个所有方面的特征词 (terms) 到 αd 中,如下公式（6）

![lara](https://ws2.sinaimg.cn/large/006tNbRwly1fynznsxhn3j30dg028wed.jpg)

其中做出代换：∑i=1k αdi = 1   ;  0<=αdi <=1 ， i=1,2,....,k

为了以下约束非线性优优化问题，我们应用conjugate-gradient-interior-point 方法，采用如下公式，对 αd 求倒数。

![lara](https://ws3.sinaimg.cn/large/006tNbRwly1fynzo3235zj30cl01q3yd.jpg)

#### 4.2.3 讨论

LRR 既不是一个纯粹的监督模型也不是一个纯粹的无监督模型，但是它包含了一些已经已知的监督与无监督模型。

一方面，对于它的目标函数，LRR与监督回归模型类似，都是用来适应可观察到的总体评级（见 公式（2））。尽管如此，不同于一个通常的监督模型，LRR 没有为一个评论的总体评级建立学习预测模型；反而，在LRR 中，我们更加感兴趣于从可观察到的总体评级中推断分析出潜在方面评级与权重（虽然 LRR 可以用来预测总体评级）。从另外一方面来说，通过公式（1），包含另外一个回归模型(一方面评级作为响应变量)，而方面评级又不是可以直接观测到的，这样，唯一的监督在于我们有总体评级这个已有指标，并且我们假定他是这些方面评级的权重总和。这是我们 LRR 模型与传统监督回归模型的最大区别。

另外一方面，LRR模型也具有类似于无监督方法的特点，在于我们不需要训练集包含有方面评级数据，并且我们可以推断出这些潜在方面评级。具体来说，要分析一组评论集的潜在方面评级，我们首先需要为数据集找到最优模型参数集 θ =(μ，∑，σ2，β) ，然后运用这些参数预测潜在观点评级 αd , 另外，当加入新数据，我们需要对应更新这些参数集。然而，LRR 也不是一个传统的无监督方法，因为我们有从总体评级中的间接监督。

将我们的LRR模型与标准的话题模型做比较同样十分有趣，例如LDA[2]，在LDA模型中，我们对于那些能够刻画话题的潜在词语分布感兴趣，在 LRR 中，我们尝试发现能够的能刻画语言模型的关联方面评级的词语权重。这两个模型的一个有意义的区别在于 LDA 是完全无监督的，而 LRR 是半监督：虽然我们没有对每个方面的评级做出直接监督，而总体评级对于方面评级做出限制使得我们可以间接监督。

### 4.3 LRR 模型估计

在之前的章节里，我们讨论了如何通过 θ =(μ，∑，σ2，β) 应用LRR模型来为每个评论推断计算方面权重 αd 。在本章节中，我们讨论怎样去运用最大似然（ML :_Maximum  
Likelihood_）估计量估计这些模型参数，也就是，如何找到最优的 θ' =(μ，∑，σ2，β)来最大化所有观测总体评级的可能性。

在整个评论集中的对数似然（log-likelihood）函数是如下公式(7)

![lara](https://ws1.sinaimg.cn/large/006tNbRwly1fynzohwcp2j30ax01s745.jpg)

这样，ML估计参量：

![lara](https://ws3.sinaimg.cn/large/006tNbRwly1fynzop0ebyj30d101s3ye.jpg)

为了计算ML估计参量，我们首先随机初始化所有参数值来获取一个初始θ(0) ,然后用如下EM-style算法来迭代的更新于改进这些参量，通过在每轮迭代中二选一的执行 E-Step 和M-Step：

* **E-Step**：对于语料库中每个评论文本 d ，基于现阶段的参量θ(t)（下标t指迭代轮次）通过运用公式（1）（6）计算方面评级 sd 和方面权重αd 。
* **M-Step**：运用基于现阶段的参量 θ(t) 推断的方面评级 sd 和方面权重 αd 来更新模型参量，通过最大化“complete likelihood”来获取 θ(t+1) ，也就是为所有评论文本推断出所有如下变量，包括 总体评级 rd 、推测出的方面评级 sd 与方面权重 αd 。

首先，我们看方面权重 αd 的高斯先验分布（Gaussian prior distribution）参数更新。目的是在 M-Step 中最大化计算出的所有计算值 αd 的可能性 ：对于所有的评论，我们又如下基于高斯分布的ML估计更新公式。如下公式（8）

![lara](https://ws4.sinaimg.cn/large/006tNbRwly1fynzp01riqj30cr036mx1.jpg)

∑ (t+1) 记为:

![lara](https://ws4.sinaimg.cn/large/006tNbRwly1fynzp8hl34j30cr01na9w.jpg)

也就有公式（9）

![lara](https://ws4.sinaimg.cn/large/006tNbRwly1fynzpjgfpvj30cr01na9w.jpg)

然后，我们在看怎么来更新 σ2 和 β 。又这步已经假定 αd 是可知的，我们可以更新σ2 和 β 来 最大化 P( rd | ad ，σ2，β，Wd )（参见公式（2））。

我们通过如下更新公式来解决这个求最优问题：公式（10）（11）

![lara](https://ws2.sinaimg.cn/large/006tNbRwly1fynzpvb0avj30do03cwee.jpg)

![lara](https://ws2.sinaimg.cn/large/006tNbRwly1fynzq3fbgij30do01xt8l.jpg)

这个对于 β 的逼近解决方法对 一个 |V| ×|V| 据矩阵进行转置，要直接计算的话代价开销很大。为了避免这个问题，我们采用基于梯度算法（gradient-based method）来寻找 β 的最优解，梯度偏导算子如下：

![lara](https://ws4.sinaimg.cn/large/006tNbRwly1fynzqf8yw3j30ad01swec.jpg)

这样，E-Step 与M-Step交替进行，直到方程（7）的似然值收敛。

# 五、实验结果

在本章节中，我们首先描述我们在评估 LRR 模型中所使用的评论数据集，然后讨论实验结果。

## 5.1 数据集与预处理

我们从[TripAdvisor](http://www.tripadvisor.com)站点爬取到一个月内的（从2009年2月14到2009年3月15号）235793条酒店评论数据。选择这套数据集目的在于，它们不仅包含有总体评级，而且还包含有评论者各自的针对 7 个方面的细节评级，它们是value，room，location，cleanliness, checkin/front desk, service, business service，这些方面评级范围从 1 星到 5 星，我们可以利用这些作为 LARA的可靠的定量评估。这些数据集从以下网址获得：[http://times.cs.uiuc.edu/~wang296/Data.](http://times.cs.uiuc.edu/~wang296/Data.)

首先，我们对这些评论数据进行简单的预处理：

（1）统一大写转换为小写；

（2）移除标点符号，停用词（在文献[1]中所提供）；

（3）运用Porter Stemmer[22]还原每个单词词形【by vanjor：英文单词的词性、词形变化，去掉前缀、后缀等】

既然我们已经有 7 个预定义方面的评级数据，我们也把这 7 个方面应用到我们的预测实验中。因此，我们人工为每个预定义的方面选定一组种子关键词，并把他们作为 4.1 章节中所描述的算法的输入，我们设定选择门槛值 p = 5 、迭代步数上限 I = 10 。我们所用到的这个初始化方面词集，如下表，表（1）

![lara](https://ws4.sinaimg.cn/large/006tNbRwly1fynzrdodfkj30dg0520t1.jpg)

在基于方面分割评论集后，我们丢弃掉那些不与任何方面关联的句子。如果我们需要每条评论中都包含这 7 个方面的描述信息，那么数据集中只有 关于184个酒店 的 780个评论符合。为了避免评论中的数据过于贫乏，以及方面描述信息丢失，我们将关于每个对应的酒店的所有评论汇集起来合并成一个新的“评论”（称之为“h-review”），并按 总体评级/方面评级 的平均值作为可信评级。经过这些处理后，我们得到一个1850酒店的语料库，以及108891条评论，具体见下表：表（2）

![lara](https://ws1.sinaimg.cn/large/006tNbRwly1fynzrjaqdoj30ao03i3yk.jpg)

## 5.2 定性评估 (Qualitative evaluation)

首先，我们展示 LRR 模型关于定性评估生成的 3 个样列数据。

## 方面层次酒店分析(Aspect-level Hotel Analysis)

通过检查总体评级来判断给定酒店的质量是一个简单的方法。而这样粗糙的分析可能会丢失不同方面的质量的细节评估：没能指出具有相同方面评级的酒店的差异。为了检验 LRR 模型的这种甄别能力，我们随机选择 就有相同总体评级不同方面评级的的 3 个酒店，并运用 LRR 模型来预测他们的潜在方面评级。预测结果见表 3 ，其中预测值在括弧内(由于空间有限，我们只展示前四个方面数据结果)，见下表（3）

![LARA](https://ws4.sinaimg.cn/large/006tNbRwly1fynzsstueaj30dg04e3yo.jpg)

可以从中发现，3 个酒店具有相同的总体评级，不同的方面细节区别：Grand Mirage 与 Resort and Gold Coast Hotel 都有更高的 price 评级，而 Eurostars Grand Marina Hotel 拥有更高的 location 与 room 评级。这个信息对于那些有着不同方面需求的的人具有很好价值。

### 评论者层次酒店分析(Reviewer-level Hotel Analysis)

即使对于同一个酒店，不同的评论者对于同一个方面会有不同的观点意见。 LRR模型可以更进一步的通过独立评论者层次的预测方面评级来支持这种细节的分析。为了证实这一点，我们选取总数据集中的一个子集 - 那些同时具有7个方面描述评论（覆盖184个酒店的780个评论），在表4 中，两个评论者同时对 Hotel Riu Palace Punta Cana 给出 4 星的总体评级，但是他们对于具体的方面评级不同：评论者1 对于酒店的 cleanliness  评级高于其他方面，而 评论者2 认为其 value 与  location 应该是最好的两个方面。为了证实这种不同，提供如下证据（方面评级）会让用户更好的基于拥有评论进行决策。表（4）

![lara](https://ws1.sinaimg.cn/large/006tNbRwly1fynzt1t5ahj30dg03k3yn.jpg)

### 语料特定词语情感倾向(Corpus Specifc Word Sentimental Orientation)

为了对真个评论文本进行预测潜在观点评级，LRR 同样可以风分析词语的情感倾向。与传统的无监督情感分类算法不同，它们依赖预先定义的词典，LRR 可以直接从给定数据中挖掘这些情感化的信息。在 表5 中展示一些 LRR 有趣的结果，我们按每个方面 展示前5个具有积极权重的单词与前5个具有消极权重的单词。将它们与观点标注词典SentiWordNet [8]进行比较（由于空间有限，我们只展示前四个方面数据结果）。如下表，表（5）

![lara](https://ws3.sinaimg.cn/large/006tNbRwly1fynztzp2h4j30dg05saah.jpg)

我们可以发现一些有趣的结果，单词“ok”在 SentiWordNet 定义为积极的，但在我们的语料库中，评论者使用这个单词表示 仅仅可以接受的；单词 “linen",“walk"以及"beach" 在SentiWordNet中并没有标注具有观点，而他们也是名词，而 LRR 系统赋予它们积极的情感倾向，可能因为"lean"可能暗示"cleanliness"的状况是好的，"walk"与"beach"可能暗示酒店的位置是很便捷的。

这样，LRR可以为我们提供指定领域的词语倾向信息，这对于指定领域中的已有的情感字典的丰富扩大是很有帮助的。

## 5.3 定量分析（Quantitative Evaluation）

### 基本算法

在我们所调研范围内，尚没有人做过解决类似问题的尝试，最接近我们的工作[17]，其中，作者提出两种方法，那就是Local prediction与Global prediction。因此，我们也把这两个方法作为我们基本算法予以比较。也采用其他的一些算法，比如，把评论的总体评级作为方面评级来训练监督模型。我们通过支持向量回归（SVR：_Support Vector Regression_ ）模型[3]，并命名为 SVR-O 。另外，作为一个上限，我们同样也测试了一个完全监督算法 SVR-A，那就是喂有已知方面评级的训练数据的SVR模型做比较，以此找出什么样的 LRR 模型能够不需要监督而达到这一效果。我们使用libsvm包[4]中带有默认参数时间的RBF核心算法，进行SVR-O与SVR-A处理。所有的模型包括 LRR 以及这两个方法[17]都基于同一组数据集。我们采用四折交叉验证（_[4-fold cross validation](/2010/10/cross-validation/)_），并给出处理性能的平均值。

### 测量方法

我们使用四个不同的测度来定量评估这些不同的方法，包括

1. 方面评级预测的均方误差（mean square error） **△aspect 2** ；
2. 评论中的方面相关系数（ρaspect）;
3. 评论间的方面相关系数（ρreview）;
4. 平均准确率（MAP：_Mean Average Precision_）[11]，一个经常用来衡量信息检索中平局准确性的指标。

正式的，假定s di* 是真实的方面评级Ai 。**△aspect 2** 直接测量预测方面评级sdi 与真实方面评级s di* 的差异，定义如下：

![LARA](https://ws3.sinaimg.cn/large/006tNbRwly1fynzv53yutj30ak01xmwz.jpg))

ρaspect 目的在于测量方面评级预测的性能，用于保存对应评论的按照真实评级的相关方面排序。例如，在一个评论中，评论者可能偏好 location大于cleanliness，ρaspect 会评估这个预测的评级与对应真实的偏好排序是否一致，ρaspect 定义如下：

![LARA](https://ws2.sinaimg.cn/large/006tNbRwly1fynzvbop3wj306701yjr6.jpg)

其中 ρsd,sd 是两个向量 sd 与 sd 的皮尔森相关系数（Pearson correlation）。

类似的 ρreview 定义了如下皮尔森相关系数：

![LARA](https://ws3.sinaimg.cn/large/006tNbRwly1fynzvm2h7vj306j01wmwy.jpg)

其中 si 与 si* 两向量为整个评论集中的关于方面 Ai 的预测值与真实值。它可以指出在整个评论集中，有关方面Ai 的方面预测值与真实值是否是一致的，这种排序可以回答如下问题："那个酒店拥有最好的service？"

尽管如此，ρreview 对于所有的条目平等权重，并且不反应排名前几个的特性，直观来说从用户的视角来看更为重要。因此，我们也采用MAP来评估模型关于评论的的排序准确信。更准确的说，我们对真实评论站前十的评论子集作为一个相关评论集，以此来看如果我们通过预测方面评级，是否也能把这10个评论预测前十。我们对所有酒店按照7个方面排序，并计算 MAP 前10个评论作为临界值。

结果分析：

![LARA](https://ws3.sinaimg.cn/large/006tNbRwly1fynzwom1gqj30cw042weo.jpg)

如上表，表（6）展示了所有5个算法在四个度量上的测量结果。因为SVR-A是完全监督算法，而其它均不是，我们把它单独列在最下一行。同时出SVR-A外的四个模型，粗体着重标注在四个度量上各自最高性能值。

总体观察来看在 LRR 的性能在**△aspect 2** 与 ρreview 这两个度量比其他的非 SVR-A 模型性能要好的多，但是它也不是在这两个方面表现性能最好的。较高的 ρreview 表明LRR可以更好的区分一个评论中的不同方面的评价。注意到这个在不同方面上的相对偏好信息不能仅从总体评级中获取。另外，高的 MAP@10 表明 LRR 相比其他方法也能更好的检测到 在各个方面排名前十的酒店，这些对于用户的直接排名也十分有用，因为高排名的结果将可能会最影响用户满意度。

注意到 **△aspect 2** 是独立的衡量每个预测方面评级与真实方面评级，也就无法反应方面关联排序性能如何。比如，有一个只有三个方面的实体，一个评论的整体评级为 4 而事实的方面评级为（3,4,5）。那么一个生涩的预测（4,4,4）没能辨别出方面的不同，将会有**△aspect 2** =0.67，而另外一个预测（2,3,4），能够辨别出方面上的不同，却有相对更高（效果更差）**△aspect 2** =1。却是，可以观察到Local prediction模型达到最好的**△aspect 2** =0.588，但是是在以最低的 ρaspect 指标性能为代价，而事实上 ρaspect 具有更重要的用途。

通过对基于方面评级预测的准确度排名深入研究，我们可以看到两个测度 **△aspect 2** 与 ρreview 产生不同结论。这个是在预期的，因为ρreview 测度这 1850 个 h-reviews 的相关性，而[MAP@10](mailto:MAP@10)只关心前十的，Local prediction 没有在 ρreview 指标上得分最高，却在[MAP@10](mailto:MAP@10)指标上表现糟糕。这说明了它比LRR的更出色与低排名而不是高排名的那些，而这些又是用户最为关心的。

注意到给 SVR 喂养总体评级并没有达到预期的性能，这也在一定程度上证实了我们的假设：总体评级与方面评级是有区别的。因此，光只看总体评级是不够充分的。最终，并不奇怪的是，LRR 没有基于事实方面评级数据训练模型 SVR-A 那样高的表现性能。尽管如此 LRR 不需要训练集包含有任何方面评级的标记，比 SVR-A 具有更显示的应用前景。

### 计算复杂度

在实际的应用中高效的挖掘算法是十分重要的。LRR 的主要计算工作在于解决非线性优化器问题，见公式（6）（11）。LRR的训练步骤中的收敛度取决于模型参量 Θ ,评论集 |D| 的数量 以及 迭代步骤上限 I，这个算法复杂度初步估计为O(k(n + k + 1)|D| I )，与评论集数目线性相关，对于我们的数据集，这个算法在奔腾4系列2.8CPU/2GB 内存的台式电脑上用时不到3分钟完成。

## 5.4 应用

运用 LRR 模型进行 具体的观点理解与获取对于许多应用都具有潜在的价值，我们列举如下三个样列应用.

### 基于方面的概括

因为LRR可以为评论 sd 推断方面评级，我们就可以更为容易的对于酒店的各个方面上 聚合评论集的方面评级（例如：（1/|D| )∑d∈D sd）。这样酒店的方面评级可以被看做为基于方面观点的概括。基于这些，我们也可以对指定酒店通过公式（1）计算选择那些句子，这些在每个方面具有最高的和最低的评分，帮助用户更加理解每个方面的观点。

我们在 表7 中展示了一个基于方面的概括。我们可以看到评价者当考虑到酒店在西雅图这么好的位置时对于价格的容忍值就很高。尽管如此，还是有很多地方可以改进，不好的供暖系统，以及互联网接入需收费。这些细节信息将会对用户从海量评价信息中挖掘基本观点十分有用。如下表（7）

![LARA](https://ws1.sinaimg.cn/large/006tNbRwly1fynzza221uj30dg04i74p.jpg)

### 用户评价行为表现分析

通过为每个独立的评论推断潜在方面权重ad，我们可以知道对应的评论者在不同方面的强调程度，可以视作为用户评级行为表现的理解。一个潜在的应用是挖掘用户在作出最终评价时，那些因素对于用户的判断具有最大影响。为了深入研究，我们选择了两组不同价格区间的酒店数据：一组的价格超过$800（称之为昂贵的酒店），另一组为价格低于$100（称之为廉价的酒店），对于每一组，我们选取平均整体评级排名前十与末十的酒店，最终形成四个酒店子集。我们展示这四个酒店子集的平均方面权重ad，结果见表（8）

![LARA](https://ws3.sinaimg.cn/large/006tNbRwly1fynzzgsjwwj30cw05g74k.jpg)

我们发现一个有趣的现象是，评价者们为昂贵的酒店给出高评价主要是因为他们良好的service与locations，而对应给出低评价这是因为糟糕的房屋环境与过高价格。

作为对比，评价者为廉价的酒店给出高评价主要是因为好的price/value 以及良好的location，而相对给出低评是因为糟糕的cleanliness.

另外，这些量化的评级可能包含不同评价间的平均反应：低收入客户为便宜的酒店给出“value”方面的 5 星评价，而一些其他追求更好服务的客户可能给予昂贵的酒店在“value”方面的 5 星评价。仅仅为每个方面预测评级仍然不够以挖掘到用户间的微妙差异，但是这种方面权重的推断更好的便于我们理解为什么低收入客户相比“service”更倾向于“value”。为了深入了解这些，我们从四个城市：Amsterdam、Barcelona、Florence 以及 San Francisco中（这些地方的酒店在我们的语料库中大有所在），挑选出在“value”方面评级都具有5星评价的酒店，我们按照他们的方面权重比率 value/location、value/room、value/service 进行排序，相对应的，对于每个比率，计算出平均前十和末十的酒店。数据展现在表（9）

![LARA](https://ws4.sinaimg.cn/large/006tNbRwly1fynzzpcmp9j30dg05s0t0.jpg)

我们发现那些相对具有“value”上更高权重的酒店具有更低的价格，同时对“location”、“room”与“service”这些方面具有较高权重的酒店倾向于就有更高的价格，表明了即时这些酒店在“value”方面具有同样方面评价，偏好于“value”方面的客户可能更倾向于价格低廉的酒店，而那些对偏好于“location”或“service”（除了"price"）方面的则可能会接受更高的价格。这样推断出的方面权重αd 对于挖掘用户评级行为十分有用。

### 个性化排序

对酒店按照各个方面推断出的评级对于用户十分有用。我们展示为每个独立的评论学习不同方面的评级权重，使得我们可以为一个当前用户挑选具有相同评价行为偏好的评论进行个性化排序。具体来说，给定一个用户的偏好权重作为查询条件，我们可以通过选择那些具有类似偏好权重的评论者，并且只基于这些评论者们的评论集做出酒店排名。

为了有效的展示 LRR 模型支持这种个性化牌型，考虑一个样例查询：Query= { value weight：0.9, others：0.016 }，暗示用户最看重偏好“value”，并不太关心其他方面。我们运用两用不同排名方法：

* **方法 1**: 不考虑输入查询，通过按预测方面评级对酒店进行排序。
* **方法 2**: 挑选出与所给定查询具有最接近的方面权重（也就是 αd）的评论者前10%，并仅只根据他们的评论做出酒店排名。

通过两种方法，我们基于查询中定义的方面权重对酒店进行排序，得到的前 5 的结果如下表：表（10）

![lara](https://ws1.sinaimg.cn/large/006tNbRwly1fyo02siw85j30dg06jaai.jpg)

十分有趣的现象是尽管方法 1 中前5的结果都具有5星评价（并且大致也可推测道他们在“value”方面上也具有很高评级，因为排名主要是基于查询中的权重的），它们的价格倾向比方法2 中得到的前五酒店要高；的确，方法 1 中的前 5 酒店平均价格是$412.6 而方法2 中前5 酒店平均价格仅为$289.4，相对要低很多。（整个数据集中所有酒店的平均价格为$334.3）。直觉可以看到对于样列查询，方法 2 对于用户更有帮助。这也就意味着像方法2 这样，只从挑选类似偏好权重行为的用户的评论集进行排序，个性化排序对“value”方面偏好权重更大，确保排名前几的酒店真的具有相对较低的价格。

# 结论

在本文中，我们定义了一种新颖文本挖掘算法问题-潜在方面观点评级分析（LARA：Latent Aspect Rating Analysis）来分析在线评论在话题方面级别的观点。LARA 通过分析一套带有整体评级的评论文本 以及 一组指定的方面作为输入，来挖掘每个独立的评论者在给定的这些方面的潜在评级，以及不同方面的相对权重大小。为了解决这个问题，我们提出一个新颖的潜在评级回归模型（LRR）。

我们对于一组酒店评论数据集的以经验为主的实验表明 LRR 模型能有效的解决 LARA 问题，挖掘那些方面评级的差异背后的有趣行为，即使总体评级相同。这个结果页表明基于话题方面的层级上的观点分析可以支持多种应用，包括方面观点概括，基于方面评级的实体排序 以及 用户评级行为分析。

我们的工作开启了一个文本挖掘的新颖方向，聚焦在分析带有观点性文本的潜在评级分析。再为来它将会有更多有趣的研究方向值得挖掘。列于，虽然我们是基于评论定义了LARA问题，LARA 也很明显可以应用于任何带有观点性的文本（比如网络日志），并带有总体评级以达到具体的理解文本观点。她可能在其他应用场景中就有有趣研究价值。另外,我们的LRR模型并没有严格的显示为词语特性，其他的类型特性也可以很容易的应用到此模型中。同样，在我们的LARA定义中，我们假定方面描述为一组关键词形式。这有利于用户灵活的把握控制他所想要研究的方面。

LARA问题 在未来关于潜在方面挖掘与方面评级及方面权重将是十分有趣的研究话题方向。

# 鸣谢

感谢匿名评论者的十分有价值的评论。本文基于IBM Faculty Award -an Alfred P. Sloan Research Fellow-ship, and by the National Science Foundation under grants IIS-0347933, IIS-0713581, IIS-0713571, and CNS-0834709所支持下的工作。

# 参考文献

* [1] Onix text retrieval toolkit stopword list. [http://www.lextek.com/manuals/onix/stopwords1.html](http://www.lextek.com/manuals/onix/stopwords1.html).
* [2] D. Blei, A. Ng, and M. Jordan. Latent dirichlet allocation. The Journal of Machine Learning Research, 3:993 - 1022, 2003.
* [3] C. Burges. A tutorial on support vector machines for pattern recognition. Data mining and knowledge discovery, 2(2):121 - 167, 1998.
* [4] C.C. Chang and C.J. Lin. LIBSVM: a library for support vector machines, 2001. Software available at [http://www.csie.ntu.edu.tw/~cjlin/libsvm](http://www.csie.ntu.edu.tw/~cjlin/libsvm).
* [5] H. Cui, V. Mittal, and M. Datar. Comparative experiments on sentiment classifcation for online product reviews. In Twenty-First National Conference on Artificial Intelligence, volume 21, page 1265, 2006.
* [6] K. Dave, S. Lawrence, and D. M. Pennock. Mining the peanut gallery: opinion xtraction and semantic classifcation of product reviews. In WWW '03, pages 519{528, 2003.
* [7] A. Devitt and K. Ahmad. Sentiment polarity identifcation in fanancial news: A cohesion-based approach. In Proceedings of ACL'07, pages 984 - 991, 2007.
* [8] A. Esuli and F. Sebastiani. SentiWordNet: A publicly available lexical resource for opinion mining. In Proceedings of LREC, volume 6, 2006.
* [9] A. Goldberg and X. Zhu. Seeing stars when there aren , a¶rt many stars: Graph-based semi-supervised learning for sentiment categorization. In HLT-NAACL 2006 Workshop  
    on Textgraphs: Graph-based Algorithms for Natural Language Processing, 2006.
* [10] M. Hu and B. Liu. Mining and summarizing customer reviews. In W. Kim, R. Kohavi, J. Gehrke, and W. DuMouchel, editors, KDD, pages 168 - 177. ACM, 2004.
* [11] K. Jarvelin and J. Kekalainen. IR evaluation methods for retrieving highly relevant documents. In Proceedings of SIGIR'00, pages 41 - 48. ACM, 2000.
* [12] N. Jindal and B. Liu. Identifying comparative sentences in text documents. In Proceedings of SIGIR '06, pages 244 - 251, New York, NY, USA, 2006. ACM.
* [13] H. Kim and C. Zhai. Generating Comparative Summaries of Contradictory Opinions in Text. In Proceedings of CIKM'09, pages 385 - 394, 2009.
* [14] S. Kim and E. Hovy. Determining the sentiment of opinions. In Proceedings of COLING, volume 4, pages 1367 - 1373, 2004.
* [15] K. Lerman, S. Blair-Goldensohn, and R. T. McDonald. Sentiment summarization: Evaluating and learning user preferences. In EACL, pages 514 - 522, 2009.
* [16] B. Liu, M. Hu, and J. Cheng. Opinion observer: Analyzing and comparing opinions on the web. In WWW '05, pages 342 - 351, 2005.
* [17] Y. Lu, C. Zhai, and N. Sundaresan. Rated aspect summarization of short comments. In Proceedings of WWW'09, pages 131 - 140, 2009.
* [18] S. Morinaga, K. Yamanishi, K. Tateishi, and T. Fukushima. Mining product reputations on the web. In KDD '02, pages 341{349, 2002.
* [19] B. Pang and L. Lee. Seeing stars: Exploiting class relationships for sentiment categorization with respect to rating scales. In Proceedings of the ACL, pages 115 - 124, 2005.
* [20] B. Pang, L. Lee, and S. Vaithyanathan. Thumbs up? Sentiment classi¯cation using machine learning techniques. In EMNLP 2002, pages 79 - 86, 2002.
* [21] A.-M. Popescu and O. Etzioni. Extracting product features and opinions from reviews. In Proceedings of HLT '05, pages 339 - 346, Morristown, NJ, USA, 2005. Association for Computational Linguistics.
* [22] M. Porter. An algorithm for su±x stripping. Program, 14(3):130 - 137, 1980.
* [23] B. Snyder and R. Barzilay. Multiple aspect ranking using the good grief algorithm. In Proceedings of NAACL HLT, pages 300 - 307, 2007.
* [24] I. Titov and R. McDonald. A joint model of text and aspect ratings for sentiment summarization. In ACL '08, pages 308 - 316.
* [25] Y. Yang and J. O.Pedersen. A comparative study on feature selection in text categorization. In Proceedings of ICML'97, pages 412 - 420, 1997.
* [26] L. Zhuang, F. Jing, and X. Zhu. Movie review mining and summarization. In Proceedings of CIKM 2006, page 50. ACM, 2006.