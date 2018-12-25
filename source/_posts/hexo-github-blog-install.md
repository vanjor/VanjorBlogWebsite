---
title: 基于Hexo在Github上搭建技术博客
date: 2016-05-04T15:15:00.000Z
tags:
  - hexo
  - github
  - markdown
categories: 最佳实践
published: true
---
![github托管hexo博客](http://i.v2ex.co/5bb7J7NT.png)
# 序
花了一天的时间研究了搭建静态博客系统的方案，最终选型如下
* 博客语言：[Markdown](http://wowubuntu.com/markdown/)
* 博客程序：[Hexo](https://hexo.io/) 含有部署功能
* 博客主题：[NexT](http://theme-next.iissnan.com/)
* 静态博客托管：[Github](https://github.com/)+[Coding.net](https://coding.net/) 双线托管,支持HTTPS
* 域名DNS: [Aliyun](https://www.aliyun.cn/) 按国内外双线解析
* 自动部署：[travis-ci](https://travis-ci.org) 在线写文章，自动编译并发布
* 在线写文章: [prose](https://prose.io/)  可视化Markdown在线编辑器，可保存项目
<!--more-->

为什么要从自建worldpress迁移过来，原因可参考阮一峰的[文章](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)，相比之下wordpress各种不省心并复杂繁琐，而现在方案每篇文章就是一个markdown文档，满足随心DIY。

> 第一阶段，刚接触Blog，觉得很新鲜，试着选择一个免费空间来写。
第二阶段，发现免费空间限制太多，就自己购买域名和空间，搭建独立博客。
第三阶段，觉得独立博客的管理太麻烦，最好在保留控制权的前提下，让别人来管，自己只负责写文章。

# Hexo 安装

其他只简单尝试过Jeky,  Hexo相比更易用，并利用YMAL配置保留较好的配置能力，详细比较见：[开源博客横向评测](https://www.zhihu.com/question/21981094)

Hexo搭建教简单，参考[官方教程](https://hexo.io/docs/setup.html) 10分钟能出hello world。

个人大致总结Hexo特点:

* 极易上手，几部命令完成
* 单一配置文件_config.yml 易懂扩展性强
* Hexo支持较好的部署功能，支持多种部署方式与多目标部署
* 模板与插件丰富，安装方便。 官方网站已经有多达百种供选择

# Github&Coding.net双线加速

国内访问gitlub不太稳定，通过Hexo方便的支持双线同时发布，并通过dnspod的智能路由解析功能实现，国内访问到是coding.net托管的，国外访问的是github托管的。

详细教程参考: [hexo同时托管到coding.net与github](https://segmentfault.com/a/1190000004548638)

附我的hexo的[_config.yml](https://github.com/vanjor/VanjorBlogWebsite/blob/source/_config.yml)

# travis-ci自动部署发布文章

github 支持Webhook可以自动触发自己主机上的发布脚本，但仍需要自己维护主机，利用travis-ci可以消除这一依赖。

完整教程参考: [用 Travis CI 自动部署 hexo](http://blog.acwong.org/2016/03/20/auto-deploy-hexo-with-travis-CI/)
注: ssh-keygen -t rsa -C "youremail@example.com" -t id_rsa 通过-t命令指定保存为当前目录的id_rsa文件。

附我的：   
* 源码系统项目：https://github.com/vanjor/VanjorComWebsite
  * branch: source as default branch is the source code
  * branch: master retain the compiled website code serve as Github Page service
* travis-ci自动编译部署历史：https://travis-ci.org/vanjor/VanjorBlogWebsite

# 在线编写&发布文章
* 三方在线应用: [prose](https://prose.io), [stackedit](https://stackedit.io)
* 三方本地应用: [HexoEditor](https://github.com/zhuzhuyule/HexoEditor)
* 官方自行搭建服务: [hexo-admin](https://github.com/jaredly/hexo-admin)


# 其他

其他一些细节优化见：[动动手指，NexT主题与Hexo更搭哦（基础篇）](http://www.arao.me/2015/hexo-next-theme-optimize-base/)

图片功能最终还是采用图床方式，最终采用七牛云服务，参考[有哪些适合开发人员的图床](https://www.zhihu.com/question/21349585)

总之这块是必不可少，但实际上弄的越复杂依赖就越多，以后潜在维护量就越大，因此就不做更多的挣扎。


# 附

* Logo图片来源: http://wsgzao.github.io/post/hexo-guide/
