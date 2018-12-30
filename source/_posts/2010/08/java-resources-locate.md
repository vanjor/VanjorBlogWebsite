---
title: 最佳实践-Java资源路径定位
date: 2010-08-29 12:09:39
updated: 2010-08-29 12:09:39
categories: 知识积累
tags:
  - Java
---

Java编程中经常会涉及到其他文件资源对位查找，比如类反射，配置文件，数据文件读写，如何更准确灵活的定位好资源保证程序移植健壮性，也是一个值得关注的问题。

<!-- more -->

# Java资源标识符

关键词：**URI、URL、URN**

* URI: Uniform Resource Identifier,(see RFC1630)，有两大子类型URL与URN
* URL: Uniform Resource Locator,(see URC1738)
* URN:Uniform Resource Name

它们的Scheme都是\[scheme:\]scheme-specific-part

## URI

URI以scheme和冒号开头。Scheme用大写/小写字母开头，后面为空或者跟着更多的大写/小写字母、数字、加号、减号和点号。冒号把scheme与scheme-specific-part分开了，并且scheme-specific-part的语法和语义（意思）由URI的名字空间决定。

SAMPLE：

* 绝对URI：
  * <http://g.cn>
  * <ftp://vanjor@vanjor.com:90/demo>
  * mailto:vanjor@vanjor.org
* 相对URI：demo/a.doc （作为相对资源引用依赖于运行环境）

API：

* URI(String arg); (return URI)
* toURL(); (return URL)

API中解析：

uri.getFragment()、uri.getHost()、uri.getPath()、uri.getPort()、uri.getQuery()、uri.getScheme()、uri.getSchemeSpecificPart()、uri.getUserInfo()、uri.isAbsolute()、uri.isOpaque();

[扩展阅读](http://download.oracle.com/javase/1.4.2/docs/api/java/net/URI.html)

## URL

URL的一种更新形式，统一资源名称(URN, Uniform Resource Name)不依赖于位置，并且有可能减少失效连接的个数。但是其流行还需假以时日，因为它需要更精密软件的支持。

API:

* URL(String arg);(return URL)
* toURI;(return URI);

API中初步分析：getAuthority()、getDefaultPort()、 getFile()、 getHost()、 getPath()、getPort()、getProtocol()、getQuery()、getRef()、getUserInfo()、getDefaultPort()

[扩展阅读](http://www.jar114.com/jdk6/zh_CN/api/java/net/URL.html)

## URN

URL的一种更新形式，统一资源名称(URN, Uniform Resource Name)不依赖于位置，并且有可能减少失效连接的个数。但是其流行还需假以时日，因为它需要更精密软件的支持。

# Java中相对路径

Java中对资源定位是基于1中的统一资源定位符，主要有相对路径与绝对路径，其中相对路径是依赖于系统运行环境， 绝对路径：如
> File a = new File("C://a.txt");

相对路径
> File b = new File("a.txt");System.getProperty("user.dir");

user.dir为当前OS当前用户的默认目录

## 几种java本地应用程序路径获取方式

 **Scenario**：对应某个demo程序MyClass.java编译后的MyClass.class输出路径为
> “C://dev/demo/bin/org/vanjor/demo/MyClass.class”

那么运用下述方法得到路径分别为：

| Method                                                         | Notes                                                                   | Result for demo             |
| -------------------------------------------------------------- | ----------------------------------------------------------------------- | --------------------------- |
| MyClass.class.getResource("")                                  | 获得当前类MyClass。class文件的URI目录，不包括class文件名                | “file:/C:/dev/demo/bin/org/ |
| vanjor/demo”                                                   |                                                                         |                             |
| MyClass.class.getResource(“/”)                                 | 得到当前classpath的绝对基准URI路径                                      | “file:/C:/dev/demo/bin/”    |
| Thread.currentThread().getContextClassLoader().getResource("") | 通过java SDK中类得到当前classpath的绝对基准URI路径                      | “file:/C:/dev/demo/bin/”    |
| MyClass.class.getClassLoader().getResource("")                 | 得到当前classpath的绝对基准URI路径                                      | “file:/C:/dev/demo/bin/”    |
| ClassLoader.getSystemResource("")                              | 通过java.lang.ClassLoader的静态方法，得到当前classpath的绝对基准URI路径 | “file:/C:/dev/demo/bin/”    |

推荐使用Thread.currentThread().getContextClassLoader().getResource("");来得到当前的classpath的绝对路径的URI表示法。对于在classpath路径外的资源定位，可以通过先获取classpath绝对路径路径，再运用../等方法解析路径外的资源的绝对位置。

## Web应用程序中资源的寻址

Web中运行环境可能有差异，在JavaSE程序中，我们一般使用classpath来作为存放资源的目的地。但是，在Web应用程序中，我们一般使用classpath外面的WEB-INF及其子目录作为资源文件的存放地。 Eclipse中Web中class输出环境默认配置为：

```xml
<classpathentry kind="output" path="WebRoot/WEB-INF/classes"/>
```

而应用程序一般是相对src同级别有一个bin目录，配置为：

```xml
<classpathentry kind="output" path="bin"/>
```

在Web中，通常可以用在Web应用程序中，我们一般通过ServletContext.getRealPath("/")方法得到Web应用程序的根目录的绝对路径。 但是在Junit测试中也会面临没有ServletContext所需Web容器环境的问题，这里依然可以通过ClassLoader类来获得路径。

参考

* URI，URL，URN详解：[http://eastsun.javaeye.com/blog/37013](http://eastsun.javaeye.com/blog/37013)
* Java路径解决方案：[http://studyroom.ccut.edu.cn/article.php?articleid=8765](http://studyroom.ccut.edu.cn/article.php?articleid=8765&pagenum=-1)