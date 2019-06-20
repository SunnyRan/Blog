---
title: iis发布后出现 .woff 404 (Not Found)的问题
date: 2016-05-17 15:51:30
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/51436728   
  网站发布到IIS后，发现网站使用的Bootstrap框架所引用的woff字体无法正常显示。于是跟踪http请求，发现woff字体请求出现GET .woff 404 (Not Found)的问题，但是项目中woff字体的文件并未丢失。后经排查，原来是服务器上IIS没有添加woff字体的MIME类型，导致发送HTTP请求时，IIS无法处理和识别此类型的文件。

 解决方法一：在Web.config配置文件中添加woff字体的MIME类型

 如果网站是使用ASP.NET 或者ASP.NET MVC 编写的，可以很方便的直接使用配置文件进行woff字体的配置。只要在Web.config中的system.webServer节点添加下面的配置可以了。

 
```
  <system.webServer>    
    <staticContent>
      <remove fileExtension=".woff" />
      <mimeMap fileExtension=".woff" mimeType="font/x-font-woff" />      
    </staticContent>
  </system.webServer>
```
 这里要注意下的是这个配置，添加此节点是防止出现这个错误：“在唯一密钥属性“fileExtension”设置为“.woff”时，无法添加类型为“mimeMap”的重复集合项”，这个问题可以点击此链接查看解决方法。如果只添加下面的 这个节点，而且没有报这个错误的话，remove节点可以不用添加。另外”font/x-font-woff”是woff字体的MIME类型值。

 将该节点添加到网站的配置文件后，在重新打开网站即可正常显示woff字体。此方法可用于没有权限操作IIS管理器的时候作为解决方案。

 解放方案二：在IIS中添加woff字体的MIME类型

 如果可以直接操作IIS管理器的话，也可以直接在IIS上添加woff字体的mime type。只要打开当前的IIS，打开MIME类型的配置，最后添加一个新的MIME类型就可以了，这里woff字体的扩展名是.woff, MIME类型为：”font/x-font-woff“。具体操作如下图所示：

 1.打开控制面板中的IIS管理器，选择当前IIS，打开MIME类型配置   
 2.点击MIME类型右边操作的栏的添加功能   
 3.弹出的添加MIME类型对话框中，文件扩展名填写.woff，MIME类型可填写 font/x-font-woff 或者application/x-font-woff   
 4.点击确定后成功添加了.woff扩展名的MIMI TYPE，现在打开网站请求WOFF字体就不会出现404 NOT FOUND错误了。

   
  