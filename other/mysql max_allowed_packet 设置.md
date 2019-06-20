---
title: mysql max_allowed_packet 设置
date: 2017-08-15 14:42:31
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/77189637   
  mysql根据配置文件会限制server接受的数据包大小。

 有时候大的插入和更新会受max_allowed_packet 参数限制，导致写入或者更新失败。

 查看目前配置

 
```
show VARIABLES like '%max_allowed_packet%';
```
 显示的结果为：

 
```
+--------------------+---------+

| Variable_name      | Value   |

+--------------------+---------+

| max_allowed_packet | 1048576 |

+--------------------+---------+  
```
 以上说明目前的配置是：1M

 修改方法

 1、修改配置文件

 可以编辑my.cnf来修改（windows下my.ini）,在[mysqld]段或者mysql的server配置段进行修改。(高版本的MySQL没有配置文件,需要复制my-default.ini重命名为my.ini后进行配置)

 
```
max_allowed_packet = 20M
```
 如果找不到my.cnf可以通过

 
```
mysql --help | grep my.cnf
```
 去寻找my.cnf文件。

 linux下该文件在/etc/下。   
 重启mysql服务，再进入。

 2、在mysql命令行中修改(此方法不需要重启Mysql，因为重启后Mysql会根据my.ini初始化，导致通过命令设置的值失效)

 在mysql 命令行中运行

 
```
set global max_allowed_packet = 2*1024*1024*10
```
 查看下max_allowed_packet是否编辑成功

 
```
show VARIABLES like '%max_allowed_packet%';
```
 注意：该值设置过小将导致单个记录超过限制后写入数据库失败，且后续记录写入也将失败。

   
  