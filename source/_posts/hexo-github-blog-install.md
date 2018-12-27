---
title: 基于Hexo在Github上搭建技术博客
date: 2016-05-04 15:15:00
tags:
  - hexo
  - github
  - markdown
categories: 最佳实践
published: true
---

![Github托管Hexo博客](https://ws4.sinaimg.cn/large/006tNbRwly1fyjdb5974cj30hs07sdgp.jpg)

# 序

选型配置如下:

* 博客语言: [Markdown](http://wowubuntu.com/markdown/) 可移植跨平台
* 博客程序: [Hexo](https://hexo.io)
* 博客主题: [NexT](https://theme-next.org)
* 静态博客托管: [Github](https://github.com) + [Coding.net](https://coding.net) 双线托管
* 域名DNS: [Aliyun](https://www.aliyun.cn/) 双线解析
* 自动部署: [travis-ci](https://travis-ci.org) 持续集成自动部署
* 写作编辑: [vscode](https://code.visualstudio.com) + [prose](https://prose.io) 离线&在线写作
* 图片工具: [iPic](https://itunes.apple.com/cn/app/ipic-markdown-图床-文件上传工具/id1101244278) 图床管理
* 评论&阅读数统计: [leancloud](https://leancloud.cn) 基于 next 的valine 插件实现

<!--more-->

为什么要从自建worldpress迁移过来，原因可参考阮一峰的[文章](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)，相比之下wordpress各种不省心并复杂繁琐，而现在方案每篇文章就是一个markdown文档，满足随心DIY。

> 第一阶段，刚接触Blog，觉得很新鲜，试着选择一个免费空间来写。
第二阶段，发现免费空间限制太多，就自己购买域名和空间，搭建独立博客。
第三阶段，觉得独立博客的管理太麻烦，最好在保留控制权的前提下，让别人来管，自己只负责写文章。

# Hexo 安装

Hexo搭建教简单，参考[官方教程](https://hexo.io/docs/setup.html) 10分钟能出hello world。

个人大致总结Hexo特点:

* 极易上手，几部命令完成
* 单一配置文件_config.yml 易懂扩展性强
* Hexo支持较好的部署功能，支持多种部署方式与多目标部署
* 模板与插件丰富，安装方便。 官方网站已经有多达百种供选择

其他可见: [开源博客横向评测](https://www.zhihu.com/question/21981094)

# 国内国外双线加速

国内访问gitlub不太稳定，通过Hexo方便的支持双线同时发布，并通过dnspod的智能路由解析功能实现，国内访问到是coding.net托管的，国外访问的是github托管的。

详细教程参考: [hexo同时托管到coding.net与github](https://segmentfault.com/a/1190000004548638)

附我的hexo的[_config.yml](https://github.com/vanjor/VanjorBlogWebsite/blob/source/_config.yml)

# 自动部署发布文章

Github支持Webhook可以自动触发自己主机上的发布脚本，但仍需要自己维护主机，利用travis-ci可以消除这一依赖。

完整教程参考: [用 Travis CI 自动部署 hexo](http://blog.acwong.org/2016/03/20/auto-deploy-hexo-with-travis-CI/)
注: ssh-keygen -t rsa -C "youremail@example.com" -t id_rsa 通过-t命令指定保存为当前目录的id_rsa文件。

附本博客:

* 源码系统项目: <https://github.com/vanjor/VanjorComWebsite>
  * branch: source as default branch is the source code
  * branch: master retain the compiled website code serve as Github Page service
* travis-ci自动编译部署历史: <https://travis-ci.org/vanjor/VanjorBlogWebsite>

# 博客的详细配置变更

对 hexo 定制化过的文件

* source 文档及一些图片资源
* _config.yml hexo 的配置
* package.json hexo 依赖
* themes/next/source/lib 扩展包安装
* themes/next/_config.yml 主体配置
* .travis.yml travis CI 配置
* .travis travis 所需 ssh key token 配置
* .gitignore git 所需
* README.md 文档

开启的的功能列表:

* valine + leancloud_visitors基于 leancloud 的评论与阅读数统计 [依赖三方网络服务]
* RSS
* local_search 搜索
* math公式支持
* han + pangu 文章编排美化
* 百度统计  [依赖三方网络服务]
* fancybox 图片相册支持
* pace 加载进度条
* post_edit 显示文章源码link
* symbols_count_time 字数与阅读时长显示
* icon 更新页底copyright信息
* 增加 tags, categories, archieves, about 四个子页面
* 开启social 社交网站链接
* Hexo 特有的 Tags Settings (开启，但暂没使用需要进一步评估)
* 增加百度谷歌访问分析
* 增加了 robots 协议