---
title: Git学习指南
date: 2016-05-06 17:15:00
tags:
  - git
categories: 最佳实践
---

![article-logo](https://ws1.sinaimg.cn/large/006tNbRwly1fyk93b0u54j30xc0hi759.jpg)

<!-- more -->

# Git 工作原理

Git作为一种分布式代码仓库，便在设计上区别与SVN

## git 工作流

### simplest

![git workflow basic](https://ws4.sinaimg.cn/large/006tNbRwly1fyk93ufs6oj30uy0k5757.jpg)

### detailer

![git workflow complex](https://ws2.sinaimg.cn/large/006tNbRwly1fyk94abf3wj30dm0b0dft.jpg)

### comprehension

![complex workflow](https://ws4.sinaimg.cn/large/006tNbRwly1fyl9gxi6mhj30sg0ift9c.jpg)

### multiple remote repos

![multiple remote repos](https://ws2.sinaimg.cn/large/006tNbRwly1fyk963sdybj30sg0r4tak.jpg)

### 扩展参考

* [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
* [Our Magento Git Guide and Work Flow](https://www.sonassi.com/blog/knowledge-base/our-magento-git-guide-and-work-flow)
* [What is the best Git cheat sheet?](https://www.quora.com/What-is-the-best-Git-cheat-sheet)

# Git 常用命令清单

重度参考 [常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html) ，有增删改纠。 以下为常用的20几个命令分类介绍

## 新建代码库

```shell
# 新建一个目录，并初始化为本地Git代码库
$ git init [project-name]

# 下载一个远程Git项目代码库到指定目录
$ git clone <repository_uri> [directory]
```

## 配置

```shell
# 查看当前git配置
$ git config --list

# 编辑git配置, global为用户全局配置
$ git config -e [--global]

# 配置当前用户信息，提交代码需要
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"

# 配置项目git忽略文件/夹列表, 项目根目录创建.gitignore文件
$ echo ".project" >> .gitigore
```

## 增删文件

```shell
# 添加文件/目录到暂存区(stage/index)
$ git add [file1] [file2]

# 添加当前目录所有文件到暂存区
$ git add .

# 添加当前项目所有文件到暂存区
$ git add --all

# 删除[递归]指定workspace文件或目录
$ git rm [-r] [file1] [file2]

# 停止追踪指定文件，git add的反向操作
$ git rm --cached [file]
```

## 代码提交

```shell
# 将变更从暂存区提交到本地仓库
$ git commit -m [message]

# 自动stage哪些已经被修改或删除的文件（新增文件无效）, 一并提交到本地仓库
$ git commit -am [message]

# 使用一次新的commit，替代上一次提交，并包含当前stage的变更
$ git commit --amend -m [message]
```

## 分支与合并

```shell
# 列举所有本地分支, -a 包含远程分支
$ git branch [-a]

# 对当前分支新建一个分支，仍停留在当前分支
$  git branch [branch-name]

# 对当前分支新建一个分支，切换工作区间到新分支
$ git checkout -b [branch-name]

# 合并指定分支到当前分支
$ git merge [branch-name]

# 选择一个commit，仅合并改commit到当前分支
$ git cherry-pick [commit-hashcode]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git branch -dr [branch-name]
$ git push origin --delete [branch-name]
```

## 查看信息

```shell
# 显示变更文件
$ git status

# 显示当前仓库中所有版本历史, stat显示变更文件
$ git log [--stat]

# 显示某个文件的历史版本，包括改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件每一行是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和本地仓库(最后一次commit)的差异
$ git diff --cached

# 显示工作区和本地仓库(最后一次commit)的差异
$ git diff HEAD

# 显示某次提交的具体变动内容
$ git show [commit-hashcode]

# 显示本地仓库最近几次提交 (当前分支)
$ git reflog [branch]

```

## 远程同步

```shell
# 取回远程仓库所有变动
$ git fetch

# 取回远程仓库所有变动到本地仓库，并与本地分支合并
# 相当于 git fetch & git merge
$ git pull [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```

## 撤销

```shell
# 恢复暂存区的指定文件(最后一次添加到暂存区的版本)到工作区，清除工作区改文件的修改
$ git checkout [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变， git add 逆向操作
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

# Git 的特点

## Git与SVN常用命令对比

| 操作     | svn             | git          |
| -------- | --------------- | ------------ |
| 创建     | svnadmin create | git init     |
| 签出     | svn checkout    | git clone    |
| 更新     | svn update      | git pull     |
| 添加     | svn add         | git add      |
| 提交     | svn commit      | git commit   |
| 查看状态 | svn status      | git status   |
| 合并     | svn merge       | git merge    |
| 回滚     | svn revert      | git checkout |
| diff     | svn diff        | git diff     |
| 创建分支 | svn copy        | git branch   |
| 切换分支 | git checkout    | svn switch   |

## Git 特点

* 分布式代码版本控制
* 基于清点引用的方式进行变更追踪

# Git 完整命令清单

## Git 官方最常用命令

```shell
$ git help
The most commonly used git commands are:
 add        Add file contents to the index
 bisect     Find by binary search the change that introduced a bug
 branch     List, create, or delete branches
 checkout   Checkout a branch or paths to the working tree
 clone      Clone a repository into a new directory
 commit     Record changes to the repository
 diff       Show changes between commits, commit and working tree, etc
 fetch      Download objects and refs from another repository
 grep       Print lines matching a pattern
 init       Create an empty Git repository or reinitialize an existing one
 log        Show commit logs
 merge      Join two or more development histories together
 mv         Move or rename a file, a directory, or a symlink
 pull       Fetch from and integrate with another repository or a local branch
 push       Update remote refs along with associated objects
 rebase     Forward-port local commits to the updated upstream head
 reset      Reset current HEAD to the specified state
 rm         Remove files from the working tree and from the index
 show       Show various types of objects
 status     Show the working tree status
 tag        Create, list, delete or verify a tag object signed with GPG
```

## Git 全部命令清单

全部命令共155个

```shell
$ git help
  add                       fsck                      receive-pack
  add--interactive          fsck-objects              reflog
  am                        gc                        relink
  annotate                  get-tar-commit-id         remote
  apply                     grep                      remote-ext
  archimport                gui                       remote-fd
  archive                   gui--askpass              remote-ftp
  bisect                    hash-object               remote-ftps
  bisect--helper            help                      remote-http
  blame                     http-backend              remote-https
  branch                    http-fetch                remote-testsvn
  bundle                    http-push                 repack
  cat-file                  index-pack                replace
  check-attr                init                      request-pull
  check-ignore              init-db                   rerere
  check-mailmap             instaweb                  reset
  check-ref-format          interpret-trailers        rev-list
  checkout                  log                       rev-parse
  checkout-index            ls-files                  revert
  cherry                    ls-remote                 rm
  cherry-pick               ls-tree                   send-email
  citool                    mailinfo                  send-pack
  clean                     mailsplit                 sh-i18n--envsubst
  clone                     merge                     shell
  column                    merge-base                shortlog
  commit                    merge-file                show
  commit-tree               merge-index               show-branch
  config                    merge-octopus             show-index
  count-objects             merge-one-file            show-ref
  credential                merge-ours                stage
  credential-cache          merge-recursive           stash
  credential-cache--daemon  merge-resolve             status
  credential-store          merge-subtree             stripspace
  cvsexportcommit           merge-tree                submodule
  cvsimport                 mergetool                 svn
  cvsserver                 mktag                     symbolic-ref
  daemon                    mktree                    tag
  describe                  mv                        unpack-file
  diff                      name-rev                  unpack-objects
  diff-files                notes                     update-index
  diff-index                p4                        update-ref
  diff-tree                 pack-objects              update-server-info
  difftool                  pack-redundant            upload-archive
  difftool--helper          pack-refs                 upload-pack
  fast-export               patch-id                  var
  fast-import               prune                     verify-commit
  fetch                     prune-packed              verify-pack
  fetch-pack                pull                      verify-tag
  filter-branch             push                      web--browse
  fmt-merge-msg             quiltimport               whatchanged
  for-each-ref              read-tree                 write-tree
  format-patch              rebase
```

* [Git 官方中文手册](https://git-scm.com/book/zh/v2)
* [Git远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
* [Git 参考手册-命令详解](http://gitref.org/zh/index.html)

## Git cheat sheet

![git-cheatsheet](https://ws2.sinaimg.cn/large/006tNbRwly1fyk97sbhsxj30sg0kddij.jpg)

# Git 深入学习

## git reset, git checkout, git revert 区别

svn主要有revert，而git有三个，

 git reset用于撤销未被提交到远端的改动。除了可以移动当前分支的HEAD，你可以通过不同的标记选择修改 staged snapshot 或者 working directory

* --soft： staged snapshot 和 working directory 都未被改变 (建议在命令行执行后，再输入 git status 查看状态)
* --mixed： staged snapshot 被更新， working directory 未被更改。【这是默认选项】（建议同上)
* --hard： staged snapshot 和 working directory 都将回退。

详细参考:  [Reset, Checkout, and Revert](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting), [中文翻译](http://blog.mexiqq.com/index.php/archives/3/)

## Merging vs. Rebasing

详细参考:  [Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

## Refs and the Reflog

详细参考:  [Refs and the Reflogg](https://www.atlassian.com/git/tutorials/refs-and-the-reflog)

## Git log 高级使用方法

详细参考:  [Advanced Git log](https://www.atlassian.com/git/tutorials/git-log)

## Git hacks

### 避免每次迁入迁出输入密码

[Git - How to avoid typing your password repeatedly](https://blog.sleeplessbeastie.eu/2012/08/12/git-how-to-avoid-typing-your-password-repeatedly/)

# Git Ops

## 如何搭建私有Git代码仓库

git本身是一种去中心化的设计，可以无需中心代码仓库而工作，大家协同工作可以基于各自私有的repos进行互相track开发。但企业难免需要有一个中心代码仓库，方便统一管理、权限控制、24小时服务。

严格来说任意一台联网机器至少装了git客户端就可以作为中心服务器，但不支持多种协议签出代码，没有权限控制等一系列功能。

目前最好的方式就是直接搭建[Gitlab](https://about.gitlab.com/features/)（开源免费）一站式服务，包含有全面的代码管理，权限，issue，wiki等功能，与github功能十分类似。

不基于Gitlab可以自己通过[Gitolite](http://gitolite.com/gitolite/index.html)(权限控制层模块)，[nginx](http://nginx.org)(提供http协议)，等自己组装实现。

* [Which is the best git hosting sw? - Gitolite vs. Gitlab vs.
 Gitorius](http://stackoverflow.com/questions/17167414/which-is-the-best-git-hosting-sw-gitolite-vs-gitlab-vs-gitorius)
* [搭建Git服务器](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)
* [配置Gitolite+Gitweb+Nginx](http://zodiacg.net/2014/05/gitolite_gitweb_nginx/)

# Git 开发流程与规范

![enter link description here](http://nvie.com/img/git-model@2x.png)

* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [Git 分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)

# Git commit message 编写规范

* [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
