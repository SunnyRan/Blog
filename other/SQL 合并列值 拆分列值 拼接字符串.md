---
title: SQL 合并列值 拆分列值 拼接字符串
date: 2016-08-22 14:51:22
tags: CSDN迁移
---
  ## SQL STUFF函数 拼接字符串

 数据库设计   
 ![这里写图片描述](https://img-blog.csdn.net/20160822144313194)   
 数据库数据展示   
 ![这里写图片描述](https://img-blog.csdn.net/20160822144323599)   
 期望效果   
 ![这里写图片描述](https://img-blog.csdn.net/20160822144334709)

 create table tb(idint, value varchar(10))

 insert into tbvalues(1,’aa’)

 insert into tbvalues(1,’bb’)

 insert into tbvalues(2,’aaa’)

 insert into tbvalues(2,’bbb’)

 insert into tbvalues(2,’ccc’)

 go

 /* stuff(param1, startIndex, length, param2)   
 说明：将param1中自startIndex(SQL中都是从1开始，而非0)起，删除length个字符，然后用param2替换删掉的字符。*/

 
```
SELECT id, value = stuff ((SELECT     ',' + value
FROM tb AS t
WHERE t .id = tb.id FOR xml path('')), 1, 1, '')
FROM tb
GROUP BY id

```
 这样即可。

 注意：   
 将int类型转换为varchar   
 如cast(1 as varchar(10)),再进行连接

 
```
set @sql =@sql+'update User set Medal='+@count+' where ID='+cast(@ID as varchar(10))+';'
```
 
## SQL Server中将多行数据拼接为一行数据（一个字符串）

 方法一: 使用T-SQL   
 DECLARE @Users NVARCHAR(MAX)   
 SET @Users = ”

 SELECT @Users = @Users + ‘,’ + UserName FROM dbo.[User]   
 WHERE RoleID = 1

 SELECT @Users

 转载自：[http://www.fengfly.com/plus/view-172336-1.html](http://www.fengfly.com/plus/view-172336-1.html)

 方法二:使用for xml path(”) 和stuff   
 –使用 自连接、for xml path(”)和stuff合并显示多行数据到一行中 

 –注   
 –1、计算列可以不用包含在聚合函数中而直接显示，如下面语句的val。   
 –2、for xml path(”) 应该应用于语句的最后面，继而生成xml。   
 –3、for xml path(‘root’)中的path参数是生成的xml最顶级节点。   
 –4、字段名或是别名将成为xml的子节点，对于没有列名(字段+”)或是没有别名的字段将直接显示。[value] +’,’则是用,分隔的数据(aa,bb,)。   
 –5、对于合并多行数据显示为一行数据时使用自连。 

 –生成测试表并插入测试数据   
 create table tb(id int, value varchar(10))   
 insert into tb values(1, ‘aa’)   
 insert into tb values(1, ‘bb’)   
 insert into tb values(2, ‘aaa’)   
 insert into tb values(2, ‘bbb’)   
 insert into tb values(2, ‘ccc’)   
 go 

 –第一种显示   
 select id, [val]=(   
 select [value] +’,’ from tb as b where b.id = a.id for xml path(”)) from tb as a   
 –第一种显示结果   
 –1 aa,bb,   
 –1 aa,bb,   
 –2 aaa,bbb,ccc,   
 –2 aaa,bbb,ccc,   
 –2 aaa,bbb,ccc, 

 –第二种显示   
 select id, [val]=(   
 select [value] +’,’ from tb as b where b.id = a.id for xml path(”)) from tb as a   
 group by id   
 –第二种显示结果   
 –1 aa,bb,   
 –2 aaa,bbb,ccc, 

 –第三种显示   
 select id, [val]=stuff((   
 select ‘,’+[value] from tb as b where b.id = a.id for xml path(”)),1,1,”) from tb as a   
 group by id   
 –第三种显示结果   
 –1 aa,bb   
 –2 aaa,bbb,ccc 

 –典型应用   
 –AMD_GiftNew中获取所有的管理员ID   
 –select adminIds = stuff((select ‘,’+cast(UserId as varchar) from MM_Users where RoleId = 1 and flag =0 for xml path(”)),1,1,”)   
 –典型应用显示结果   
 –3,27

 转载自：[http://blog.csdn.net/kula_dkj/article/details/8568599](http://blog.csdn.net/kula_dkj/article/details/8568599)

 select PPID, [val]=stuff((   
 select CAST(SKUCD AS VARCHAR) + ‘,’ from PPSKUTbl as b where b.PPID = a.PPID for xml path(”)),1,1,”) from PPSKUTbl as a where PPID = @PPID group by PPID

 
## SQL— CONCAT(字符串连接函数)

 有的时候，我们有需要将由不同栏位获得的资料串连在一起。每一种资料库都有提供方法来达到这个目的：

  
  * MySQL: CONCAT() 
  * Oracle: CONCAT(), || 
  * SQL Server: +  CONCAT() 的语法如下：   
 CONCAT(字串1, 字串2, 字串3, …): 将字串1、字串2、字串3，等字串连在一起。   
 请注意，Oracle的CONCAT()只允许两个参数；   
 换言之，一次只能将两个字串串连起来。不过，在Oracle中，我们可以用’||’来一次串连多个字串。   
 来看几个例子。假设我们有以下的表格：   
 ![这里写图片描述](https://img-blog.csdn.net/20160822145037109)   
 例子1:

 
```
MySQL/Oracle:
SELECT CONCAT(region_name,store_name) FROM Geography
WHERE store_name = 'Boston';
```
 结果：   
 ‘EastBoston’   
 例子2:

 
```
Oracle:
SELECT region_name || ' ' || store_name FROM Geography
WHERE store_name = 'Boston';
```
 结果：   
 ‘East Boston’   
 例子3:

 
```
SQL Server:
SELECT region_name + ' ' + store_name FROM Geography
WHERE store_name = 'Boston';
```
 结果：   
 ‘East Boston’

 
## 合并列值

 表结构，数据如下：   
 id value

 
--------
 1 aa   
 1 bb   
 2 aaa   
 2 bbb   
 2 ccc

 需要得到结果：   
 id values

 
--------
 1 aa,bb   
 2 aaa,bbb,ccc   
 即：group by id, 求 value 的和（字符串相加）

  
  2. 旧的解决方法(在sql server 2000中只能用函数解决。)   
      –1. 创建处理函数  
```
create table tb(id int, value varchar(10))
insert into tb values(1, 'aa')
insert into tb values(1, 'bb')
insert into tb values(2, 'aaa')
insert into tb values(2, 'bbb')
insert into tb values(2, 'ccc')
go

CREATE FUNCTION dbo.f_str(@id int)
RETURNS varchar(8000)
AS
BEGIN
  DECLARE @r varchar(8000)
  SET @r = ''
  SELECT @r = @r + ',' + value FROM tb WHERE id=@id
  RETURN STUFF(@r, 1, 1, '')
END
GO
```
 – 调用函数

 
```
SELECt id, value = dbo.f_str(id) FROM tb GROUP BY id

drop table tb
drop function dbo.f_str

/*
id value   
----------- -----------
1 aa,bb
2 aaa,bbb,ccc
（所影响的行数为 2 行）
*/
```
 –2、另外一种函数.

 
```
create table tb(id int, value varchar(10))
insert into tb values(1, 'aa')
insert into tb values(1, 'bb')
insert into tb values(2, 'aaa')
insert into tb values(2, 'bbb')
insert into tb values(2, 'ccc')
go
```
 –创建一个合并的函数

 
```
create function f_hb(@id int)
returns varchar(8000)
as
begin
  declare @str varchar(8000)
  set @str = ''
  select @str = @str + ',' + cast(value as varchar) from tb where id = @id
  set @str = right(@str , len(@str) - 1)
  return(@str)
End
go
```
 –调用自定义函数得到结果：

 
```
select distinct id ,dbo.f_hb(id) as value from tb

drop table tb
drop function dbo.f_hb

/*
id value   
----------- -----------
1 aa,bb
2 aaa,bbb,ccc
（所影响的行数为 2 行）
*/
```
  
  2. 新的解决方法(在sql server 2005中用OUTER APPLY等解决。)  
```
create table tb(id int, value varchar(10))
insert into tb values(1, 'aa')
insert into tb values(1, 'bb')
insert into tb values(2, 'aaa')
insert into tb values(2, 'bbb')
insert into tb values(2, 'ccc')
go
```
 – 查询处理

 
```
SELECT * FROM(SELECT DISTINCT id FROM tb)A OUTER APPLY(
  SELECT [values]= STUFF(REPLACE(REPLACE(
  (
  SELECT value FROM tb N
  WHERE id = A.id
  FOR XML AUTO
  ), '<N value="', ','), '"/>', ''), 1, 1, '')
)N
drop table tb

/*
id values
----------- -----------
1 aa,bb
2 aaa,bbb,ccc

(2 行受影响)
*/
```
 –SQL2005中的方法2

 
```
create table tb(id int, value varchar(10))
insert into tb values(1, 'aa')
insert into tb values(1, 'bb')
insert into tb values(2, 'aaa')
insert into tb values(2, 'bbb')
insert into tb values(2, 'ccc')
go

select id, [values]=stuff((select ','+[value] from tb t where id=tb.id for xml path('')), 1, 1, '')
from tb
group by id

/*
id values
----------- --------------------
1 aa,bb
2 aaa,bbb,ccc

(2 row(s) affected)

*/

drop table tb
```
 拆分列值   
 有表tb, 如下:   
 id value

 
--------
 1 aa,bb   
 2 aaa,bbb,ccc   
 欲按id,分拆value列, 分拆后结果如下:   
 id value

 
--------
 1 aa   
 1 bb   
 2 aaa   
 2 bbb   
 2 ccc

  
  2. 旧的解决方法(sql server 2000)  
```
SELECT TOP 8000 id = IDENTITY(int, 1, 1) INTO # FROM syscolumns a, syscolumns b  

SELECT A.id, SUBSTRING(A.[values], B.id, CHARINDEX(',', A.[values] + ',', B.id) - B.id)
FROM tb A, # B
WHERE SUBSTRING(',' + A.[values], B.id, 1) = ','

DROP TABLE #
```
  
  2. 新的解决方法(sql server 2005)   
```
create table tb(id int,value varchar(30))
insert into tb values(1,'aa,bb')
insert into tb values(2,'aaa,bbb,ccc')
go
SELECT A.id, B.value
FROM(
  SELECT id, [value] = CONVERT(xml,'<root><v>' + REPLACE([value], ',', '</v><v>') + '</v></root>') FROM tb
)A
OUTER APPLY(
  SELECT value = N.v.value('.', 'varchar(100)') FROM A.[value].nodes('/root/v') N(v)
)B

DROP TABLE tb

/*
id value
----------- ------------------------------
1 aa
1 bb
2 aaa
2 bbb
2 ccc

(5 行受影响)
*/
```
   
  