---
title: hibernate问题总结。
date: 2016-03-26 09:00:50
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/50985263   
  ```
1
```
 hibernate—Table ‘XXX.XXX’ doesn’t exist   
 在设置自动生成数据表的策略中：   
    
 update//别的值也可以   
 但是出现了一个问题：Table ‘XXX.XXX’ doesn’t exist。   
 解决方法：   
 将Hibernate连接方言改为：org.hibernate.dialect.MySQL5InnoDBDialect   
    
 org.hibernate.dialect.MySQL5InnoDBDialect

 
```
2
```
   
  