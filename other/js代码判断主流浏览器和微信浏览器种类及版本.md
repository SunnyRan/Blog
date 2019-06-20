---
title: js代码判断主流浏览器和微信浏览器种类及版本
date: 2016-09-12 15:39:34
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/52514102   
  以下是PC端电脑浏览器判断代码

 
```

  //判断当前浏览类型  
  function BrowserType()  
  {  
      var userAgent = navigator.userAgent; //取得浏览器的userAgent字符串  
      var isOpera = userAgent.indexOf("Opera") > -1; //判断是否Opera浏览器  
      var isIE = userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1 && !isOpera; //判断是否IE浏览器  
      var isEdge = userAgent.indexOf("Windows NT 6.1; Trident/7.0;") > -1 && !isIE; //判断是否IE的Edge浏览器  
      var isFF = userAgent.indexOf("Firefox") > -1; //判断是否Firefox浏览器  
      var isSafari = userAgent.indexOf("Safari") > -1 && userAgent.indexOf("Chrome") == -1; //判断是否Safari浏览器  
      var isChrome = userAgent.indexOf("Chrome") > -1 && userAgent.indexOf("Safari") > -1; //判断Chrome浏览器  

      if (isIE)   
      {  
           var reIE = new RegExp("MSIE (\\d+\\.\\d+);");  
           reIE.test(userAgent);  
           var fIEVersion = parseFloat(RegExp["$1"]);  
           if(fIEVersion == 7)  
           { return "IE7";}  
           else if(fIEVersion == 8)  
           { return "IE8";}  
           else if(fIEVersion == 9)  
           { return "IE9";}  
           else if(fIEVersion == 10)  
           { return "IE10";}  
           else if(fIEVersion == 11)  
           { return "IE11";}  
           else  
           { return "0"}//IE版本过低  
       }//isIE end  

       if (isFF) {  return "FF";}  
       if (isOpera) {  return "Opera";}  
       if (isSafari) {  return "Safari";}  
       if (isChrome) { return "Chrome";}  
       if (isEdge) { return "Edge";}  
   }//myBrowser() end  

   //判断是否是IE浏览器  
   function isIE()  
   {  
      var userAgent = navigator.userAgent; //取得浏览器的userAgent字符串  
      var isIE = userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1 && !isOpera; //判断是否IE浏览器  
      if(isIE)  
      {  
          return "1";  
      }  
      else  
      {  
          return "-1";  
      }  
   }  


   //判断是否是IE浏览器，包括Edge浏览器  
   function IEVersion()  
   {  
      var userAgent = navigator.userAgent; //取得浏览器的userAgent字符串  
      var isIE = userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1 && !isOpera; //判断是否IE浏览器  
var isEdge = userAgent.indexOf("Windows NT 6.1; Trident/7.0;") > -1 && !isIE; //判断是否IE的Edge浏览器  
      if(isIE)  
      {  
           var reIE = new RegExp("MSIE (\\d+\\.\\d+);");  
           reIE.test(userAgent);  
           var fIEVersion = parseFloat(RegExp["$1"]);  
           if(fIEVersion == 7)  
           { return "IE7";}  
           else if(fIEVersion == 8)  
           { return "IE8";}  
           else if(fIEVersion == 9)  
           { return "IE9";}  
           else if(fIEVersion == 10)  
           { return "IE10";}  
           else if(fIEVersion == 11)  
           { return "IE11";}  
           else  
           { return "0"}//IE版本过低  
      }  
else if(isEdge)  
{  
    return "Edge";  
}  
      else  
      {  
          return "-1";//非IE  
      }  
   }  

```
 以下是手机微信浏览器判断代码

 
```
function isWeiXin(){
    var ua = window.navigator.userAgent.toLowerCase();
    if(ua.match(/MicroMessenger/i) == 'micromessenger'){
        return true;
    }else{
        return false;
    }
}
```
   
  