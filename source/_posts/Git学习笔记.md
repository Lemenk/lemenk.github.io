---
title: Git学习笔记
date: 2020-03-04 10:17:28
tags:
---

#### Git技巧总结

<!--more-->

# Git简介

## Git的诞生

1991年，Linus创建了开源的Linux，由于多人参与代码编写，代码管理十分麻烦。最初是由Linus手工方式合并的。2005年，Linus使用C语言编写了一个分布式版本控制系统，就是Git。

2008年，GitHub网站上线，可以免费且开源的方式提供Git存储。众多项目开始迁移至GitHub。

## 集中式和分布式

集中式版本控制系统：CVS、SVN

分布式版本控制系统：Git

区别：

1. 集中式版本控制系统：版本库存放在中央服务器，需要先从中央服务器获取最新版本的代码下载至本机，然后修改完成后再将代码推送至中央服务器。**需要联网**。
2. 分布式版本控制系统：每个人的电脑上都有一个完整的版本库，在修改完成后只要将修改的内容推送到对方或者存储库（如GitHub）即可。

**与集中式相比，分布式安全性能更高。而且还具有强大的分支管理功能。**

# Git的安装

## 在Windows上

1、在官网下载安装程序：[传送门](https://git-scm.com/downloads)

2、打开**Git Bash**

3、配置本机Git参数

~~~shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
~~~

## 在Linux上

[官网教程](https://git-scm.com/download/linux)

**Debian/Ubuntu**

`# apt-get install git`

**CentOS**

一、使用yum安装

1. 安装git：`yum install git`

2. 查看yum源仓库Git信息：`yum info git`

3. 安装依赖库：

   ~~~shell
   yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
   
   yum install gcc-c++ perl-ExtUtils-MakeMaker
   ~~~

二、通过源码构建，[下载地址](https://mirrors.edge.kernel.org/pub/software/scm/git/)。



查看版本信息：**git --version**



# 创建版本库

版本库，又名仓库，英文名为**repository**。是一个被Git管理起来的目录。

1、在Windows上进入**Git Bash**，在Linux中进入命令行

~~~she
# 创建名为test的目录
$ mkdir test

$ cd test
#查看目录
$ pwd
/c/~~~/Desktop/test
~~~

2、通过`git init`命令将其作为Git仓库

~~~she
$ git init
~~~

文件夹中会出现一个`.git`文件夹目录，默认是隐藏的。

3、编写文件，first.txt

4、用命令`git add`告诉Git，把文件添加到仓库：

提交名为`first.txt`的文件

~~~shell
$ git add first.txt
~~~

提交所有文件：

~~~shell
$ git add .
~~~

5、用命令`git commit`告诉Git，把文件提交到仓库：

~~~shell
# 双引号内为说明信息
$ git commit -m "第一次提交"
[master (root-commit) 9aa8dfe] 第一次提交
 1 file changed, 1 insertion(+)
 create mode 100644 first.txt
~~~

6、使用`git status`命令看看结果：

~~~shell
$ git status
On branch master
nothing to commit, working tree clean
~~~

# 修改删除文件

1. 修改first.txt文件

2. 运行`git status`命令看看结果：

   ~~~shell
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   first.txt
   ~~~

   提示first.txt文件已经被修改过了，使用`git add`添加到仓库或者使用`git restore`命令取消修改

3. 使用 git diff命令查看修改信息记录：

   ~~~shell
   $ git diff
   diff --git a/first.txt b/first.txt
   index ba9c1ad..84fc394 100644
   --- a/first.txt
   +++ b/first.txt
   @@ -1 +1,2 @@
    nihao
   +你好
   ~~~

4. 使用 `git restore`命令取消修改：

   ~~~shell
   $ git restore first.txt
   ~~~

   使用`git status`命令看看结果：

   ~~~shell
   $ git status
   On branch master
   nothing to commit, working tree clean
   ~~~



# 版本修改

1. 使用`git log`命令查看提交历史记录

   ~~~shell
   $ git log
   commit 925cdee529fb05cbdf3ae1b65e0d3199be173a2a (HEAD -> master)
   Author: 用户名和邮箱
   Date:   日期
   
       第二次提交
   
   commit 9aa8dfe6369f514a3b73f58be89ed048de4bed1f
   Author: 用户名和邮箱
   Date:   日期
   
       第一次提交
   ~~~

   其中，HEAD标识当前版本。上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，以此类推。也可以写成`HEAD~150`标识150个版本以前。

   可以根据日期查看提交的先后顺序。

2. 使用`git reset`命令回退到上一个版本

   ~~~shell
   $ git reset --hard HEAD^
   HEAD is now at 9aa8dfe 第一次提交
   ~~~

   当前first.txt内容已经改变。

3. 再使用`git log`查看日志：

   ~~~shell
   $ git log
   commit 9aa8dfe6369f514a3b73f58be89ed048de4bed1f (HEAD -> master)
   Author: Lemenk <Lemenk@163.com>
   Date:   Wed Mar 4 11:10:18 2020 +0800
   
       第一次提交
   ~~~

4. 如果想要回退到上次版本，可以根据版本号回退

   ~~~shell
   # 其中925cd为版本号前几位，并不用写全。git会自动查找。
   $ git reset --hard 925cd
   HEAD is now at 925cdee 第二次提交
   ~~~

5. 使用git reflog命令查看此前的每一次命令：

   ~~~shell
   $ git reflog
   925cdee (HEAD -> master) HEAD@{0}: reset: moving to 925cd
   9aa8dfe HEAD@{1}: reset: moving to HEAD^
   925cdee (HEAD -> master) HEAD@{2}: commit: 第二次提交
   9aa8dfe HEAD@{3}: commit (initial): 第一次提交
   ~~~

   

**总结**：Git中可以随时查看版本信息，并且可以通过命令进行回退操作。

Git管理的文件分为：工作区，版本库。

版本库又分为暂存区stage和暂存区分支master(仓库)

工作区----->>暂存区----->>仓库

git add把文件从工作区----->>暂存区，`git commit`把文件从暂存区----->>仓库，

`git diff`查看工作区和暂存区差异，

`git diff --cached`查看暂存区和仓库差异，

`git diff HEAD` 查看工作区和仓库的差异，

git add的反向命令`git checkout`或者`git restore`，撤销工作区修改，即把暂存区最新版本转移到工作区，

`git commit`的反向命令`git reset HEAD`或者`git restore --staged`，就是把仓库最新版本转移到暂存区。



# 远程仓库

使用GitHub或者Gitee创建远程仓库。

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以需要设置SSH Key。

1. 创建SSH Key

   ~~~shell
   ssh-keygen -t rsa -C "你的GitHub注册邮箱"
   ~~~

2. 在用户目录下可以看到.ssh文件夹。`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

3. 将`id_rsa.pub`内容全部复制，打开GitHub，“Account settings”，“SSH Keys”页面，填写随意的title，粘贴`id_rsa.pub`内容即可。

4. GitHub允许添加多个Key，只要将key添加到GitHub即可。



## 添加远程仓库

1. 创建一个新的仓库，Repository name最好与本地仓库名称一致。

2. 在本地仓库运行命令：关联远程仓库

   ~~~shell
   # 账户名处应为GitHub的用户名
   # origin为远程库的名字。可以更改，但建议使用该名称。
   # git部分参数也可以使用github上仓库的url地址
   $ git remote add origin git@github.com:Lemenk/test.git
   ~~~

3. 用`git pull`命令合并两个仓库

   ~~~shell
   # --allo……为参数
   $git pull origin master --allow-unrelated-histories
   ~~~

4. 用`git push`命令，把当前分支`master`推送到远程。

   ~~~shell
   # $ git push <远程主机名> <本地分支名>:<远程分支名>
   $git push origin master:master
   ~~~

## 克隆远程仓库

~~~shell
# url例如：https://github.com/Lemenk/test.git
$ git clone 仓库url
~~~

# 分支管理

在Git中，master为主分支，即master分支。

## 创建合并删除分支

1. 创建dev分支，并切换到dev分支。

   ~~~shell
   $ git switch -c dev
   Switched to a new branch 'dev'
   ~~~

   `git switch`命令加上`-c`参数表示创建并切换，相当于以下两条命令：

   ~~~shell
   $ git branch dev
   $ git switch dev
   Switched to branch 'dev'
   ~~~

2. 用`git branch`命令查看当前分支：

   ~~~shell
   $ git branch
   * dev
     master
   ~~~

   在当前分支前会标上`*`号。

3. 在dev分支上对文件进行修改，比如修改first.txt。

4. 提交：

   ~~~shell
   $ git commit -m "new branch"
   [dev 324481f] new branch
    2 files changed, 3 insertions(+)
   ~~~

5. 切换到master分支：

   ~~~shell
   $ git switch master
   Switched to branch 'master'
   ~~~

6. 此时查看文件时并没有修改的信息。，表明刚才的提交是在dev分支。

7. 合并分支`git merge`

   ~~~shell
   $ git merge dev
   Updating 8471d6f..324481f
   Fast-forward
    README.md | 2 ++
    first.txt | 1 +
    2 files changed, 3 insertions(+)
   ~~~

   `git merge`命令用于合并指定分支到当前分支。

8. 删除分支

   ~~~shell
   $ git branch -d dev
   Deleted branch dev (was 324481f).
   ~~~

9. 查看分支：

   ~~~shell
   $ git branch
   * master
   ~~~

## 合并冲突

在创建dev分支后，修改文件并提交。然后切换到master主分支修改文件后提交会自动进入vi页面。

此时需要填写合并信息。

~~~shell
$ git merge dev
Merge made by the 'recursive' strategy.
 first.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
~~~

