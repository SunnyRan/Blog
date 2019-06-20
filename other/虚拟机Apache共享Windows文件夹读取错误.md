---
title: 虚拟机Apache共享Windows文件夹读取错误
date: 2017-09-25 15:57:20
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/78086107   
  ```
apache 中的目录为 windows 共享文件夹时,出现了一种情况:
访问大文件时>100k,只能送出前30k左右的内容,
在 ie中如何刷新都不能显示完整, 在 firefox中刷新几次后可显示完整,用 wget时,可看出明显的续传的过程.
```
 需要关闭以下两项, 具体还是不知为何,但行之有效:

 **EnableMMAP 指令**

 ![这里写图片描述](https://img-blog.csdn.net/20170925155154885?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   
 此指令指示httpd在递送中如果需要读取一个文件的内容，它是否可以使用内存映射。当处理一个需要访问文件中的数据的请求时，比如说当递送一个使用mod_include进行服务器端分析的文件时，如果操作系统支持，Apache将默认使用内存映射。   
 这种内存映射有时会带来性能的提高，但在某些情况下，您可能会需要禁用内存映射以避免一些操作系统的问题：   
 在一些多处理器的系统上，内存映射会减低一些httpd的性能。   
 在挂载了NFS的DocumentRoot上，若已经将一个文件进行了内存映射，则删除或截断这个文件会造成httpd因为分段故障而崩溃。   
 在可能遇到这些问题的服务器配置过程中，您应当使用下面的命令来禁用内存映射：   
 EnableMMAP Off   
 对于挂载了NFS的文件夹，可以单独指定禁用内存映射：

 
```
<Directory "/path-to-nfs-files"> EnableMMAP Off </Directory> 
```
 **EnableSendfile 指令**   
 ![这里写图片描述](https://img-blog.csdn.net/20170925155505826?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   
 仅在 Apache 2.0.44 及以后的版本中可用   
 这个指令控制httpd是否可以使用操作系统内核的sendfile支持来将文件发送到客户端。默认情况下，当处理一个请求并不需要访问文件内部的数据时(比如发送一个静态的文件内容)，如果操作系统支持，Apache将使用sendfile将文件内容直接发送到客户端而并不读取文件。译者注：Linux2.4/2.6内核都支持。   
 这个sendfile机制避免了分开的读和写操作以及缓冲区分配，但是在一些平台或者一些文件系统上，最好禁止这个特性来避免一些问题：   
 一些平台可能会有编译系统检测不到的有缺陷的sendfile支持，特别是将在其他平台上使用交叉编译得到的二进制文件运行于当前对sendfile支持有缺陷的平台时。   
 在Linux上启用IPv6时，使用sendfile将会触发某些网卡上的TCP校验和卸载bug。   
 当Linux运行在Itanium处理器上的时候，sendfile可能无法处理大于2GB的文件。   
 对于一个通过网络挂载了NFS文件系统的DocumentRoot (比如：NFS或SMB)，内核可能无法可靠的通过自己的缓冲区服务于网络文件。   
 如果出现以上情况，你应当禁用sendfile ：   
 EnableSendfile Off   
 针对NFS或SMB，这个指令可以被针对目录的设置覆盖：

 
```
<Directory "/path-to-nfs-files"> EnableSendfile Off </Directory> 
```
   
  