---
title: 获取客户端IP的方法
date: 2018-03-19 20:10:06
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/79616682   
  ## IP获取来源

 1.’REMOTE_ADDR’ 是远端IP，默认来自tcp 连接是，客户端的Ip。可以说，它最准确，确定是，只会得到直接连服务器客户端IP。如果对方通过代理服务器上网，就发现。获取到的是代理服务器IP了。

 如：a->b(proxy)->c ,如果c 通过’REMOTE_ADDR’ ，只能获取到b的IP,获取不到a的IP了。

 另外:该IP想篡改将很难实现，在传递知道生成php server值，都是直接生成的。

 2.’HTTP_X_FORWARDED_FOR’，’HTTP_CLIENT_IP’ 为了能在大型网络中，获取到最原始用户IP，或者代理IP地址。对HTTp协议进行扩展。定义了实体头。

 HTTP_X_FORWARDED_FOR = clientip,proxy1,proxy2 所有IP用”,”分割。 HTTP_CLIENT_IP 在高级匿名代理中，这个代表了代理服务器IP。既然是http协议扩展一个实体头，并且这个值对于传入端是信任的，信任传入方按照规则格式输入的。以下以x_forword_for例子加以说明，正常情况下，这个值变化过程。

 ![这里写图片描述](//img-blog.csdn.net/20180319195630948?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L1N1bm55X1Jhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 
## 分析风险点

 通过刚刚分析我们发现，其实这些变量，来自http请求的：x-forword-for字段，以及client-ip字段。 正常代理服务器，当然会按rfc规范来传入这些值。但是，这些值用户是可以直接伪造的。   
 ![这里写图片描述](//img-blog.csdn.net/20180319200422268?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L1N1bm55X1Jhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 ![这里写图片描述](//img-blog.csdn.net/20180319200429635?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L1N1bm55X1Jhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 x-forwarded-for不光可以自己设置值，而且可以设置任意格式值。 这样一来，好比就直接有一个可以写入任意值的字段。并且服务器直接读取，或者写入数据库，或者做显示。它将带来危险性，跟一般对入输入没有做任何过滤检测，之间操作数据源结果一样。 并且容易带来隐蔽性。

 结论：   
 所以有些getip函数，除了客户端可以任意伪造IP，并且可以传入任意格式IP。 这样结果会带来2大问题，其一，如果你设置某个页面，做IP限制。 对方可以容易修改IP不断请求该页面。 其二，这类数据你如果直接使用，将带来SQL注入，跨站攻击等漏洞。至于其一，可以在业务上面做限制，最好不采用IP限制。 对于其二，这类可以带来巨大网络风险。我们必须加以纠正。

 
```
比如你用类似上面的代码中获取IP地址就直接有这样的SQL语句: 
string sql = "INSERT INTO (IP) VALUE ('" + IP + "')"; 
那么也许破坏者还可以进行SQL注入进行数据破坏!!
```
 需要对getip 进行修改，得到安全的getip函数。

 
## 负载均衡

 在使用NetScale等负载均衡设备或者服务的时候，可以通过负载均衡服务器对客户端的REMOTE_ADDR地址进行编辑，比如扩展实体头（比如:HTTP_XXXX_CLIENTIP），增加对于REMOTE_ADDR的记录。就可以保证Web服务器在读取HTTP_XXXX_CLIENTIP时，就可以读取到相对于安全IP.

   
  