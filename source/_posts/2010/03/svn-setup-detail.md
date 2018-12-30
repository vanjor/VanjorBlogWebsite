---
title: SVN服务器相关深入配置
date: 2010-03-18 00:47:48
updated: 2010-03-18 00:47:48
categories: 知识积累
tags:
  - Subversion
---

针对**Subversion** 的配置，在上一文中简要的说明最快捷的配置与使用，今天尝试进行权限配置时，发现了不少问题，现总结如下:

<!-- more -->

# 版本库及服务创建之一

通过subversion的svnserve启动apache服务：

## Subversion 的版本库（repository)

位于服务器端，统一管理和储存数据的地方。介绍在服务器端配置和管理 Subversion 版本库的基本方法，Linux 与Windows中的配置差别不大。

## 版本库数据存储方式

在 Subversion 中，版本库的数据存储有两种方式，一种是在 Berkeley DB 数据库中存放数据；另一种是使用普通文件，采用自定义的格式来储存，称为 FSFS。（两种存放方式各有优缺点，参考 [svnbook](http://svnbook.org/) 上面的文档来了解两者详细的比较和区别）

## 版本库服务启动

Subversion 设计了一个抽象的网络层，版本库建立完毕之后，可以通过各种服务器向外公布。svnserve 是 Subversion 自带的一个小型的服务器，它使用独立的协议与客户端。我们可以通过 :

```shell
svnserve –i #作为 inetd 启动
svnserve –d #作为守护进程启动一个服务。
```

同时可以指定一些选项，常用的如 -r，用来指定版本库的根路径，例如假设版本库位于 E:/svn

```shell
svnserve –d -r E:/svn #即创建了以E:/svn为根目录的SVN版本库服务。
```

## 设置为系统服务并自动启动

```shell
svnserve --daemon --root E:\\svn\\repository
```

服务启动--daemon可简写为-d、--root可简写为-r可以建立个批处理文件并放在windows启动组中便于开机 就运行SVN服务或者在这个[地址](http://clanlib.org/~mbn/svnservice/)下载那个svnservice.exe文件拷贝到 命令行下执行:

```shell
svnservice -install --daemon --root "D:\\svn\\Repository"
sc config svnservice start= auto
net start svnservice
```

## 创建子版本库（或版本库）

假设以以上"E:/svn"为根目录，并启动svn服务器，那么创建子版本库有两种方式：

* 通过TortoiseSVN等svn客户端工具创建版本库：在E:/svn中新建子目录project1，并进入E：/svn/project1 中通过TortoiseSVN菜单创建版本库，则这个版本库可以通过svn://服务器IP/project1"访问,以此类推可建立project2等等，并且这样各个子项目库相关权限配置保持独立。
* 通过subSVN服务器命令创建版本库, 创建版本库的命令，指定数据存储为 FSFS，如果要指定为 Berkeley DB，则将 fsfs 改为 bdb

```shell
svnadmin create --fs-type fsfs /etc/svn/repos
```

## 版本库下目录文件说明

* conf 目录：存放了版本库的配置文件，包括用户访问控制和权限控制等内容，文件本身的注释说明十分详细，读者可以根据注释自行配置；
* dav 目录：是提供给 Apache 相关模块的目录，目前为空；
* db 目录：存放着 Subversion 所要管理的所有受版本控制的数据，不同的存储方式（Berkeley DB 或者 FSFS）下有着不同的目录结构，不过我们一般不用直接修改和查看这个目录下的内容，Subversion 的命令可以安全的操作这个目录；
* hooks 目录：存放着钩子脚本及其模版（一种版本库事件触发程序）
* locks 目录：存放着 Subversion 版本库锁定数据，format 文件记录了版本库的布局版本号。

## 推荐项目程序目录结构如下

``` shell
|――project_A  
| ――――branches
| ――――tags
| ――――trunk
```

可以通过客户端建立好相关目录，然后提交到版本库或子版本库中。

# 版本库及服务创建之二

通过Apache Http Server服务启动svnserve服务：

## 特点说明

区别于第一种：svnserve 只能对全局提供简单的访问控制，如果想要更加灵活的方式，可以使用 Apache Http Server 作为向外公布版本库的方式，同时可以通过HTTP访问。

## 配置步骤

* 安装 mod_dav_svn 插件：用途使 Subversion 与 dav 模块通信，需要安装 mod\_dav\_svn 插件，可以在 Subversion 的安装目录中找到。将其拷贝到 Apache 安装目录的 modules 文件夹下。（由于 Subversion 需要版本化的控制，因此标准的 Http 协议不能满足需求。要让 Apache 与 Subversion 协同工作，需要使用 WebDAV（Web 分布式创作和版本控制）。WebDAV 是 HTTP 1.1 的扩展，关于 WebDAV 的规范和工作原理，可以参考 IETF RFC 2518。）
* 并配置 Apache 的 httpd.conf 文件，让 Apache 在启动的时候加载上述模块。需要添加的内容如下：

  ```shell
  LoadModule dav\_module modules/mod\_dav.so
  LoadModule dav\_svn\_module modules/mod\_dav\_svn.so
  #首先需要启用 dav\_module，然后加载 dav\_svn_module
  ```

## 版本库建立

在Apache的httpd.conf文件中末尾加入如下配置信息：

```shell
  DAV svn
  SVNPath /etc/svn/repos
```

Location 标签指出访问的 URL 以及在服务器上的实际位置。配置完毕后重新启动 Apache，打开浏览器，输入 <http://服务器IP/repos>

## 子版本库建立

如果想要指定多个版本库，可以用多个 Location 标签，也可以使用 SVNParentPath 代替 SVNPath，例如在 /etc/svn 下有多个版本库 repos1，repos2 等等，用如下方式指定：

```shell
  DAV svn
  SVNParentPath /etc/svn
```

"SVNParentPath /etc/svn" 表示 /etc/svn 下的每个子目录都是一个版本库。可以通过 <http://服务器IP/repos/repos1>，<http://服务器IP/repos/repos2> 来访问。

## 版本库目录权限配置

### 相关配置文件说明

svn版本库下的子目录conf下初始会有三个文件：

* svnserve.conf
* passwd
* authz

### svnserve.conf文件配置

被用来配置来进行一些简单的访问权限控制。文件的初始内容大致如下：

```config
[general]
# anon-access = read
# auth-access = write
password-db = passwd
# authz-db = authz
# realm = My First Repository
```

其中:

* `anon-access` 表示匿名用户的权限
* `auth-access` 表示认证用户的权限设置
* `password-db` 指向保存用户帐号密码的文件的位置,可以使用相对路径。

去掉#即可开启对应配置功能

### passwd文件配置

需让svnsere.conf中开启password-db = passwd，可让以配置如下

```config
[user]
user1 = password1
user2 = password2
# 格式为：“用户名 = 密码”注意前后不要有空格，中间要有空格
```

### authz文件配置

需让svnsere.conf中开启authz-db = authz，可以配置如下

```shell
#两个分组：committers，developers
[groups]
committers = paulex,richard
developers = jimmy,michel,spark,sean


[/]
#在根目录下指定所有的用户有读权限
* = r
#追加 committers 组用户有读写权限
@committers = rw

#在 branches/dev 目录下指定 developers 组的用户有读写权限
[/branches/dev]
@developers = rw

#在 /tags 组下给予用户 tony 读写权限
[/tags]
tony = rw

#禁止所有用户访问 /private 目录
[/private]
* =
```

以上对于1章节中的以svnserve启动svn服务直接有效。

## Apache Http Server服务启动svnserve模块权限配置说明

### Apache 提供了基本的全局粒度权限设置

* 首先需要创建一个用户文件。Apache 提供了一个工具 htpasswd，用于生成用户文件，可以在 Apache 的安装目录下找到。具体使用方法如下： htpasswd etc/svn/passwordfile username 如果 passwordfile 不存在，可以加上 -c 选项让 htpasswd 新建一个。创建好的文件内容是用户名加上密码的 MD5 密文。
* 接下来修改 httpd.conf，在 Location 标签中加入如下内容：

  ```config
  AuthType Basic
  AuthName "svn repos"
  AuthUserFile /etc/svn/passwordfile
  Require valid-user
  ```

重新启动 Apache， 打开浏览器访问版本库。Apache 会提示你输入用户名和密码来认证登陆了。 现在只有 passwordfile 文件中设定的用户才可以访问版本库。也可以配置只有特定用户可以访问，替换上述 "Require valid-user" 为 "Require user tony robert" 将只有 tony 和 robert 可以访问该版本库。 有的时候也许不需要这样严格的访问控制，例如大多数开源项目允许匿名的读取操作，而只有认证用户才允许写操作。为了实现更为细致的权限认证，可以使用 Limit 和 LimitExcept 标签。例如：

```config
require valid-user
```

如上配置将使匿名用户有读取权限，而限制只有 passwordfile 中配置的用户可以使用写操作。

### 目录粒度权限访问设置

使用 Apache 的 mod\_authz\_svn 模块对每个目录进行认证操作。

首先需要让 Apache 将 mod\_authz\_svn 模块加载进来。在 Subversion 的安装目录中找到 mod\_auth\_svn 模块，将其拷贝到 Apache 安装目录的 modules 子目录下。修改 httpd.conf 文件，找到

```config
LoadModule dav\_svn\_module modules/mod\_dav\_svn.so
```

在其后面加上

```config
LoadModule authz\_svn\_module modules/mod\_authz\_svn.so
```

现在可以在 Location 标签中使用 authz 的功能了。一个基本的 authz 配置如下：

```config
  DAV svn
  SVNPath /etc/svn/repos
  AuthType Basic
  AuthName "svn repos"
  AuthUserFile /etc/svn/passwd
  AuthzSVNAccessFile /etc/svn/accesspolicy
  Satisfy Any
  Require valid-user
```

其中AuthUserFile和AuthzSVNAccessFile两个文件就是指向3章节中的 passwd和authz策略文件 指向的是 authz 的策略文件

使用 SVNParentPath 代替 SVNPath 来指定多个版本库的父目录时，其中所有的版本库都将按照这个策略文件配置。例如上例中 tony 将对所有版本库里的 /tags 目录具有读写权限。如果要对具体每个版本库配置，用如下的语法：

```config
[groups]
project1_committers = paulex richard
project2_committers = jimmy michel spark sean \
           steven tony robert
[repos1:/]
* = r
@ project1_committer = rw
[repos2:/]
* = r
@ project2_committer = rw
```

这样项目1的 committer 组只能对 repos1 版本库下的文件具有写权限而不能修改版本库 repos2，同样项目2的 commiter 也不能修改 repos1 版本库的文件。

## 补充

### MySQL用户认证信息保护

到目前为止我们的用户名密码文件还是以文本文件形式存放在文件系统中的，出于安全性的需要或者单点登陆等可扩展性的考虑，文本文件的管理方式都不能满足需求了。通过 Apache 的 module\_auth\_mysql 模块，我们可以用 MySQL 来保存用户信息。该模块的主页在 [Sourceforge](http://modauthmysql.sourceforge.net/)，你也可以在 [Apache](http://modules.apache.org/) 找到它的发行版本。安装方法同上述 Apache 的模块一样，拷贝至 modules 目录并在 httpd.conf 文件中添加如下语句：

```config
LoadModule mysql\_auth\_module modules/mod\_auth\_mysql.so
```

相应的 Location 区域改写为：

```config
  AuthName "MySQL auth"
  AuthType Basic
  AuthMySQLHost localhost
  AuthMySQLCryptedPasswords Off
  AuthMySQLUser root
  AuthMySQLDB svn
  AuthMySQLUserTable users
  require valid-user
```

然后在 mysql 中添加名为 svn 的数据库，并建立 users 数据表：

```sql
create database svn;
use svn;
CREATE TABLE users (
user_name CHAR(30) NOT NULL,
user_passwd CHAR(20) NOT NULL,
user_group CHAR(10),
PRIMARY KEY (user_name)
);
```

在 users 表中插入用户信息

```sql
insert into users values('username','password','group');
```

重新启动 Apache，在访问版本库的时候 Apache 就会用 mysql 数据表中的用户信息来验证了。

### SSL实现加密连接

通过 Apache 的网络链接，版本库中的代码和数据可以在互联网上传输，为了避免数据的明文传输，实现安全的版本控制，需要对数据的传输进行加密。Apache 提供了基于 SSL 的数据传输加密模块 mod_ssl，有了它，用户就可以用 https 协议访问版本库，从而实现数据的加密传输了。SSL 协议及其实现方式，是一个非常复杂的话题，本文只是介绍 Apache 提供的最基本的SSL配置方法，更加详细的介绍内容，请参考 [Apache](http://httpd.apache.org/docs-2.0/ssl/) 上的文档。

开始配置前，我们需要一个实现 Apache 中 SSL 模块的动态程序库，通常名为 mod_ssl.so，及其配置文件，通常名为 ssl.conf。这个实现是跟 Apache 的版本相关的，不匹配的版本是不能用的；而且，并不是每一个 Apache 的版本都自带了相关实现文件，很多情况下，我们需要自己去搜寻相关文件。另外，我们还需要 OpenSSL 软件及其配置文件，来生成加密密钥和数字证书。这里，我们可以使用一些免费网站，如 [campbus](http://hunter.campbus.com/) 上提供的集成版本的 Apache。

有了相关的工具和文件，我们就可以开始生成 SSL 的证书和密钥了。首先，我们需要找到 openssl 程序及其配置文件 openssl.cnf，运行如下命令来生成 128 位的 RSA 私有密钥文件：

```config
my-server.key：
openssl genrsa -des3 -out my-server.key 1024
Loading 'screen' into random state - done
Generating RSA private key, 1024 bit long modulus
.....++++++
........++++++
e is 65537 (0x10001)
Enter pass phrase for server.key:********
Verifying - Enter pass phrase for server.key:********
```

命令运行期间需要用户输入并确认自己的密码。

现在，我们需要 SSL 的认证证书，证书是由 CA（certificate authority） 发放并且认证的。为此，我们可以用如下命令生成一个 CSR(Certificate Signing Request) 文件发给 CA，从而得到 CA 的认证：

```config
openssl req -new -key my-server.key -out my-s erver.csr -config openssl.cnf
```

当然，一般情况下，如果 Subversion 的用户不是太多，安全情况不是很复杂，我们也可以生成一个自签名的认证证书，从而省去了向 CA 申请认证的麻烦。如下命令：

```config
openssl req -new -key my-server.key -x509 -out my-server.crt -config openssl.cnf
```

以上两个命令都需要用户输入那个 key 文件的密码，以及一些网络设置信息，如域名，邮箱等等，这里输入的服务器域名应该与 Apache 配置文件当中的一致。现在，我们可以在 Apache 的 conf 目录下新建一个 ssl 目录，将 my-server.key 和 my-server.crt 文件都移动到 ssl 目录里面。然后修改 ssl.conf 文件，将 SSLCertificateKeyFile 和 SSLCertificateFile 项指向这两个文件。

如果 Apache 的 module 目录里面没有 mod_ssl.so 文件，可以将事先准备好的文件拷贝过去。然后，我们可以设置 Apache 的配置文件 httpd.conf，将 ssl 模块加入其中：

```config
LoadModule ssl\_module modules/mod\_ssl.so
```

然后，在配置文件的最后，加上如下 SSL 相关配置项：

```config
SSLMutex default
SSLRandomSeed startup builtin
SSLSessionCache none
ErrorLog logs/SSL.log
LogLevel info

SSLEngine On
SSLCertificateFile conf/ssl/my-server.crt
SSLCertificateKeyFile conf/ssl/my-server.key
```

这样，基本的设置工作就完成了。重新启动 Apache 服务器，现在可以用 https 协议代替 http 协议来访问版本库了。如果要限定版本库只能用 https 访问，我们可以在 Apache 配置文件当中 Subversion 部分加上 “SSLRequireSSL”。如下：

```config
DAV svn
SVNPath /etc/svn/repos
#other items
SSLRequireSSL
```

# 文章结

目前除了第5章和apache server启动服务没有亲自测试实现，其他均尝试成功，全文参考资料[《用 Apache 和 Subversion 搭建安全的版本控制环境》](http://www.ibm.com/developerworks/cn/java/j-lo-apache-subversion/ "用 Apache 和 Subversion 搭建安全的版本控制环境")，以及其他资料，并根据个人实验理解，重新整理。

版本控制相关概念详见 [A Visual Guide to Version Control](http://betterexplained.com/articles/a-visual-guide-to-version-control/ "A Visual Guide to Version Control")