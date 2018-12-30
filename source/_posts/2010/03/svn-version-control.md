---
title: SVN版本控制开发规范化
date: 2010-03-13 00:13:37
updated: 2010-03-13 00:13:37
categories: 知识积累
tags:
  - Subversion
---

终于忍受不了项目文档QQ传来传去的痛苦，决定在让SVN介入进来，进行版本控制，这才捡起荒废半年多的SVN。整理下Windows下SVN 服务器，客户端基本的安装配置，及常用操作命令。

<!-- more -->

# 客户端-TortoiseSVN

* 官方下载地址：[http://tortoisesvn.net/downloads](http://tortoisesvn.net/downloads)
* 比如下好TortoiseSVN-1.6.7.18415-win32-svn-1.6.9.msi 并安装

# 服务器端-Subversion

* 官方网址：[http://subversion.apache.org/](http://subversion.apache.org/)
* 下载安装后，需重启。(如没有安装Apache HTTP Server，请先安装 )

# 服务器端创建版本库

有两种方式创建版本库-Repository：

* 通过控制台运行 “svnadmin create F:\\svndir” 就会在目录F:\\svndir 创建好版本库
* 通过客户端TortoiseSVN菜单工具在目录F:\\svndir下"右键->TortoiseSVN->Create Repository here

# 配置服务器权限

主要为用户名密码设置，其他有用户权限控制等等：

* 转到 F:\\svndir\\conf目录，修改svnserve.conf：

```config
# password-db = passwd
```

改为：

```config
password-db = passwd
```

* 修改同目录的passwd文件：

```config
#[users]
# harry = harryssecret
# sally = sallyssecret
```

底下增加一行，格式为：用户名=密码，如下

```config
username = password
```

# 运行版本库服务器

运行服务器同样也有两种方式：

* 通过每次启动后控制台运行“svnserve -d -r F:\\svndir”
* 通过配置系统服务，控制台运行一下设置:

    ```shell
    sc create svnserve binPath= "\\"C:\\Program Files\\Subversion\\bin\\svnserve.exe\\" --service --root F:\\svndir" displayname= "Subversion Repository" depend= Tcpip start= auto
    ```

注：sc是windows自带的服务配置程序，参数binPath表示svnserve可执行文件的安装路径，由于路径中的"Program Files"带有空格，因此整个路径需要用双引号引起来。而双引号本身是个特殊字符，需要进行转移，因此在路径前后的两个双引号都需要写成\\"，--service参数表示以windows服务的形式运行，--root指明svn repository的位置，service参数与root参数都作为binPath的一部分，因此与svnserve.exe的路径一起被包含在一对双引号当中，而这对双引号不需要进行转义。displayname表示在windows服务列表中显示的名字， depend =Tcpip 表示svnserve服务的运行需要tcpip服务，start=auto表示开机后自动运行。

若要卸载服务：执行"sc delete svnserve"

# 初始化导入

来到我们想要导入的项目根目录，F:\\workspace\\svnproject：右键->TortoiseSVN->Import... URL of repository（版本库URL）输入“svn://localhost/svnproject”

完成之后目录没有任何变化，如果没有报错，数据就已经全部导入到了我们刚才定义的版本库中。非本机访问网络URL格式为：“svn://xxx.xxx.xxx.xxx/svnproject”。

# 客户端-TortoiseSVN常用操作说明

* SVN Commit：在你做了修改之后，你可以在项目文件夹下点击右键或者你修改的文件下点击右键，选择 SVN Commit...，这两者的区别在于，第一个可以一次提交你所做所有文件的修改，而第二个只是提交你所选的文件。
* Import：如果翻译插件或者写了插件，想提交到远程服务器，选择该文件夹，点击右键，选择 TortoiseSVN => Import...
* SVN Update：与服务器版本对比，进行更新
* Revert：取消上一次的操作（只针对客户端，服务端不做改动）
* Add：增加新目录或新文件至项目
* Revision Graph：版本示意图
* Show log：查看版本日志及不同版本间相互比较
* Check for modifications：同服务器上的项目版本进行比较，并可做相应的修改。

更详细使用说明：[TortoiseSVN中文手册](http://www.subversion.org.cn/tsvndoc/)

更详细安装文档：[SVN服务器端安装配置](http://wenku.baidu.com/view/09e5a417866fb84ae45c8d95.html)