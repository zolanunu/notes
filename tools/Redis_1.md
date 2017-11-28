###Redis简介
- 开源免费，遵守BSD协议，高性能key-value数据库
- Redis支持数据的持久化，可将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用
- 不仅支持key-value类型的数据，还提供了list,set,zset,hash等数据结构的储存
- 支持数据的备份 master-slave模式的数据备份
###Redis优势
- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子–Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性
- 如果把一个事务可看作是一个程序,它要么完整的被执行,要么完全不执行。这种特性就叫原子性。
- BSD协议？？
###安装Redis
下载地址：https://github.com/MSOpenTech/redis/releases
打开一个cmd窗口使用cd命令切换目录到redis安装目录运行
```
redis-server.exe redis.windows.conf
```
这时候另启一个cmd窗口，原来的不要关闭，不然就无法访问服务端了。
切换到redis目录下运行，这部分是为了测试redis是否安装成功
```
redis-cli.exe -h 127.0.0.1 -p 6379 。
设置键值对 set myKey abc
取出键值对 get myKey
查看是否启动成功：
redis-server.exe redis.cli ping命令 -> PONG
```
说明我们已经成功安装好了redis
###配置redis
###redis启动问题
1. 进入redis根目录，执行命令
暂时没有找到一个好的启动方法，只能按照上面的 启动redis
```
redis-server.exe redis.windows.conf
```
2. 启动服务
http://www.cnblogs.com/M-LittleBird/p/5902850.html
这篇里面启动redis的操作试了一下，但是发现会报错：
错误1067：意外进程，系统找不到文件，启动不了redis服务，但是百度了很多，没有解决问题。说是配置问题，但是查看配置文件并没有出现这个问题
之后查看了redis documentation，发现在install service命令中的问题
```
* installing the service
redis-server --service-install redis.windows-service.conf --loglevel verbose
* start the service
redis-server --service-start
* stop the service
redis-server --service-stop
* unintalling the service
redis-server --service-unstall
```
安装服务中是关于redis-windows-service.conf这个配置文件的，查看这个配置文件，发现
```
logfile "Logs/redis_log.txt"
```
于是在redis根目录新建Logs文件，启动服务成功了
