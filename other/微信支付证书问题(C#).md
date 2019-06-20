---
title: 微信支付证书问题(C#)
date: 2017-09-15 14:24:30
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/77991201   
  Aps.net在IIS服务器中使用windows的容器中的证书访问https服务（在windows服务和COM+服务中有同样的这个问题）

 **问题描述：**

 在使用aps.net开发web应用的时候，我们需要使用证书去访问https的接口，我们在开发的时候可运行，但是部署到iis服务器上之后，便发现所有使用证书访问的地方都抛异常？返回的错误大致为：1、webrequest.GetResponse()函数抛异常；2、System.Net.WebException:请求被中止，未能创建   
 SSL/TLS 安全通道；3、store.Certificates.Find 函数查找出来的证书个数为0；

 **问题分析：**

 开发环境中直接使用vs调试工具可以正常运行，不是之后便运行不正常，在排查iis配置问题的情况下，基本上可以判断是权限问题，在开发工作下运行，使用的是当前用户的权限，安装在本机的证书基本也是该用户下安装的，权限肯定没问题，但是在iis服务器下面，则不是以当前用户的环境运行，当然没权限读取证书。

 Windows中证书存放分析：

 Windows存放证书有2种，一种本地计算机证书存取路径，另一是本地用户账户证书存取路径，存放的路径分别为：LOCAL_MACHINE\MY   
 和 CURRENT_USER\My（My为windows下默认路径）

 1、本地用户账户证书存取路径表示一般用户安装的证书存放地，用户开启的进程时，加载证书都从这里个路径进行加载；但是服务进程则不能从该路径加载证书。

 2、本地计算机证书存取路径表示服务类型的进程使用证书的时候，默认从该路径加载，用户安装的证书并不会同步到该路径。

 IIS一般都是以系统内置的一些账户启动，这些账户是没有权限访问CURRENT_USER目录下的证书的，所以我们需要将证书安装到LOCAL_MACHINE下去。

 **操作方法：**

 1、打开mmc（在搜索程序中输入mmc，然后回车）进入如下界面：

 ![这里写图片描述](https://img-blog.csdn.net/20170915141051927?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 2、选择“文件 》 添加/删除管理单元”进入如下界面

 ![这里写图片描述](https://img-blog.csdn.net/20170915141133016?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 3、左边选择证书，点击“添加”，出现如下界面

 ![这里写图片描述](https://img-blog.csdn.net/20170915141200008?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 4、选择计算机账户，点击下一步

 ![这里写图片描述](https://img-blog.csdn.net/20170915141307833?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 5、这里选择“本地计算机”（默认选择），点击完成后，再点“确定”，界面成下面样

 6、右“键个人 》 证书”，出现导入证书界面

 8、安装步骤导入pfx或者.p12格式的证书，导完之后右边出现刚刚导入的证书

 ![](media/22606aca8b3be83f5dd4e6ac937d44c7.png)

 到这里证书导入算是完成了。

 证书导完之后，试下自己的网站是否能够发起https请求，如果能够，则到此为止，如果不能，还需要将证书权限给指定的系统用户。方法如下：

 1、去微软官网，（地址：[http://www.microsoft.com/en-us/download/confirmation.aspx?id=19801）下载WinHttpCertCfg.exe](http://www.microsoft.com/en-us/download/confirmation.aspx?id=19801）下载WinHttpCertCfg.exe)证书配置程序，然后安装。

 2、在64位系统下，安装之后工具在C:\Program Files (x86)\Windows Resource   
 Kits\Tools路径下，使用CD \D指令定位到该目录下

 3、使用刚刚安装的工具，给指定用户开证书的访问权限，使用指令如下：

 winhttpcertcfg.exe -g -c LOCAL_MACHINE\MY -s “jackliu_test_company” -a   
 “NETWORKSERVICE”

 这里参数可以使用–help具体查看，我只描述几个最简单的

 -g 表示给该用户增加证书的访问权限，如果将该参数变成-r表示移除用户的证书访问权限

 -c   
 后面参数表示证书的路径，LOCAL_MACHINE可以替换为CURRENT_USER，但是必须如果使用CURRENT_USER下的证书，即使给了权限还是不能正常运行。

 -s   
 后面参数表示证书的名称。注意：证书名称不是证书的文件名称，名称可以从mmc管理工具中证书列表的“颁发给”字段。

 ![](media/bc499ebbf7053638ac69d9a7f431ed66.png)

 -a 后面参数表示用户名称，一般IIS运行用户为：NETWORKSERVICE 或者   
 ASPNET用户，这里如果开了这两个用户还是不行的话，则需要给windows认证的一个用户也开下证书访问权限。authenticated   
 Users

 指令如下：

 winhttpcertcfg.exe -g -c LOCAL_MACHINE\MY -s “你的证书名称” -a   
 “NETWORKSERVICE”

 winhttpcertcfg.exe -g -c LOCAL_MACHINE\MY -s “你的证书名称” -a “ASPNET”

 winhttpcertcfg.exe -g -c LOCAL_MACHINE\MY -s “你的证书名称” -a “Authenticated   
 Users”

 成功则出现如下字样：

 ![](media/dc8b05dd6ac05884344c12446d9b68d9.png)

 如果出现如下错误信息：

 ![](media/87233c2284408b7095c1e8bcd92ce57c.png)

 则表示未找到证书，请安装我上面讲的如何安装证书到LOCAL_NARCHANT下的说明。

 如果出现如下错误信息：

 ![](media/c70e65620e5169ecb8d3fb3287d2241f.png)

 表示证书已经找到，但是没找到用户信息

 **C#代码使用方法（直接使用windows下已经安装的证书）：**

 HttpWebResponse webreponse;

 try

 {

 //系统必须已经导入cert指向的证书

 string url = “[https://api.mch.weixin.qq.com/secapi/pay/refund](https://api.mch.weixin.qq.com/secapi/pay/refund)“;

 X509Store store = new X509Store(“My”, StoreLocation.LocalMachine);

 store.Open(OpenFlags.ReadOnly | OpenFlags.OpenExistingOnly);

 System.Security.Cryptography.X509Certificates.X509Certificate2 cert =

 store.Certificates.Find(X509FindType.FindBySubjectName, “你的证书名称”,   
 false)[0];

 HttpWebRequest webrequest = (HttpWebRequest)HttpWebRequest.Create(url);

 webrequest.ClientCertificates.Add(cert);

 webrequest.Method = “post”;

 webrequest.KeepAlive = true;

 webreponse = (HttpWebResponse)webrequest.GetResponse();

 Stream stream = webreponse.GetResponseStream();

 string resp = string.Empty;

 using (StreamReader reader = new StreamReader(stream))

 {

 resp = reader.ReadToEnd();

 }

 strHtml = resp;

 }

 catch (Exception exp)

 {

 strHtml = exp.ToString();

 }

 几个关键点：

 1、X509Store store = new X509Store(“My”, StoreLocation.LocalMachine);

 这里，My为证书安装的模块逻辑，windows7下默认为My，StoreLocation.LocalMachine使用这个类型。

 2、System.Security.Cryptography.X509Certificates.X509Certificate2 cert =

 store.Certificates.Find(X509FindType.FindBySubjectName, “你的证书名称”,   
 false)[0];

 这里通过证书名称查找证书，证书名称为证书列表中“颁发给字段”。这里返回是一个数组，如果你安装多个相同名称的证书，这里可能会返回多个，这种情况需要根据证书具体定位是那个。

   
  