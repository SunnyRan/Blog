---
title: Json.net之JsonConvert
date: 2016-06-12 15:12:41
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/51646457   
  首先在使用Json.net时候需要先引用Newtonsoft.Json.Net35.dll   
 下载地址如下：[http://json.codeplex.com/releases/view/64935](http://json.codeplex.com/releases/view/64935)

 引用方法：

 
```
1.用Visual Studio打开网站
2.解决方案资源管理器中右键网站
3.浏览找到下载的dll
4.确定
5.在需要用的类中添加using Newtonsoft.Json;
```
 在ajax的异步请求中   
 常常返回json对象   
 可以利用json.net给我们提供的api达到快速开发。 

 B.cs

 
```
    public class B 
    { 
       public B(){} 
      private int money = 0; 
     private string name = string.Empty; 
     public int Money 
     { 
       get { return money; } 
       set { money = value; } 
      } 
      public string Name 
     { 
     get { return name; } 
     set { name = value; } 
    } 
    } 
```
 A.cs：

 
```
 public class A   { 
      public A(){} 
      public int age { get; set; } 
    public string name { get; set; } 
    B b = null; 

   public B B 
      { 
     get { return b; } 
     set { b = value; } 
     } 
    } 

```
 测试代码如下：

 
```
using Newtonsoft.Json; 

protected void Page_Load(object sender, EventArgs e) 
{ 
A a = new A(); 
a.age = 11; 
a.name = "Name"; 
B b = new B(); 
b.Money = 10000; 
a.B = b; 
string str= JsonConvert.SerializeObject(a); //序列化对象
Response.Write(str); 
} 
```
 输出：{“age”:11,”name”:”Name”,”B”:{“Money”:10000,”Name”:”“}}

   
  