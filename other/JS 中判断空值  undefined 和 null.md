---
title: JS 中判断空值  undefined 和 null
date: 2016-09-18 10:36:18
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/52572288   
  # JS 中如何判断 undefined

 JavaScript 中有两个特殊数据类型：undefined 和 null，下节介绍了 null 的判断，下面谈谈 undefined 的判断。

 
## _以下是不正确的用法：_

 
```
var exp = undefined;
if (exp == undefined)
{
    alert("undefined");
}
```
 exp 为 null 时，也会得到与 undefined 相同的结果，虽然 null 和 undefined 不一样。注意：要同时判断 undefined 和 null 时可使用本法。

 
```
var exp = undefined;
if (typeof(exp) == undefined)
{
    alert("undefined");
}
```
 typeof 返回的是字符串，有六种可能：”number”、”string”、”boolean”、”object”、”function”、”undefined”

 以下是正确的用法：

 
```
var exp = undefined;
if (typeof(exp) == "undefined")
{
    alert("undefined");
}
```
 
--------
 
# JS 中如何判断 null

 
## _以下是不正确的用法：_

 var exp = null;   
 if (exp == null)   
 {   
 alert(“is null”);   
 }

 exp 为 undefined 时，也会得到与 null 相同的结果，虽然 null 和 undefined 不一样。注意：要同时判断 null 和 undefined 时可使用本法。

 var exp = null;   
 if (!exp)   
 {   
 alert(“is null”);   
 }

 如果 exp 为 undefined 或者数字零，也会得到与 null 相同的结果，虽然 null 和二者不一样。注意：要同时判断 null、undefined 和数字零时可使用本法。

 var exp = null;   
 if (typeof(exp) == “null”)   
 {   
 alert(“is null”);   
 }

 为了向下兼容，exp 为 null 时，typeof 总返回 object。

 var exp = null;   
 if (isNull(exp))   
 {   
 alert(“is null”);   
 }

 JavaScript 中没有 isNull 这个函数。

 
## **以下是正确的用法：**

 
--------
 var exp = null;   
 if (!exp && typeof(exp)!=”undefined” && exp!=0)   
 {   
 alert(“is null”);   
 } 

 尽管如此，我们在 DOM 应用中，一般只需要用 (!exp) 来判断就可以了，因为 DOM 应用中，可能返回 null，可能返回 undefined，如果具体判断 null 还是 undefined 会使程序过于复杂。

   
  