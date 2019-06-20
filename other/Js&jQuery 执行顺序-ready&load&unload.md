---
title: Js&jQuery 执行顺序-ready&load&unload
date: 2016-12-21 17:02:59
tags: CSDN迁移
---
  ## Js执行顺序

 1.函数的声明和调用   
 JavaScript是一种描述型脚本语言，由浏览器进行动态的解析与执行。函数的定义方式大体有以下两种，浏览器对于不同的方式有不同的解析顺序。   
 代码如下: 

 
```
//“定义式”函数定义 
function Fn1(){ 
alert("Hello World!"); 
} 
//“赋值式”函数定义 
var Fn2 = function(){ 
alert("Hello wild!"); 
} 
```
 2.页面加载过程中，浏览器会对页面上或载入的每个js代码块(或文件)进行扫描，如果遇到定义式函数，则进行预处理(类似于C等的编译)，处理完成之后再开始由上至下执行；遇到赋值式函数，则只是将函数赋给一个变量，不进行预处理(类似1中变量必须先定义后引用的原则)，待调用到的时候才进行处理。下面举个简单的例子：   
 代码如下: 

 
```
//“定义式”函数定义 
Fn1(); 
function Fn1(){ 
alert("Hello World!"); 
} 
```
 正常执行，弹出“Hello World!”，浏览器对Fn1进行了预处理，再从Fn1();开始执行。   
 代码如下: 

 
```
//“赋值式”函数定义 
Fn2(); 
var Fn2 = function(){ 
alert("Hello wild!"); 
} 

Firebug报错：Fn2 is not a function，浏览器未对Fn2进行预处理，依序执行，所以报错Fn2未定义。
```
 3.代码块及js文件的处理   
 “代码块”是指一对标签包裹着的js代码，文件就是指文件啦，废话:D   
 浏览器对每个块或文件进行独立的扫描，然后对全局的代码进行顺序执行(2中讲到了)。所以，在一个块(文件)中，函数可以在调用之后进行“定义式”定义；但在两个块中，定义函数所在的块必须在函数被调用的块之前。   
 很绕口，看例子好了：   
 代码如下: 

 
```
<script type="text/javascript"> 
Fn(); 
</script> 
<script type="text/javascript"> 
function Fn(){ 
alert("Hello World!"); 
} 
</script> 
// 报错：Fn is notdefined，两个块换过来就对了 
```
 4.重复定义函数会覆盖前面的定义   
 这和变量的重复定义是一样的，代码：   
 代码如下: 

 
```
function fn(){ 
alert(1); 
} 
function fn(){ 
alert(2); 
} 
fn(); 
// 弹出：“2” 
```
 如果是这样呢：   
 代码如下: 

 
```
fn(); 
function fn(){ 
alert(1); 
} 
function fn(){ 
alert(2); 
} 
// 还是弹出：“2” 
```
 还是弹出“2”，为什么？2都讲了好吧… 

 5.body的onload函数与body内部函数的执行   
 body内部的函数会先于onload的函数执行，测试代码：   
 代码如下: 

 
```
//html head... 
<script type="text/javascript"> 
function fnOnLoad(){ 
alert("I am outside the Wall!"); 
} 
</script> 
<body onload="fnOnLoad();"> 
<script type="text/javascript"> 
alert("I am inside the Wall.."); 
</script> 
</body> 
//先弹出“I am inside the Wall..”; 
//后弹出“I am outside the Wall!”
```
 body的onload事件触发条件是body内容加载完成，而body中的js代码会在这一事件触发之前运行(为什么呢?6告诉你..) 

 6.JavaScript是多线程or单线程？   
 严格来说，JavaScript是没有多线程概念的，所有的程序都是“单线程”依次执行的。   
 举个不太恰当的例子：   
 代码如下: 

 
```
function fn1(){ 
var sum = 0; 
for(var ind=0; ind<1000; ind++) { 
sum += ind; 
} 
alert("答案是"+sum); 
} 
function fn2(){ 
alert("早知道了，我就是不说"); 
} 
fn1(); 
fn2(); 
//先弹出：“答案是499500”， 
//后弹出：“早知道了，我就是不说” 
```
 那你肯定要问：那延时执行、Ajax异步加载，不是多线程的吗？没错，下面这样的程序确实看起来像“多线程”：   
 代码如下: 

 
```
function fn1(){ 
setTimeout(function(){ 
alert("我先调用") 
},1000); 
} 
function fn2(){ 
alert("我后调用"); 
} 
fn1(); 
fn2(); 
// 先弹出：“我后调用”， 
// 1秒后弹出：“我先调用”
```
 看上去，fn2()和延时程序是分两个过程再走，但其实，这是JavaScript中的“回调”机制在起作用，类似于操作系统中的“中断和响应” —— 延时程序设置一个“中断”，然后执行fn2()，待1000毫秒时间到后，再回调执行fn1()。   
 同样，5中body的onload事件调用的函数，也是利用了回调机制——body加载完成之后，回调执行fnOnLoad()函数。   
 Ajax请求中的数据处理函数也是一样的道理。   
 关于JavaScript线程问题的更深入讨论，看这篇 javascript中的线程之我见，以及infoQ上的 JavaScript多线程编程简介。   
 困了，再说一下回调函数吧。 

 7.回调函数   
 回调函数是干嘛用的？就是回调执行的函数嘛，又废话:D   
 如6所说，最常见的回调就是onclick、onmouseo教程ver、onmousedown、onload等等浏览器事件的调用函数；还有Ajax异步请求数据的处理函数；setTimeOut延时执行、setInterval循环执行的函数等。   
 干脆我们写一个纯粹的回调函数玩：   
 代码如下: 

 
```
function onBack(num){ 
alert("姗姗我来迟了"); 
// 执行num个耳光 
} 
function dating(hours, callBack){ 
var SP= 0; // SP,愤怒值 
//女猪脚在雪里站了hours个钟头 
//循环开始.. 
SP ++; 
//循环结束... 
callBack(SP); 
} 
dating(1, onBack); 
```
 
## 多个$(document).ready()的执行顺序实例分析

 下面说明多个(document).ready()的执行顺序问题，由实例可以看出多个(document).ready()的执行顺序并非单纯的顺序执行，其与嵌套层级也有一定的关系。具体实例代码如下：

 
```
<html>
<head>
<script src="./jquery-1.9.0.min.js"></script>
<script type="text/javascript">
  $(function(){
    alert('1');
    $(function(){
      alert('2');
      $(function(){
        alert('3');
      });
    });


  });
</script>
<body>
TTTTTTTTTTTT
<script type="text/javascript">
  $(document).ready(function() {
    alert('4');
    $(function(){
      alert('5');
    });


  });
</script>
KKKKKKKKKKKK
<script type="text/javascript">
  $(function(){
    alert('6');
    $(document).ready(function() {
      alert('7');
    });


  });
</script>
</body>
</html>
```
 运行alert显示顺序为：1，4，6，2，5，7，3   
 读者可以自己测试体验一下，以加深对多个$(document).ready()执行顺序的理解。

 
## load(); ready();的执行顺序

 $(document).load();

 当web页面以及其附带的资源文件，如CSS，Scripts，图片等，加载完毕后执行此方法。   
 常用于检测页面（及其附带资源）是否加载完毕。

 $(document).ready();   
 当页面DOM对象加载完毕，web浏览器能够运行JS时，此方法即被触发。如果你想尽快执行JS，可以使用此方法。[在html的头部的script标签中的，不处于ready()中的JS代码将早于ready()执行]

 $(document).unload();   
 此事件在停止浏览页面的时候触发，此操作可能存在于刷新操作/F5，单击上一页按钮，进入其他页面或关闭整个tab或窗口。

 总而言之，他们的调用顺序是 ready() >> load() >> unload() 。

   
  