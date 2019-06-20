---
title: JenKins+GitLab+.Net 持续化集成实践
date: 2017-06-08 16:02:23
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/72918073   
  ## **JenKins安装**

 操作系统Windows, 确保需要的.NET Framework已经安装   
 从 [http://jenkins-ci.org/](http://jenkins-ci.org/)下载Windows安装包。   
 安装后,访问[http://localhost:8080](http://localhost:8080) . 

 如果端口被占用，可以在根目录下的jenkins.xml里面进行修改

 
```
  <executable>%BASE%\jre\bin\java</executable>
  <arguments>-Xrs -Xmx256m -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "%BASE%\jenkins.war" --httpPort=8080 --webroot="%BASE%\war"</arguments>

```
 按照步骤完成安装和注册后，下面要安装所需要的插件。   
 安装目录在 **首页-系统管理-管理插件-可选插件**   
 ![这里写图片描述](https://img-blog.csdn.net/20170608141014921?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 **MSBuild Plugin —— .Net构建工具**   
 安装完后， 切换到 Jenkins => Manager Jenkins =>Global Tool Configuration => MSBuild

 ![这里写图片描述](https://img-blog.csdn.net/20170608141505417?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   
 这里用的是.NetFramWork4.0 所以名称命名为Ver4.0   
 构建路径填写（请根据自己本机实际情况进行填写）:C:\Windows\Microsoft.NET\Framework64\v4.0.30319\MSbuild.exe

 **Git Plugin & GIT Client Plugin—–Git插件**   
 让Jenkins可以自动build git repo中的代码   
 插件安装完毕后，我们需要在jenkins中配置Git.exe的位置。   
 ![这里写图片描述](https://img-blog.csdn.net/20170608142157131?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 **Gitlab Hook Plugin**   
 让Jenkins可以收到Gitlab发来的hook从而自动build   
 安装时可能遇到一些错误：   
 方案：采用手动上传插件安装方式，下载地址   
 [下载地址](http://updates.jenkins-ci.org/download/plugins/ruby-runtime/)

 
## **创建Job**

 点击 New Job, 输入任务名称   
 ![这里写图片描述](https://img-blog.csdn.net/20170608142438663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   
 源码管理菜单内选择你所使用的源码管理工具，我这里是已Git为例。如果是SVN需要安装subversion插件   
 ![这里写图片描述](https://img-blog.csdn.net/20170608142812731?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   
 填写好Git路径后，增加自己的身份验证数据，可以使用账户密码或者SSH   
 ![这里写图片描述](https://img-blog.csdn.net/20170608142723886?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 
## **构建**

 MSBuilder Version 为之前配置的 “Ver 4.0”   
 MSBuild Build File 是项目文件或者工程文件的名称   
 然后就是MSBuild的命令行参数了。   
 /t:Rebuild 表示每次都重建，不使用增量编译   
 /property:Configuration=Release 表示编译Release版本，   
 /property:TargetFrameworkVersion=v4.5表示编译的目标是.NET 4.5   
 保存后，点击左侧Build Now开始测试一次编译。   
 ![这里写图片描述](https://img-blog.csdn.net/20170608144313251?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 如果编译过程中出现错误，需查看Console Output.   
 一种常见的错误情况是:编译的机器上没有安装Visual Studio, 在编译的过程中可能会引发MSB4019错误

 
```
.error MSB4019: The imported project "C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v11.0\WebApplications\Microsoft.WebApplication.targets" was not found. Confirm that the path in the <Import> declaration is correct, and that the file exists on disk. 
```
 这种情况，可以将开发机上的C:\Program Files (x86)\MSBuild文件夹之间拷贝到编译机上。   
 如果成功，则显示 0 Error(s)，在编译成功后可以启动单元测试，如果有NUnit的话. 

 如果出现首次编译失败，可以尝试用VS打开项目文件，手动编译发布一次。

 
## **部署**

 部署的话，可以通过批处理完成, 首先安装 Post build task插件， 与之前MSBuild插件的安装方式一样   
 然后在Job的配置中，添加post build task   
 ![这里写图片描述](https://img-blog.csdn.net/20170608144918410?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   
 在Log Text那，可以使用正则表达式检测0 Error(s)出现了， 如\b0\s+(Errors)   
 Script中直接调用磁盘上的批处理文件

 
## **GitLabHook触发构建**

  
  2. 查看jenkin生成回调地址。在任务重构建触发器下获取回调URL。下面的URL那一行只有Gitlab Hook Plugin插件下载成功后才能显示。  
## 补充1.如何发布VS2010的Web站点

 如果是发布Web站点，可以直接指定需要发布站点的csproj文件,如

 使用如下参数

 
```
/t:ResolveReferences;Compile /t:_CopyWebApplication /p:Configuration=Release /p:WebProjectOutputDir=C:\Jenkins_Publish /p:OutputPath=C:\Jenkins_Publish\bin  
```
 其中WebProjectOutputDir是web站点的发布路径；OutputPath是编译输出的dll路径

   
  