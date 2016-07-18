title: oracle函数 with as(临时表)用法 & mysql的变通实现
date: 2013-07-07 00:00:00
categories:  临时表

tags: [oracle 函数  with as , mysql ]


---


# with table as 介绍:
> 相当于建个临时表（用于一个语句中某些中间结果放在临时表空间的SQL语句），Oracle 9i 新增WITH语法，可以将查询中的子查询命名，放到SELECT语句的最前面。


**何时被清除 **
临时表不都是会话结束就自动被PGA清除嘛! 但with as临时表是查询完成后就被清除了！


- 1.用作临时表


```
with tab as(
select 'aaa','bbb','ccc' from dual
union
select 'aaa2','bbb2','ccc2' from dual
)
select * from tab;
```
```
with tt as (
  select 'aaa' id, '高' value from dual union all
  select 'bbb' id, '低' value from dual union all
  select 'aaa' id, '低' value from dual union all
  select 'aaa' id, '高' value from dual union all
  select 'bbb' id, '低' value from dual union all
  select 'bbb' id, '高' value from dual)
SELECT id,
       COUNT(decode(VALUE, '高', 1)) 高,
       COUNT(decode(VALUE, '低', 1)) 低
  FROM tt
GROUP BY id;
```
```
WITH
Q1 AS (SELECT 3 + 5 S FROM DUAL),
Q2 AS (SELECT 3 * 5 M FROM DUAL),
Q3 AS (SELECT S, M, S + M, S * M FROM Q1, Q2)
SELECT * FROM Q3;
```
- 2.就是把一大堆重复用到的SQL语句放在with as 里面，取一个别名，后面的查询就可以用它.这样对于大批量的SQL语句起到一个优化的作用
```
with a as (select * from test)
select * from a;
```
---
# mysql 并不支持此关键字。单功能依然在
**Oracle**
```
with tab as(
select 'aaa','bbb','ccc' from dual
union
select 'aaa2','bbb2','ccc2' from dual
)
select * from tab;
```
** mysql **
```
# 临时表，方式一
select * from (
    select 'aaa,','bbb','ccc' from dual
    union
    select 'aaa2,','bbb2','ccc2' from dual
) tab;
```
或
```
# 临时表，方式二
drop table if exists tab; create table tab as
select * from(
    select '张三' name,'男' sex,18 age,SYSDATE() time
    union select '李四','女',28,date_sub(SYSDATE(),interval 1 day)
) tab;
 
select * from tab;
 
# 临时表，方式二(补充AB表)
drop table if exists tabA; create table tabA as
select * from(
    select '刘一' name,'男' sex,18 age,SYSDATE() time
    union select '陈二','女',28,date_sub(SYSDATE(),interval 1 day)
    union select '张三','女',28,date_sub(SYSDATE(),interval 2 day)
    union select '李四','女',28,date_sub(SYSDATE(),interval 3 day)
    union select '王五','女',28,date_sub(SYSDATE(),interval 4 day)
    union select '赵六','女',28,date_sub(SYSDATE(),interval 5 day)
    union select '孙七','女',28,date_sub(SYSDATE(),interval 6day)
) tabA;
drop table if exists tabB; create table tabB as
select * from(
    select '赵六' name,'男' sex,18 age,SYSDATE() time
    union select '孙七','女',28,date_sub(SYSDATE(),interval 1 day)
    union select '周八','女',28,date_sub(SYSDATE(),interval 2 day)
    union select '吴九','女',28,date_sub(SYSDATE(),interval 3 day)
    union select '郑十','女',28,date_sub(SYSDATE(),interval 4 day)
) tabB;
```


**参考**
`mysql中怎么实现with..as操作，请大神帮忙。`
http://zhidao.baidu.com/link?url=3L-qB-MhKme7JPbbOn34jlLGd7HsJqy_xLtuFCYAcXo2Rcaqi9hMd7Z-bCXUa7SdFxuQvWu1CXQGM_QvJeTEHK


<!-- more -->