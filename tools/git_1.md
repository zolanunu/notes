## 使用git遇到的问题

### 上传到github出现上传不上去的问题

百度解决的

此时很多人会尝试下面的命令把当前分支代码上传到master分支上。

$ git push -u origin master

但依然没能解决问题

出现错误的主要原因是github中的README.md文件不在本地代码目录中（本地和GitHub上文件出现不一致的情况）

可以通过如下命令进行代码合并【注：pull=fetch+merge]

```
git pull --rebase origin master
```

如此就ok了