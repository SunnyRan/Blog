---
title: 检测到在集成的托管管道模式下不适用的 ASP.NET 设置
date: 2016-01-15 15:56:45
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/50524282   
   在一个项目中，从vs2010切换到2015时候出现了：

 

 **HTTP 错误 500.23 - Internal Server Error**

 **检测到在集成的托管管道模式下不适用的 ASP.NET 设置。**

   
错误原因如下：

 

 在IIS7的应用程序池有两种模式，一种是“集成模式”，一种是“经典模式”。

 经典模式 则是我们以前习惯的IIS 6 的方式。

 如果使用集成模式，那么对自定义的httpModules 和 httpHandlers 就要修改配置文件，需要将他们转移到<modules>和<hanlders>节里去。

 **两种解决方法：**

 
# []()第一种方法、配置应用程序池

 在IIS7上配置应用程序池，并且将程序池的模式改为“经典”，之后一切正常。

 

 

 
# 第二种方法、修改web.config配置文件：

 

 例如原先设置（你的环境中可能没有httpModules，httpHandlers节点）

 ```
<span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">system.web</span>></span> ............ <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">httpModules</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">add</span> <span class="attribute" style="color: rgb(0, 128, 128);">name</span>=<span class="value" style="color: rgb(221, 17, 68);">"MyModule"</span> <span class="attribute" style="color: rgb(0, 128, 128);">type</span>=<span class="value" style="color: rgb(221, 17, 68);">"MyApp.MyModule"</span> /></span> <span class="tag" style="color: rgb(0, 0, 128);"></<span class="title">httpModules</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">httpHandlers</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">add</span> <span class="attribute" style="color: rgb(0, 128, 128);">path</span>=<span class="value" style="color: rgb(221, 17, 68);">"*.myh"</span> <span class="attribute" style="color: rgb(0, 128, 128);">verb</span>=<span class="value" style="color: rgb(221, 17, 68);">"GET"</span> <span class="attribute" style="color: rgb(0, 128, 128);">type</span>=<span class="value" style="color: rgb(221, 17, 68);">"MyApp.MyHandler"</span> /></span> <span class="tag" style="color: rgb(0, 0, 128);"></<span class="title">httpHandlers</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"></<span class="title">system.web</span>></span>
```
  
 在IIS7应用程序池为“集成模式”时，改为：

 ```
<span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">system.web</span>></span> ........... <span class="tag" style="color: rgb(0, 0, 128);"></<span class="title">system.web</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">system.webServer</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">modules</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">add</span> <span class="attribute" style="color: rgb(0, 128, 128);">name</span>=<span class="value" style="color: rgb(221, 17, 68);">"MyModule"</span> <span class="attribute" style="color: rgb(0, 128, 128);">type</span>=<span class="value" style="color: rgb(221, 17, 68);">"MyApp.MyModule"</span> /></span> <span class="tag" style="color: rgb(0, 0, 128);"></<span class="title">modules</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">handlers</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"><<span class="title">add</span> <span class="attribute" style="color: rgb(0, 128, 128);">name</span>=<span class="value" style="color: rgb(221, 17, 68);">"MyHandler"</span> <span class="attribute" style="color: rgb(0, 128, 128);">path</span>=<span class="value" style="color: rgb(221, 17, 68);">"*.myh"</span> <span class="attribute" style="color: rgb(0, 128, 128);">verb</span>=<span class="value" style="color: rgb(221, 17, 68);">"GET"</span> <span class="attribute" style="color: rgb(0, 128, 128);">type</span>=<span class="value" style="color: rgb(221, 17, 68);">"MyApp.MyHandler"</span> <span class="attribute" style="color: rgb(0, 128, 128);">preCondition</span>=<span class="value" style="color: rgb(221, 17, 68);">"integratedMode"</span> /></span> <span class="tag" style="color: rgb(0, 0, 128);"></<span class="title">handlers</span>></span> <span class="tag" style="color: rgb(0, 0, 128);"></<span class="title">system.webServer</span>></span>
```
  
   


 网上找了很多资料，一般都是说把资源池的托管管道模式改成经典模式，但是我这里还是不行

 然后看到在配置文件中加上下面的节点就行了，解决问题！希望能帮大家早点解决问题

 <system.webServer>   
 <validation validateIntegratedModeConfiguration="false"/>   
 </system.webServer>

   


 

   
   
 