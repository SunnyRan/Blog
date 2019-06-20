---
title: 利用URLRewriter重写url地址-实现伪静态
date: 2017-07-10 16:04:30
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/74926535   
  首先，当然是下载URLRewriter

 
```
download.microsoft.com/download/0/4/6/0463611e-a3f9-490d-a08c-877a83b797cf/MSDNURLRewriting.msi  
```
 下载安装后再bin目录下找到URLRewriter.dll文件

  
  * 在web.config文件中加入如下代码  
```
<configuration>
  <configSections>
    <section name="RewriterConfig" type="URLRewriter.Config.RewriterConfigSerializerSectionHandler, URLRewriter" />
  </configSections>

</configuration>
```
 用于指定配置节”RewriterConfig”的处理程序类的名称为   
 ”URLRewriter.Config.RewriterConfigSerializerSectionHandler”   
 该类存在于bin目录下的URLRewriter .dll文件中

  
  * 在web.config文件中的system.web节点下加入如下代码  
```
<httpHandlers>
      <add verb="*" path="*.html"
           type="URLRewriter.RewriterFactoryHandler, URLRewriter" />
   </httpHandlers>
```
 这段代码的意思是：将文件扩展名为 .html 的文件的所有 HTTP 请求映射到类 URLRewriter.RewriterFactoryHandler 具体可以看MSDN。你可以自己根据自己业务修改path中的参数   
 其中verb代表请求方式 比如POST GET

  
  * 重写URL  有两种方法

 1.通过在Web.config中增加节点实现加载新的config文件进行配置

 
```
<RewriterConfig configSource="URLRewriter.config"/>
```
 然后在目录下新建URLRewriter.config文件

 
```
<?xml version="1.0"?>
<RewriterConfig>
  <Rules>
    <RewriterRule>
      <LookFor>~/admin/keyworddetail/(.+)\.aspx</LookFor>
      <SendTo>~/admin/keyworddetail.aspx?kwpkid=$1</SendTo>
    </RewriterRule>
    <RewriterRule>
      <LookFor>~/admin/testsubject/(.+)\.aspx</LookFor>
      <SendTo>~/admin/testsubject.aspx?tstpkid=$1</SendTo>
    </RewriterRule>
  </Rules>
</RewriterConfig>
```
 2.放web.config中的节点下面 

 
```
<RewriterConfig>
  <Rules>
    <RewriterRule>
      <LookFor>~/admin/keyworddetail/(.+)\.aspx</LookFor>
      <SendTo>~/admin/keyworddetail.aspx?kwpkid=$1</SendTo>
    </RewriterRule>
    <RewriterRule>
      <LookFor>~/admin/testsubject/(.+)\.aspx</LookFor>
      <SendTo>~/admin/testsubject.aspx?tstpkid=$1</SendTo>
    </RewriterRule>
  </Rules>
</RewriterConfig>
```
 意思是把第一个路径转成另一个路径。其中（）中的正则表达式就是第二句中的参数$1 .

 同样也可以用23来表示中第二 第三个（）中的参数。

 如果你的页面有回传。比如说放了DATAGRID，有分页的，你点到下一页就发现，晕倒，又出问题了。   
 这下怎么办呢，这个其实微软件的网站上就有说到，我在这里简述一下了。

 加入窗体回传保持的组件：   
 在原来你下载的项目里找到 ActionlessForm.dll 放到你的项目 bin 目录下。   
 然后在你的这个页面中加入：

 
```
<%@ Register TagPrefix="skm" Namespace="ActionlessForm" Assembly="ActionlessForm" %>
```
 再把你的 `<Form...>` 改为：

 
```
<skm:Form id="你的表单名" method="post" runat="server">
.....
</skm:Form>
```
 [msdn官方文档](https://msdn.microsoft.com/zh-cn/library/bya7fh0a%28VS.80%29.aspx%20MSDN%E5%AE%98%E6%96%B9%E6%96%87%E6%A1%A3)   
 以上

   
  