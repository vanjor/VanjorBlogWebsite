---
title: Web2.0之Mashup初探
date: 2010-08-29 20:31:26
updated: 2010-08-29 20:31:26
categories: 知识积累
tags:
  - Webservice
  - Mashup
---

当今信息社会里，信息数据作为一种比较核心价值所在，人们面对如此众多庞杂分散的信息数据时，信息组织与呈现方式并不能让人们满意，许多地方都需要进一步来细化挖掘与完善， 比如，如今纷繁的团购网雨后春笋似的诞生时，那些团购聚合网也就随之出现了，从而满足用户一站式浏览各个团购网的信息，这一方面是为用户提供一个便捷的信息整合检索服务。

<!-- more -->

![基于纷纭的团购网站而生的团购聚合网](https://asset.vanjor.com/images/006tNbRwly1fynnwlff7jj30lb0fgdi5.jpg)

另外的一种场景，城市的交通部门拥有路况监控设备与路况事实信息数据，每天会有各种显示街区道路的路况数据报表，但是由于人眼视觉原因，不可能很好的快速去理解消化这些条条框框里的数据，如果作为Web平台服务提供给大众的话，这些一大堆表格与查询控件所带来无疑是一种比较糟糕的用户体验度。

一种可能的方式是结合Web-GIS相关服务数据平台，将路况信息与Map-GIS系统进行糅合-Mashup，那么这个结合后的信息呈现方式更直观更快捷的为人们所理解和便于做出快速决策。下图为

![搜狗北京城区实时交通流量](https://asset.vanjor.com/images/006tNbRwly1fynnwyh62kj30sg0f4jud.jpg)

# Mashup概念

那么什么是Mashup，参考[programmableweb](http://www.programmableweb.com/)的定义：

> A web Mashup is a web page or application that combines data from two or more external online sources. The external sources are typically other web sites and their data may be obtained by the Mashup developer in various ways including, but not limited to: APIs, XML feeds, and screen-scraping.

Mashup作为一种新型的基于 Web 的数据集成应用程序的统称，它们的流行萌芽于对交互式用户参与和集成第三方数据的类似于科学怪人方式的重视。使用萌芽一词是有一定原因的；Mashup Web 站点的特点就表现为它正在 Web 上扎根发芽，它们利用了从组织边界之外的数据源获取的内容和功能。

Mashup单词的起源：它源于流行音乐，Mashup 是从两首不同的歌曲（通常属于不同的流派）中混合演唱和乐器的音轨而构成的一首新歌。与那些 “bastard pop” 歌曲类似，Mashup 也是内容的一种不常见的创新组合（通常都源自于无关的数据源），这都是人工进行合成的（而不是通过计算机来合成的）。

个人认为Mashup实际上是属于概念范畴，是对各种数据，信息，系统，乃至服务的一种分析，关联，组合与重新展现的方案，而这个数据信息不在仅仅来自服务方，而已是WWW上所有能够获得到的数据与信息，包括众多的第三方服务。

# Mashup相关技术

Mashup与服务整合，内容信息聚合具有类似的范畴，而Mashup不仅仅是对同类信息的整合，比如将不同RSS源信息糅合，可以是Google Map的Google Map APIFrame结合大众点评网对商家的点评信息Atom等跨技术跨领域范畴里的数据信息整合与服务。

Mashup作为一种概念来说，支撑它的技术牵扯到各个领域，作为Web2.0中的方方面面的技术都可以为之所用，不再只是Web服务编程REST/SOAP, XML/JSON,AJAX, RDF/URI organizeFlex, 数据缓存，语义分析等等。

另外对于核心的数据源处理，异构数据之间的抽取，清洗，分析，规整，聚类，重整都是Mashup涉及到的问题与技术，在之后的篇章中，我将会介绍分享自身的一些经验与思路。

# Mashup分类

这里以目前应用范畴与Mashup信息分类来看，有以下一些：

## 地图GIS与多媒体内容信息Mashup

毫无疑问，Google在这个阶段的信息技术中，人们搜集大量有关事物和行为的数据，二者都常常具有位置注释信息。所有这些包含位置数据的不同数据集均可利用地图通过令人惊奇的图形化方式呈现出来。Mashup 蓬勃发展的一种主要动力就是 Google 公开了自己的 [Google Maps API](http://code.google.com/apis/maps/index.html)，打开了一道大门，让 Web 开发人员（包括爱好者、修补程序开发人员和其他一些人）可以在地图中包含所有类型的数据。

Google也充分在地图上整合的商家信息，大众评论，Picasa等用户上传的地理地貌图片信息这里Map已经成为其他相关信息的一种载体，甚至Google也充分发挥自己能动性，拍摄各国城市街景图像数据，通过A+B=C式的Mashup诞生各种各样新式的服务。

![Google map与图片，文字，评论等信息的Mashup](https://asset.vanjor.com/images/006tNbRwly1fynrzwcnphj30sg0h5q4q.jpg)

## 基于社交网络SNS等产生内容元数据抽取关联的Mashup

分析内容提供者所提交的图像文字音像资料，从中抽出大量相关的元数据（例如谁提供的，内容中关联主体，地点，时间，事件），SNS设计者充分利用这些抽取元数据重新组织管关联，显示一个个丰富社交网络图。如大家熟悉的人人网，在日志中提供关联他人的元数据的管理点，相册服务提供人像圈定关联他人。甚至是现在通用的新闻网站在新闻中通过照片匹配而将照片中的内容以文字的形式呈现出来，也是一种Mashup。

这一点其实Google的Picasa相册服务已经做得前沿探索了也很成熟，它可以从用户提供的相册按照所属主人，相册，分享范围，时间，拍照地图地点关联地图，图片自动人像识别关联Google Mail中的联系人等等将大量的用户信息自动关联识别到一起，为用户提供了更加人性智能化的使用体验。

## 商业社会化服务Mashup

搜索和购物 整合在 Mashup 这个术语出现之前就已经存在很长时间了。在 Web API 出现之前，有相当多的购物站点与工具，就诞生了众多的针对购物的垂直搜索引擎，整合了各大购物网站的信息数据，而现在社会商业各个方面，只要能有目标用户的特定需求，像团购，求职，房屋，中介，二手，等等都会存在大量的Mashup，而之前的Mashup大多是不太完善的，是Mashup设计者通过第三方从信息源挖掘抽取数据，常见于运用信息抽取采集系统大量搜寻相关信系重新整理为规整元数据集并展现。

而如今，淘宝已经为其平台不断的开放第三封那个数据访问API接口平台与服务，这也会成为未来的一种发展趋势，百度的框计划，在搜索结果中直接聚合了图片信息，常用天气，计算结果，网站登录入口等服务也是广义上一种Mashup的思路。

![Google - shopping ，meta data mashup](https://asset.vanjor.com/images/006tNbRwly1fyns2eha79j30s30fmmz2.jpg)

## 基于新闻源分享的文本图像信息聚类Mashup

例如纽约时报、BBC 或路透社 已从 2002 年起使用 RSS 和 Atom 之类的联合技术来发布各个主题的新闻提要。以联合技术为基础的 Mashup 可以聚集一名用户的提要，并将其通过 Web 呈现出来，创建个性化的报纸，从而满足读者独特的兴趣。Diggdot.us 正是这样的一个例子，它合并了 Digg.com、Slashdot.org 和 Del.icio.us 上与技术有关的内容，还有抓虾，Google阅读器等等。随着文本语义挖掘，图像音视频识别到等技术的成熟与实现，个性化信息内容服务也将越来越精准而人性化。

# Mashup相关

有的文章以社会挑战，技术挑战等方面来说明，个人主要从以几个角度来探讨：

## 健康性

既然Mashup是基于数据服务的一种三方的整合，任何人都可以发布自己的WebService服务接口等，在构建自己的Mashup服务中，如何确保开发人员需要面对类似源自于在异构数据集之间共享语义的挑战。如何确保这些异构数据源的完整性，正确性，避免过时的信息源，错误信息源，宕机暂不可用等等所带来负面影响。

## 安全性

对于所发布这些大量的服务接口API，如何界定合适访问权限，以防止非法目的的收集，比如前些时针对有黑客针对Facebook系统通过网页采集或许多大几百万个公开资料的人的个人页面资料，以及如何防止数据源被污染，如植入人为干扰，广告信息，等等。对与敏感数据也可能要求一定的机密性（即加密），我们必须要清楚何时将它们与其他资源集成在一起，而不会带来风险。身份对于审计和法规遵从性来说也非常重要。另外，由于数据集成是在服务器和客户端同时发生的，因此从用户到 mashup 服务进行的身份和证书委托也可能会成为一个需要去注意是实现的问题。

## 盈利模式

不能带来一种可持续盈利的模式的技术是难以长久的，在Mashup所赖以生成的三方数据服务接口提供者如何能通过自己所发布的服务获取盈利呢，目前的很多Mashup三方服务接口，平均每几秒就有一个诞生，大部分是基于免费的提供，这些服务质量和稳定上良莠不齐，难以转为收费模式而获取盈利，而Google通过其发布的Google Map Api吸引的不少的开发者，他的质量和稳定性收到大家的青睐，在一些第三方商业需求Google Map服务时，就需要想Google缴纳一定费用索取API key，这也是一个成功的模式，另外会有一些类似百度推广，植入广告等可以作为一种盈利模式。

# 参考

* IBM development works: [http://www.ibm.com/developerworks/cn/xml/x-mashups.html](http://www.ibm.com/developerworks/cn/xml/x-mashups.html)
* 查看最新的Mashup内容：[http://www.programmableweb.com/Mashups](http://www.programmableweb.com/mashups)
* SOAP/REST: [http://www.ibm.com/developerworks/cn/webservices/0907\_rest\_soap/](http://www.ibm.com/developerworks/cn/webservices/0907_rest_soap/)