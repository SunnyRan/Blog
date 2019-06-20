---
title: 微信分享自定义内容开发-微信JSSDK
date: 2016-06-17 16:24:03
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/51700882   
  之前工作需要开发一个关于H5微信分享的页面，后面需求是要求分享的内容标题。   
 首先你要有个公众帐号，如果没有的话，可以申请一个公众测试帐号地址如下：   
 [http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login](http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login)   
 ![这里写图片描述](https://img-blog.csdn.net/20160617155217384)

 这里可以获取到AppID appsecret 等重要属性，并且后期设置的安全域名也是需要在这里设置的。

 =========================================================   
 步骤一：绑定域名

 先登录微信公众平台进入“公众号设置”的“功能设置”里填写“JS接口安全域名”。

 =========================================================

 步骤二：引入JS文件

 在需要调用JS接口的页面引入如下JS文件，（支持https）：[http://res.wx.qq.com/open/js/jweixin-1.0.0.js](http://res.wx.qq.com/open/js/jweixin-1.0.0.js)

 请注意，如果你的页面启用了https，务必引入 [https://res.wx.qq.com/open/js/jweixin-1.0.0.js](https://res.wx.qq.com/open/js/jweixin-1.0.0.js) ，否则将无法在iOS9.0以上系统中成功使用JSSDK

 =========================================================

 步骤三：通过config接口注入权限验证配置

 所有需要使用JS-SDK的页面必须先注入配置信息，否则将无法调用

 
```
wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '', // 必填，公众号的唯一标识
    timestamp: , // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名，见附录1
    jsApiList: [] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
});
```
 <<<验证服务器地址的有效性>>>>

 开发者提交信息后，微信服务器将发送GET请求到填写的服务器地址URL上，GET请求携带四个参数：

 
```
signature   微信加密签名，signature结合了开发者填写的token参数和请求中的timestamp参数、nonce参数。
timestamp   时间戳
nonce       随机数
echostr     随机字符串
```
 开发者通过检验signature对请求进行校验（下面有校验方式）。若确认此次GET请求来自微信服务器，请原样返回echostr参数内容，则接入生效，成为开发者成功，否则接入失败。

 这里首先要获取Token值   
 access_token是公众号的全局唯一票据，公众号调用各接口时都需使用access_token。开发者需要进行妥善保存。access_token的存储至少要保留512个字符空间。access_token的有效期目前为2个小时，需定时刷新，重复获取将导致上次获取的access_token失效。

 AppID和AppSecret调用本接口来获取access_token。AppID和AppSecret可在微信公众平台官网-开发者中心页中获得（需要已经成为开发者，且帐号没有异常状态）。注意调用所有微信接口时均需使用https协议。

 接口调用请求说明

 http请求方式: GET   
 [https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET](https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&amp;appid=APPID&amp;secret=APPSECRET)   
 参数说明

 参数 是否必须 说明

 
```
grant_type      获取access_token填写client_credential
appid       第三方用户唯一凭证
secret      第三方用户唯一凭证密钥，即appsecret
```
 返回说明

 正常情况下，微信会返回下述JSON数据包给公众号：

 
```
{"access_token":"ACCESS_TOKEN","expires_in":7200}
```
 根据获得到的Token值 去进行加密签名获取jsapi_ticket

 <<<<<<<<

 
```
{
"errcode":0,
"errmsg":"ok",
"ticket":"bxLdikRXVbTPdHSM05e5u5sUoXNKd8-41ZO3MhKoyN5OfkWITDGgnr2fwJ0m9E8NYzWKVZvdVtaUgWvsdshFKA",
"expires_in":7200
}
```
 获得jsapi_ticket之后，就可以生成JS-SDK权限验证的签名了。

 签名算法

 签名生成规则如下：参与签名的字段包括noncestr（随机字符串）, 有效的jsapi_ticket, timestamp（时间戳）, url（当前网页的URL，不包含#及其后面部分） 。对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串string1。这里需要注意的是所有参数名均为小写字符。对string1作sha1加密，字段名和字段值都采用原始值，不进行URL 转义。

 即signature=sha1(string1)。 示例：

 
```
noncestr=Wm3WZYTPz0wzccnW
jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg
timestamp=1414587457
url=http://mp.weixin.qq.com?params=value
```
 步骤1. 对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串string1：

 
```
jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg&noncestr=Wm3WZYTPz0wzccnW&timestamp=1414587457&url=http://mp.weixin.qq.com?params=value
```
 步骤2. 对string1进行sha1签名，得到signature：

 0f9de62fce790f9a083d5c99e95740ceb90c27ed

 =========================================================   
 步骤四：通过ready接口处理成功验证

 
```
wx.ready(function(){

    // config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。
});
```
 =========================================================

 =========================================================

 =========================================================

 =========================================================

 ========================以下为代码==========================

 首先看下项目的文件目录   
 ![这里写图片描述](https://img-blog.csdn.net/20160617161259490)

 web.config

 
```
<?xml version="1.0" encoding="utf-8"?>

<!--
  有关如何配置 ASP.NET 应用程序的详细信息，请访问
  http://go.microsoft.com/fwlink/?LinkId=169433
  -->

<configuration>

    <system.web>
      <compilation debug="true" targetFramework="4.5.2" />
      <httpRuntime targetFramework="4.5.2" />
    </system.web>
  <appSettings>
  <!--填写AppId与Secret-->
    <add key="appid" value="wxafc42346aa01b"/>
    <add key="secret" value="27b1123213d4159a59d0b9500f8bda"/>
    <add key="WeChatJsDebug" value="true"/>
  </appSettings>

  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="Access-Control-Allow-Methods" value="OPTIONS,POST,GET"/>
        <add name="Access-Control-Allow-Headers" value="x-requested-with,content-type"/>
        <add name="Access-Control-Allow-Origin" value="*" />
      </customHeaders>
    </httpProtocol>
  </system.webServer>

</configuration>


```
 shareAjax.cs

 
```
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Text;
using System.Web;
using System.Web.Script.Services;
using System.Web.Security;
using System.Web.Services;


public partial class shareAjax : System.Web.UI.Page
{

    [WebMethod(Description = "微信分享请求参数接口")]
    [ScriptMethod(UseHttpGet = false)]
    public void GetWXSharedParam(string url)
    {
        string timestamp = WxSharedClass.ConvertDateTimeInt(DateTime.Now).ToString();
        string nonceStr = Guid.NewGuid().ToString();
        string ticket = string.Empty;
        string appId = ConfigurationManager.AppSettings["appid"];

        //获取jsapi_ticket
        if (HttpRuntime.Cache["JsApiTicket"] == null)
        {
            WxSharedClass.GetJsApiTicket();
        }
        ticket = HttpRuntime.Cache["JsApiTicket"] as string;
        if (string.IsNullOrEmpty(ticket))
        {
            Response.Write(JsonConvert.SerializeObject(new { result = false }));
        }

        SortedList < string, string > SLString = new SortedList<string, string > ();
        SLString.Add("noncestr", nonceStr);
        SLString.Add("url", url);
        SLString.Add("timestamp", timestamp);
        SLString.Add("jsapi_ticket", ticket);

        StringBuilder sb = new StringBuilder();
        foreach (KeyValuePair<string, string > des in SLString)  
    {
            sb.Append(des.Key + "=" + des.Value + "&");
        }
        string signature = sb.ToString().Substring(0, sb.ToString().Length - 1);
        signature = FormsAuthentication.HashPasswordForStoringInConfigFile(signature, "SHA1").ToLower();

        Response.Write(JsonConvert.SerializeObject(new { result = true, timestamp, nonceStr, signature, appId }));
        Response.End();
    }

    protected void Page_Load(object sender, EventArgs e)
    {
        string url = (Request["Url"] ?? string.Empty).Trim();
        GetWXSharedParam(url);
    }
}
```
 WxSharedClass.cs

 
```
using Newtonsoft.Json;
using System;
using System.Configuration;
using System.IO;
using System.Net;
using System.Text;
using System.Web;




/// <summary>
/// WxSharedClass 的摘要说明
/// </summary>
public static class WxSharedClass
{
    static System.Web.Caching.Cache objCache = HttpRuntime.Cache;

    /// <summary>
    /// 获取jsapi_ticket
    /// 有效期7200秒，开发者必须在自己的服务全局缓存jsapi_ticket
    /// </summary>
    /// <returns></returns>
    public static void GetJsApiTicket()
    {
        string accessToken = string.Empty;
        if (objCache["AccessToken"] == null)
        {
            GetAccessToken();
        }
        accessToken = objCache["AccessToken"] as string;
        if (!string.IsNullOrEmpty(accessToken))
        {
            accessToken = objCache["AccessToken"] as string;
            string url = "https://api.weixin.qq.com/cgi-bin/ticket/getticket?access_token=" + accessToken + "&type=jsapi";
            string resStr = HttpGet(url);
            TicketModel model = JsonConvert.DeserializeObject<TicketModel>(resStr);
            if (!string.IsNullOrEmpty(model.ticket))
            {
                //请求成功了
                DateTime dt = DateTime.Now.AddSeconds(Convert.ToInt32(model.expires_in));
                objCache.Insert("JsApiTicket", model.ticket, null, dt, System.Web.Caching.Cache.NoSlidingExpiration);
            }
        }
    }

    /// <summary>  
    /// DateTime时间格式转换为Unix时间戳格式  
    /// </summary>  
    ///<param name="time"> DateTime时间格式  
    /// <returns>Unix时间戳格式</returns>  
    public static int ConvertDateTimeInt(System.DateTime time)
    {
        System.DateTime startTime = TimeZone.CurrentTimeZone.ToLocalTime(new System.DateTime(1970, 1, 1));
        return (int)(time - startTime).TotalSeconds;
    }

    /// <summary>
    /// 获取access_token
    /// 有效期7200秒，开发者必须在自己的服务全局缓存access_token
    /// </summary>
    /// <returns></returns>
    private static void GetAccessToken()
    {
        string appId = ConfigurationManager.AppSettings["appid"];//订阅号应用id
        string secret = ConfigurationManager.AppSettings["secret"];//订阅号应用密钥
        string url =
            "https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=" + appId + "&secret=" + secret;
        string resStr = HttpGet(url);
        if (!resStr.Contains("errcode"))//string.IsNullOrEmpty(model.errcode)
        {
            //请求成功了
            AccessTokenModel model =JsonConvert.DeserializeObject<AccessTokenModel>(resStr);
            //AccessTokenModel model = JsonHelper.JsonSerializer<AccessTokenModel>(resStr);
            DateTime dt = DateTime.Now.AddSeconds(Convert.ToInt32(model.expires_in));
            objCache.Insert("AccessToken", model.access_token, null, dt, System.Web.Caching.Cache.NoSlidingExpiration);
        }
    }


    /// <summary>
    /// HttpGet请求
    /// </summary>
    ///<param name="url">
    /// <returns></returns>
    private static string HttpGet(string url)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);
        request.Method = "GET";
        request.ContentType = "text/html;charset=UTF-8";
        HttpWebResponse response = (HttpWebResponse)request.GetResponse();
        Stream myResponseStream = response.GetResponseStream();
        StreamReader myStreamReader = new StreamReader(myResponseStream, Encoding.GetEncoding("utf-8"));
        string retString = myStreamReader.ReadToEnd();
        myStreamReader.Close();
        myResponseStream.Close();
        return retString;
    }


}

```
 TicketModel.cs

 
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;

/// <summary>
/// TicketModel 的摘要说明
/// </summary>
public class TicketModel
{
    public string errcode;

    public string errmsg;

    public string ticket;

    public string expires_in;
}
```
 ConvertDateTime.cs

 
```
using System;

namespace Tools
{
    internal class ConvertDateTimeInt
    {
        private DateTime now;

        public ConvertDateTimeInt(DateTime now)
        {
            this.now = now;
        }
    }
}
```
 AccessTokenModel.cs

 
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;

/// <summary>
/// AccessTokenModel 的摘要说明
/// </summary>
public class AccessTokenModel
{
    public string access_token;
    public string expires_in;

}
```
 index.html (用于测试的界面)

 
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title></title>
    <meta charset="utf-8" />
</head>
<body>

</body>
</html>
<script src="JS/jquery.min.js"></script>
<script type="text/javascript" src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>
<script type="text/javascript">
    $.ajax({
        async: false,  // 同步请求  
        url: "http://localhost:1832/shareAjax.aspx",
        type: "get",
        success: function (data) {
            var obj = eval("(" + data + ")");
            alert("1");
            if (obj.result== true) {
                wx.config({
                    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
                    appId: obj.appId, // 必填，公众号的唯一标识
                    timestamp: obj.timestamp, // 必填，生成签名的时间戳
                    nonceStr: obj.nonceStr, // 必填，生成签名的随机串
                    signature: obj.signature,// 必填，签名，见附录1
                    jsApiList: [
                        'onMenuShareTimeline',
                        'onMenuShareAppMessage',
                        'onMenuShareQQ',
                        'onMenuShareWeibo',
                        'onMenuShareQZone'
                    ] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
                });
                wx.ready(function () {
                    alert(data);
                    // config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。
                });

                wx.error(function (res) {
                    alert("error");
                    // config信息验证失败会执行error函数，如签名过期导致验证失败，具体错误信息可以打开config的debug模式查看，也可以在返回的res参数中查看，对于SPA可以在这里更新签名。

                });

                wx.onMenuShareTimeline({
                    title: '', // 分享标题
                    link: '', // 分享链接
                    imgUrl: '', // 分享图标
                    success: function () {
                        // 用户确认分享后执行的回调函数
                    },
                    cancel: function () {
                        // 用户取消分享后执行的回调函数
                    }
                });

                wx.onMenuShareAppMessage({
                    title: '', // 分享标题
                    desc: '', // 分享描述
                    link: '', // 分享链接
                    imgUrl: '', // 分享图标
                    type: '', // 分享类型,music、video或link，不填默认为link
                    dataUrl: '', // 如果type是music或video，则要提供数据链接，默认为空
                    success: function () {
                        // 用户确认分享后执行的回调函数
                    },
                    cancel: function () {
                        // 用户取消分享后执行的回调函数
                    }
                });

            } else {
             //   alert(data);
            }
        }
    });

</script>
```
 我们可以用微信的测试工具–微信web开发者工具 直接进行测试网页。但是一定需要有自己域名，并且绑定为安全域名。

 ![这里写图片描述](https://img-blog.csdn.net/20160617162100282)   
 弹出此对话框的话，标志这我们的验证是完成的，即可调用JSSDK的函数了。

 ![这里写图片描述](https://img-blog.csdn.net/20160617162226909)   
 分享自定义内容成功。

 感谢您的阅读。

 微信开发者文档：[http://mp.weixin.qq.com/wiki/home/index.html](http://mp.weixin.qq.com/wiki/home/index.html)   
 微信Web开发者工具：[https://mp.weixin.qq.com/wiki/10/e5f772f4521da17fa0d7304f68b97d7e.html](https://mp.weixin.qq.com/wiki/10/e5f772f4521da17fa0d7304f68b97d7e.html)

   
  