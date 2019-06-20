---
title: SQL内联、左联、右联、全联查询语法
date: 2017-03-28 13:32:43
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/67634846   
  概述：   
 联合查询效率较高，举例子来说明联合查询：内联inner join 、左联left outer join 、右联right outer join 、全联full outer join 的好处及用法。   
 联合查询效率较高，以下例子来说明联合查询(内联、左联、右联、全联)的好处：

 
     T1表结构（用户名,密码） | userid（int） | usernamevarchar（20） | password varchar（20）
     ------------- | ----------- | ------------------- | -------------------- 
                   | 1           | jack                | jackpwd             
                   | 2           | owen                | owenpwd             

 
--------
 
     T1表结构（用户名,密码） | userid（int） | jifenvarchar（20） | dengji varchar（20）
     ------------- | ----------- | ---------------- | ------------------ 
                   | 1           | 20               | 3                 
                   | 3           | 50               | 6                 

 **第一：内联(inner join)。**   
 如果想把用户信息、积分、等级都列出来，那么一般会这样写：   
  `select * from T1 ,T3 where T1.userid = T3.userid`    
 (其实这样的结果等同于 `select * from T1 inner join T3 on T1.userid=T3.userid`  )。   
 把两个表中都存在userid的行拼成一行(即内联)，但后者的效率会比前者高很多，建议用后者(内联)的写法。   
 SQL语句： `select * from T1 inner join T2 on T1.userid=T2.userid` 

 ![这里写图片描述](https://img-blog.csdn.net/20170328112016375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 **第二：左联(left outer join)。**   
 显示左表T1中的所有行，并把右表T2中符合条件加到左表T1中;右表T2中不符合条件，就不用加入结果表中，并且NULL表示。

 
```
select * from T1 left outer join T2 on T1.userid=T2.userid
```
 ![这里写图片描述](https://img-blog.csdn.net/20170328133049652?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 第三：右联(right outer join)。   
 显示右表T2中的所有行，并把左表T1中符合条件加到右表T2中;左表T1中不符合条件，就不用加入结果表中，并且NULL表示。

 
```
select * from T1 right outer join T2 on T1.userid=T2.userid
```
 ![这里写图片描述](https://img-blog.csdn.net/20170328133126173?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 第四：全联(full outer join)。   
 显示左表T1、右表T2两边中的所有行，即把左联结果表+右联结果表组合在一起，然后过滤掉重复的。   
 

 
```
　select * from T1 full outer join T2 on T1.userid=T2.userid
```
 ![这里写图片描述](https://img-blog.csdn.net/20170328133150892?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3VubnlfUmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 总结，关于联合查询，效率的确比较高，4种联合方式如果可以灵活使用，基本上复杂的语句结构也会简单起来。这4种方式是：1)Inner join 2)left outer join 3)right outer join 4)full outer join

   
  