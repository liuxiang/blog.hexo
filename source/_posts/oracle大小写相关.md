
title: oracle大小写相关
date: 2017-3-15 00:00:00
tags: [oracle ]


---


# oracle大小写相关
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/90238236.jpg)


```

select * from country;-- ok 对应表COUNTRY
select * from Country;-- ok 对应表COUNTRY
select * from COUNTRY;-- ok 对应表COUNTRY
select * from "COUNTRY";-- ok 对应表COUNTRY
select * from "country";-- [Err] ORA-00942: table or view does not exist


select * from test;-- ok 对应表TEST
select * from Test;-- ok 对应表TEST
select * from TEST;-- ok 对应表TEST
select * from "test";--ok 对应表test
select * from "Test";--[Err] ORA-00942: table or view does not exist
select * from "TEST";--ok 对应表TEST


select * from test2;-- [Err] ORA-00942: table or view does not exist
select * from "test2";-- ok 对应表test2
```


# 规律
`创建的表在数据库是大写`，`sql中就能大小写不区分查询`
`如创建表带了双引号（即小写表名）`，`sql中就必须带双引号查询（这不便于开发） `


# 场景
## 一、Navicat（`不推荐`）,新建表默认SQL （默认被带引号，使用时均需带引号）
```
CREATE TABLE "C##WOSAI"."NewTable" (
"id" NUMBER NOT NULL ,
"name" VARCHAR2(255) NULL ,
PRIMARY KEY ("id")
)
NOCOMPRESS ;
```


## 二、PL/SQL（`推荐`），新建表默认SQL（默认不带引号，使用时将不再区分大小写）
```
-- Create table
create table TEST6
(
  id   NUMBER,
  name VARCHAR2(32)
) ;
```
- 由于创建至数据库中的表名及字段名全大写，为了方便维护，单词间用下划线`_`分隔
如：`CREATE_TIME`


---
# 问题
- `ORA-00959` ： Navicat创建的表空间为小写导致。  ![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/5458590.jpg)


- `ORA-01549` 删除表空间错误
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/37391008.jpg)


解决：
```
# 查询表空间关联
select tablespace_name,count(0) from dba_segments group by tablespace_name;
select owner,tablespace_name,segment_name,segment_type from dba_segments where tablespace_name='bright';


drop table C##WOSAI.COUNTRY;
drop table C##WOSAI.TEST;


drop tablespace "bright";-- 删除 ORA-01549 tablespace not empty,user INCLUDING CONTENTS option
DROP TABLESPACE "bright"　INCLUDING CONTENTS;-- 强制删除
drop tablespace "bright" including contents and datafiles;-- 连带文件一起删除(推荐，否则会残留表空间文件)
```




---
` 关于oracle sql语句查询时表名和字段名要加双引号的问题详解 - Oracle数据库栏目 - 红黑联盟`
http://www.2cto.com/database/201504/387184.html


`ORA-01549-oracle_db-ITPUB博客`
http://blog.itpub.net/15720542/viewspace-721965/



`使用pl/sql来Oracle创建表空间和创建用户 - 海阔天空 - 博客频道 - CSDN.NET`
http://blog.csdn.net/cpp_lzth/article/details/6128297
