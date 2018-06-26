---
title: git常用命令一
date: 2017-11-21
tags: [git,github]
categories: [工具]
---

### 写在命令之前

#### git安装

git官网下载相应的版本就好。配置环境变量

#### github账号
git之前：需要有一个github，申请github账号，提示：不要勾选organization,如果哪一天你需要删除这个github账号，organization会让删除账号操作麻烦很多。

#### github配置

- cmd命令

```
git config --global user.name zolanunu
git config --global user.email 935539721@qq.com
```

- 查看配置

```
git config --list
```

以上是我的账号信息，你可以输入你自己的。

- 创建本地ssh

```
ssh-keygen -t rsa -C "935539721@qq.com"
```
上面的命令一路回车enter就好，然后在相应的目录下找到id_isa.pub文件，复制里面的内容到github账户中的ssh

- 查看是否生成密钥

```
cd ~/.ssh
```

- 验证是否配置成功

```
ssh -T git@github.com

Hi ***! You've successfully authenticated, but GitHub does not provide shell access.
说明已经成功啦
```

### git常用命令

#### git相关文档和教程

```
http://www.runoob.com/git/git-tutorial.html

https://git-scm.com/docs

https://git-scm.com/book/zh/v2
```
##### 了解git点点

##### git工作流程

一般工作流程如下：

- 克隆 Git 资源作为工作目录。
- 在克隆的资源上添加或修改文件。
- 如果其他人修改了，你可以更新资源。
- 在提交前查看修改。
- 提交修改。
- 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

下面一张图解释：

![](./git-process.png)


##### git工作区 版本库的暂存区和版本库

我们先来理解下Git 工作区、暂存区和版本库概念

- 工作区：就是你在电脑里能看到的目录。
- 暂存区：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库

![](./git-workspace.jpg)

#### git 命令

##### 创建仓库

```
git init: 某个目录下git bash,保证github上有同名的工作空间
git init newrepo: 指定目录下创建仓库
```
##### 跟踪和添加

```
git add . : 对该目录下所有文件进行跟踪.添加到缓存区
git add filename: 对目录下某个文件进行跟踪
git commit -m "初始化项目版本"
```

##### 远程仓库

```
git pull # 抓取远程仓库所有分支更新并合并到本地

git pull --no-ff # 抓取远程仓库所有分支更新并合并到本地，不要快进合并

git fetch origin # 抓取远程仓库更新

git merge origin/master # 将远程主分支合并到本地当前分支

git co --track origin/branch # 跟踪某个远程分支创建相应的本地分支

git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上

git push # push所有分支

git push origin master # 将本地主分支推到远程主分支

git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)

git push origin <local_branch> # 创建远程分支， origin是远程仓库名

git push origin <local_branch>:<remote_branch> # 创建远程分支

git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支
```

##### 远程仓库管理

```
git remote -v #查看远程服务器地址和仓库名称

git remote show origin #查看远程服务器仓库状态

git remote add origin git@ github:robbin/robbin_site.git #添加远程仓库地址

git remote set-url origin git@ github.com:robbin/robbin_site.git 
 #设置远程仓库地址(用于修改远程仓库地址) git remote rm <repository> # 删除远程仓库
```

##### 拷贝项目

```
git clone git://github.com/CosmosHua/locate new
git clone http://github.com/CosmosHua/locate new
git clone http://github.com/CosmosHua/locate.git new
git clone git://github.com/CosmosHua/locate.git new
# new：自定义新建的项目名称
```

没有new，比如：

```
git clone git://github.com/schacon/grit.git
# 执行该命令,在当前目录下创建一个名为grit的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录。
```

##### 查看项目状态

```
git status: 命令用于查看项目的当前状态。

 #例子：
$ touch README
$ touch hello.php
$ ls
README        hello.php
$ git status -s
?? README
?? hello.php

$ git add README hello.php
$ git status -s
A  README #说明已经添加到缓存区中了
A  hello.php
```

##### git bash下修改

```
$ vim README
 #修改后：

$ git status -s
AM README #AM说明添加以后又改动，重新添加
A  hello.php
 #git status 以查看在你上次提交之后是否有修改。 # -s ： 简化说明信息
$ git add .
$ git status -s
A  README
A  hello.php

$ git status #无-s的版本，说明信息详细
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   README
    new file:   hello.php
```

##### git diff

git status 显示你上次提交更新后的更改或者写入缓存的改动， 而 git diff 一行一行地显示这些改动具体是啥。

git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。git diff 主要命令有：

```
git diff  #尚未缓存的改动

git diff --cached #查看已经缓存的改动

git diff HEAD #查看已缓存的与未缓存的所有改动

git diff --stat

git diff <file> # 比较当前文件和暂存区文件差异 git diff

git diff <id1><id2> # 比较两次提交之间的差异

git diff <branch1>..<branch2> # 在两个分支之间比较

git diff --staged # 比较暂存区和版本库差异

git diff --cached # 比较暂存区和版本库差异

git diff --stat # 仅仅比较统计信息
 
```

##### git commit

```
git commit -a #跳过git add

git commit -am "初始化版本"
```

##### git reset

```
git reset HEAD #命令用于取消已缓存的内容
```

##### git rm

```
git rm <file>: #要从已跟踪文件清单中移除，然后提交

git rm <file> -f: #删除之前修改过并且已经放到暂存区域

git rm --cached <file> #件从暂存区域移除，但仍然希望保留在当前工作目录中，仅是从跟踪清单中删除
```

##### git mv

```
git mv README  README.md #git mv移动或者重命名文件，目录，软链接
```

##### git log

```
git log git log <file> #查看该文件每次提交记录

git log -p <file> #查看每次详细修改内容的diff

git log -p -2 #查看最近两次详细修改内容的diff

git log --stat #查看提交统计信息

```