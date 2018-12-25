---
title: 基于Hexo在Gitlab上搭建技术博客
date: 2016-05-04 15:15:00
tags:
  - hexo
  - gitlab
categories: 最佳实践
---
![github托管hexo博客](http://i.v2ex.co/5bb7J7NT.png)
# 序
花了一天的时间研究了搭建静态博客系统的方案，最终选型如下
* 博客语言：[Markdown](http://wowubuntu.com/markdown/)
* 博客程序：[Hexo](https://hexo.io/) 含有部署功能
* 博客主题：[NexT](http://theme-next.iissnan.com/)
* 静态博客托管：[Gitlab](https://gitlab.com/)+[Coding.net](https://coding.net/) 双线托管
* 博客源码及编译项目: [Gitlab](https://gitlab.com/)
* 域名DNS: [DNSPod](https://www.dnspod.cn/) 按国内外双线解析
* 自动部署：[travis-ci](https://travis-ci.org) 在线写文章，自动编译并发布
* 在线写文章: [stackedit](https://stackedit.io/)  可视化Markdown调试, 自动同步上传到Gitlab项目
<!--more-->

为什么要从自建worldpress迁移过来，原因可参考阮一峰的[文章](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)，相比之下wordpress各种不省心并复杂繁琐，而现在方案每篇文章就是一个markdown文档，满足随心DIY。

> 第一阶段，刚接触Blog，觉得很新鲜，试着选择一个免费空间来写。
第二阶段，发现免费空间限制太多，就自己购买域名和空间，搭建独立博客。
第三阶段，觉得独立博客的管理太麻烦，最好在保留控制权的前提下，让别人来管，自己只负责写文章。

# Hexo 安装
其他只简单尝试过Jeky,  Hexo相比更易用，并利用YMAL配置保留较好的配置能力，详细比较见：[FarBox、Jekyll、Octopress、ghost、marboo、Hexo、Medium、Logdown、prose.io，这些博客程序有什么特点？](https://www.zhihu.com/question/21981094)

Hexo搭建教简单，参考[官方教程] (https://hexo.io/docs/setup.html) 10分钟能出hello world, 支持本地利用nodejs起服务查看，自己遇到一个坑就是_config.yml 中配置 key: 后面必须有一个空格，否者会导致各种问题，还没有报错信息。

大致总结Hexo特点:

* 极易上手，几部命令完成
* 单一配置文件_config.yml 易懂扩展性强
* Hexo支持较好的部署功能，支持多种部署方式与多目标部署
* 模板与插件丰富，安装方便。 官方网站已经有多达百种供选择


# Github&Coding.net双线加速
国内访问gitlub不太稳定，通过Hexo方便的支持双线同时发布，并通过dnspod的智能路由解析功能实现，国内访问到是coding.net托管的，国外访问的是github托管的。

详细教程参考: [hexo同时托管到coding.net与github](https://segmentfault.com/a/1190000004548638)

附我的hexo的[_config.yml](https://github.com/vanjor/vanjor/blob/master/_config.yml)配置片段
```
deploy:
  type: git
  repository:
    github: git@github.com:vanjor/vanjor.github.io.git
    coding: git@git.coding.net:vanjor/vanjor.git
  branch: master
```

DNSPod配置截图:
![enter image description here](https://o6mq6uqzy.qnssl.com/blog/image/dnspod_double_routing.png)

# travis-ci自动部署发布文章

github 支持Webhook可以自动触发自己主机上的发布脚本，但仍需要自己维护主机，利用travis-ci可以消除这一依赖。

完整教程参考: [用 Travis CI 自动部署 hexo](http://blog.acwong.org/2016/03/20/auto-deploy-hexo-with-travis-CI/)
这里有遇到过网络连接问题，deploy代码失败，长期考虑需要加检测与重试功能。

附我的：   
* 源码系统项目：https://github.com/vanjor/vanjor
* travis-ci自动编译部署历史：https://travis-ci.org/vanjor/vanjor/builds
* 编译后的静态博客项目
  * github: https://github.com/vanjor/vanjor.github.io
  * coding.net: https://coding.net/u/vanjor/p/vanjor

# 在线编写&发布文章

https://stackedit.io 支持保存草稿，并选择任意文章同步到指定的github项目对应目录中。[hexo边搭边记](http://blog.sunnyxx.com/2014/02/27/hexo_startup/) 也赞过该系统。

当然Hexo官方页面上也介绍了两个扩展（[hexo-admin](https://github.com/jaredly/hexo-admin) 及 [hexo-hey](https://github.com/nihgwu/hexo-hey)）实现了内置admin后台，支持在线写文章，鉴于不太稳定，暂不用。


# 其他

参考Hexo主题Hext的 [文档](http://theme-next.iissnan.com/third-party-services.html) 集成支持了评论系统，统计分析，搜索，MathJax；

参考 [Hexo的NexT主题个性化：添加文章阅读量](http://www.jeyzhang.com/hexo-next-add-post-views.html) 增加了文章统计

其他一些细节优化见：[动动手指，NexT主题与Hexo更搭哦（基础篇）](http://www.arao.me/2015/hexo-next-theme-optimize-base/)

图片功能最终还是采用图床方式，最终采用七牛云服务，参考[有哪些适合开发人员的图床](https://www.zhihu.com/question/21349585)

总之这块是必不可少，但实际上弄的越复杂依赖就越多，以后潜在维护量就越大，因此就不做更多的挣扎。

# TODO

对于有HTTPS洁癖的人，当然没有放弃果尝试，可以在自己主机做一个proxy实现https功能，但这就有需要自己维护了，鉴于目前还没有较好的三方方案，先做观察。并且我已经在图床中图片均采用HTTPS方式提前考虑未来升级HTTPS了。

目前的一种完全三方可能方式：[Pages 博客 HTTPS 化尝试与 Universal SSL](https://blog.jamespan.me/2015/04/17/github-and-gitcafe-pages/)


# 附

* Logo图片来源: http://wsgzao.github.io/post/hexo-guide/

