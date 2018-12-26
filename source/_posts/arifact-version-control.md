---
title: Arifact打包版本管理
date: 2016-10-20 10:00:00
tags:
  - java
  - maven
  - snapshot
  - release
categories: 知识积累
---

Java打包与依赖奠定了软件包发布与版本管理的依赖的一个业界规范。

<!-- more -->

# 版本号

Maven中版本号类似1.0,1.1,1.1.1,主题规则如下
> <主版本>.<次版本>.<增量版本>
其中:

* 主版本: 重大架构变更，类似maven1和maven2，兼容性业不一定保证的，Gitlab项目以年为粒度迭代
* 次版本：功能模块级别的变更，Gitlab项目已月为粒度迭代
* 增量版本：补丁buffix级别，一般针对次版本的发布的功能等快速修复，Gitlab以天或周级别迭代

# snapshot与release区别

release是稳定版，但在开发中为了解决间依赖的项目间并行开发，可以基于snapshot快速迭代，而不需要频繁变更pom.xml中依赖的版本

* snapshot：用于保存开发过程中不稳定版本，一般允许redeploy
* release：用于保存稳定发布的版本，一般不允许redeploy

假设当前CodeOne，已发布1.0.0 release版本，现在开发新的功能。

1. 更新pom版本从1.0.0 到 1.1.0-SNAPSHOT
2. 不断开发功能，持续发布1.1.0-SNAPSHOT版本

开发完毕后需要正式发布
3. 更新pom版本从1.1.0-SNAPSHOT 到 1.1.0，并项目打上tag 1.1.0
完成1.1.0 release正式版发布.ss

注意: 一般发布是不分源代码项目分支的，如果多个分支需要各自迭代，可实现安排号发布版本号区别，比如 1.1.0-SNAPSHOT与1.2.0-SNAPSHOT

# maven发布规则

## snapshot发布

```xml
<groupId>com.vanjor</groupId>
<artifactId>demo</artifactId>
<version>0.1-SNAPSHOT</version>
<packaging>jar</packaging>

```

-SNAPSHOT(必须大写)，maven将自动判断为snapshot发布

如果发布到 <http://central.maven.org/maven2/> 中，则实际发布地址为：
> <http://central.maven.org/maven2/com/vanjor/demo/0.1-SNAPSHOT/demo-0.1-20161020.091255-1.jar>

其中demo-0.1-20161020.091255-1.jar, snapshot将自动被转换为 日期-时间-迭代次数

## release发布

```xml
<groupId>com.vanjor</groupId>
<artifactId>demo</artifactId>
<version>0.1</version>
<packaging>jar</packaging>
```

如果发布到 <http://central.maven.org/maven2/> 中，则实际发布地址为：
> <http://central.maven.org/maven2/com/vanjor/demo/0.1/demo-0.1.jar>

# 参考

* <http://juvenshun.iteye.com/blog/376422>
* <http://liyixing1.iteye.com/blog/2171254>
