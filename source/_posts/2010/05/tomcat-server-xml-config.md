---
title: Tomcat核心server.xml配置
date: 2010-05-23 12:39:37
updated: 2010-05-23 12:39:37
categories: 知识积累
tags:
  - Tomcat
  - Webservice
---

默认安装好Tomcat后，通过IP+端口访问到的是Tomcat的管理页面，其他常规部署到Tomcat的webapps目录下的项目都会是默认二级站点结构，下面总结通过配置server.xml更改Tomcat默认访问项目以及部署多个虚拟主机.

<!-- more -->

# Tomcat配置文件解读

以6.0版本为列，主要有在conf目录下，主要context.xml,server.xml,web.xml,tomcat.users.xml

* context.xml,web.xml是tomcat为每个部署其中的项目一个默认配置，所有其他项目的配置无非是基于之上的添加修改
* server.xml是tomcat的核心配置文件，通过配置server.xml可以灵活设置虚拟主机
* tomcat.users.xml配置tomcat管理员账户等

server.xml个元素属性详参照[https://www.cnblogs.com/starhu/p/5599773.html](https://www.cnblogs.com/starhu/p/5599773.html)

** Tomcat虚拟目录配置

默认安装好Tomcat后，通过IP+port访问到的是Tomcat的管理页面，其他常规部署到Tomcat的webapps目录下的项目都会是默认二级站点结构，以下为默认的主机配置信息Host：

默认的访问目录为ROOT，Context值如下：（注：Context属性归属与某Host属性内）

```xml
<Context path="" docBase="ROOT" debug="0">
</Context>
```

表示默认访问Tomcat开启服务的IP：Port访问将是tomcat目录下ROOT对应的项目

可以通过更改docBase的值，配置为本地任何一个目录下的项目，以下给出一个较复杂的样列：

```xml
<Context path="/informanager" docBase="D:\\informanager\\WebRoot"
 reloadable="true">
    <Resource name="jdbc_mysql" type="javax.sql.DataSource"
    driverClassName="com.mysql.jdbc.Driver"
    url="jdbc:mysql://127.0.0.1:3306/informanagerautoReconnect=true&amp;
    URIEncoding=UTF-8&amp;useUnicode=true&amp;characterEncoding=UTF-8"
    username="root" password="123456" maxActive="20" maxIdle="10" maxWait="5000"/>
</Context>
```

# Tomcat虚拟主机配置

现在很多WWW上多个站点同时部署在一个IP服务器上，也可多个IP服务器对应一个站点，Tomcat也可通过配置server.xml来达到配置虚拟主机以下为默认最简化server.xml中的虚拟主机条目配置Host在很多WWW上多个站点同时部署在一个IP服务器上，也可多个IP服务器对应一个站点。

原理：HTTP/1.1协议中请求头域Host的值决定可以向同一IP发送请求，但是Host值不同，即请求的站点不同.

Tomcat同一IP多个虚拟主机配置可以通过可以通过基于主机名配置，也可通过基于端口号配置。

## 基于主机名配置

Tomcat的server.xml文件，在初始状态下，只包括一个虚拟主机，但是它容易被扩充到支持多个虚拟主机。增加虚拟主机只要增加完整Host标签即可。每一个Host元素必须包括一个或多个context元素，所包含的context元素中必须有一个是默认的context，这个默认的context的显示路径应该为空（例如，path=""）

```xml
<Host
appBase="webapps"
name="google.com">
<Alias>www.google.org</Alias>
<Alias>google.google.org</Alias>
<Alias>bluepure.google.org</Alias>
<Context path="/" docBase="vanjor" reloadable="true" >
</Context>
</Host>
```

这里通过设置Alias别名，可以将多个域名host映射到同一个站点项目，这个需要配置DNS解析信息，本地可以通过更改Hosts文件

（windows系统Windows\\System32\\drivers\\etc目录下），添加“127.0.0.1  google.com”等，可以模拟访问效果.

## 基于端口号配置

可以通过server.xml中增加一对Service属性信息，如下：更换端口为8088

```xml
<Service name="Catalina-2">
<Connector port="8088" protocol="HTTP/1.1"
connectionTimeout="20000"
redirectPort="8443" />
<Engine name="Catalina-2" defaultHost="localhost">
<Host name="localhost-2" appBase="webapps-2"
unpackWARs="true" autoDeploy="true"
xmlValidation="false" xmlNamespaceAware="false">
<Context path="" docBase="." debug="0">
</Context>
</Host>
</Engine>
</Service>
```