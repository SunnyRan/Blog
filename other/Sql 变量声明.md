---
title: Sql 变量声明
date: 2016-02-25 19:12:25
tags: CSDN迁移
---
  声明局部变量语法：   
 DECLARE @variable_name DataType   
 其中 variable_name为局部变量的名称，DataType为数据类型。   
 给局部变量赋值有两种方法：   
 1、SET @variable_name=value   
 2、SELECT @variable_name=value   
 两者的区别：SET赋值语句一般用于赋给变量一个指定的常量，SELECT赋值语句一般用于从表中查询出数据然后赋给变量。   
 例如：   
 DECLARE @count int   
 SET @count=123   
 PRINT @count   
 全局变量：   
 由于全局变量是系统定义的，我们这里只做举例。   
 @@ERROR 最后一个T-SQL错误的错误号   
 @@IDENTITY 最后一次插入的标识值   
 @@LANGUAGE 当前使用的语言名称   
 @@MAX_CONNECTIONS 可以创建的同时连接的最大数目   
 @@SERVERNAME 本地服务器的名称   
 @@VERSION SQL Server的版本信息

 这里是触发器的写法：

 create trigger updateTest on test for update   
 as   
 begin   
 declare @id int   
 declare @tablename varchar(100)   
 declare @remark varchar(150)   
 set @tablename=’test’   
 set @remark=”   
 select @id=id from deleted   
 insert into tb_index values(@id,@tablename,@remark)   
 end

 create trigger deleteTest on test for delete   
 as   
 begin   
 declare @id int   
 declare @tablename varchar(100)   
 declare @remark varchar(150)   
 set @tablename=’test’   
 set @remark=”   
 select @id=id from deleted   
 insert into tb_index values(@id,@tablename,@remark)   
 end

 create trigger insertTest on test for insert   
 as   
 begin   
 declare @id int   
 declare @tablename varchar(100)   
 declare @remark varchar(150)   
 set @tablename=’test’   
 set @remark=”   
 select @id=id from inserted   
 insert into tb_index values(@id,@tablename,@remark)   
 end

   
  