---
title: Windows下安装Redis及可视化工具
date: 2017-03-31 13:45:55
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/68924253   
  Redis对于Linux是官方支持的,安装和使用没有什么好说的,普通使用按照官方指导，5分钟以内就能搞定。详情请参考:

 [http://redis.io/download](http://redis.io/download) 

 Redis官方是不支持windows的，只是 Microsoft Open Tech group 在 GitHub上开发了一个Win64的版本,项目地址是：

 [https://github.com/MSOpenTech/redis/releases](https://github.com/MSOpenTech/redis/releases)

 下载解压，没什么好说的，在解压后的bin目录下有以下这些文件：

 [plain] view plain copy 在CODE上查看代码片派生到我的代码片   
 redis-benchmark.exe #基准测试   
 redis-check-aof.exe # aof   
 redis-check-dump.exe # dump   
 redis-cli.exe # 客户端   
 redis-server.exe # 服务器   
 redis.windows.conf # 配置文件   
 当然，还有一个 RedisService.docx 文件，是一些启动和安装服务的说明文档。

 【需要用Administrator用户运行，如果不是管理员账户就会出各种问题，服务安装以后启动不了等等问题，应该可以修改服务的属性–>登录用户等选项来修正.】   
 打开 redis-server.exe # 服务器 即可开启服务。 

 如果需要把redis增加为windows的服务，脚本如下:

 1，在redis的目录下执行（执行后就作为windows服务了）

 
```
redis-server --service-install redis.windows.conf
```
 2，安装好后需要手动启动redis

 
```
redis-server --service-start
```
 3，停止服务

 
```
redis-server --service-start
```
 4，卸载redis服务

 
```
redis-server --service-uninstall
```
 可以使用自带的客户端工具进行测试。   
 双击打开 redis-cli.exe , 如果不报错,则连接上了本地服务器,然后测试，比如 set命令，get命令:   
 ![这里写图片描述](https://img-blog.csdn.net/20170331134142451?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 
## **最后推荐一款可视化的Redis工具**

 
### **Redis Desktop Manager**

 一款基于Qt5的跨平台Redis桌面管理软件   
 支持: Windows 7+, Mac OS X 10.10+, Ubuntu 14+   
 特点： C++ 编写，响应迅速，性能好。但不支持数据库备份与恢复。

 ![这里写图片描述](https://img-blog.csdn.net/20170331134532787?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 项目地址： [https://github.com/uglide/RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)   
 [https://github.com/uglide/RedisDesktopManager/releases](https://github.com/uglide/RedisDesktopManager/releases)

   
  