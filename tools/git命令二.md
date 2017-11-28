---
title: git常用命令二
date: 2017-11-28
tags: [git, github]
categories: [工具]
---

## 写在命令之前

### github如何删除repository

`github`账号下进入待删除的`respository`,进入`setting`,下拉到最下面，删除`respository`,输入`respository`的名字,就ok了

### 本地仓库和远程仓库

本地仓库有什么呢？

`http://www.runoob.com/git/git-workspace-index-repo.html`

###分支管理

```
$ git checkout -b dev # 新建dev分支并进入到dev分支下
Switched to a new branch 'dev'

 # 相当于

git branch dev # 创建dev分支
git checkout dev # 进入到这个分支

git brach  # 查看当前分支 git branch命令会列出所有分支，当前分支前面会标一个*号

 # 此时在dev分支上进行提交 比如

$ git add readme.txt 
$ git commit -m "branch test"
[dev fec145a] branch test
 1 file changed, 1 insertion(+)

 # 切换到master分支

$ git checkout master
Switched to branch 'master'

 # 切换回master分支后，再查看一个readme.txt文件
 # 刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：

 # 合并分支 git merge命令用于合并指定分支到当前分支

$ git merge dev
Updating d17efd8..fec145a
Fast-forward
 readme.txt |    1 +
 1 file changed, 1 insertion(+)

 # 合并完成后，就可以放心地删除dev分支了

$ git branch -d dev
Deleted branch dev (was fec145a).

```
### 标签管理


```
 # 切换到需要打标签的分支

$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'

 # 敲命令git tag <name>就可以打一个新标签 默认标签是打在最新提交的commit

$ git tag v1.0

 # 如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打

 # 方法是找到历史提交的commit id，然后打上就可以

$ git log --pretty=oneline --abbrev-commit
6a5819e merged bug fix 101
cc17032 fix bug 101
7825a50 merge with no-ff
6224937 add merge
59bc1cb conflict fixed
400b400 & simple
75a857c AND simple
fec145a branch test
d17efd8 remove test.txt
...

 # 对旧操作add merge打tag,它对应的commit id 是：6224937

git tag v0.9 6004937

 # 查看标签 标签不是按时间顺序列出，而是按字母排序的

$ git tag
v0.9
v1.0

 # git show <tagname>查看标签信息
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Mon Aug 26 07:28:11 2013 +0800

version 0.1 released

commit 3628164fb26d48395383f8f31179f24e0882e1e0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Tue Aug 20 15:11:49 2013 +0800
append GPL

 # 删除标签 创建的标签都只存储在本地，不会自动推送到远程 打错的标签可以在本地安全删除

git tag -d v0.1
Deleted tag 'v0.1' (was e078af9)

 # 要推送某个标签到远程，使用命令git push origin <tagname>

$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0

 # 推送所有的标签

$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 554 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new tag]         v0.2 -> v0.2
 * [new tag]         v0.9 -> v0.9

 # 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除

$ git tag -d v0.9
Deleted tag 'v0.9' (was 6224937)

 # 然后，从远程删除。删除命令也是push，但是格式如下

$ git push origin :refs/tags/v0.9
To git@github.com:michaelliao/learngit.git
 - [deleted]         v0.9

 # 要看看是否真的从远程库删除了标签，可以登陆GitHub查看。

```
### 版本控制系统