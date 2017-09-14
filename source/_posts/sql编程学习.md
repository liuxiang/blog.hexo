title: sql编程学习
date: 2016-06-16 00:00:00
categories: sql编程

tags: [ sql编程 ]


---


# 系统函数

> 系统函数   数据库系统定义的函数，即内置函数



|  函数列别 |     说明  |
| :-------- |  : --------|
| 聚合函数   |   执行的操作是将多个值合并为一个值。例如 COUNT、SUM、MIN 和MAX。
| 配置函数   |   是一种标量函数，可返回有关配置设置的信息。
|   加密函数   |   支持加密、解密、数字签名和数字签名验证。
|   游标函数   |   返回有关游标状态的信息。
|   日期和时间函数   |   可以更改日期和时间的值。
|   数学函数   |   执行三角、几何和其他数字运算。
|   元数据函数  |    返回数据库和数据库对象的属性信息。
|   排名函数   |   是一种非确定性函数，可以返回分区中每一行的排名值。
|   行集函数   |   返回可在 Transact-SQL 语句中表引用所在位置使用的行集。
|   安全函数  |    返回有关用户和角色的信息。
|   字符串函数    |  可更改 char、varchar、nchar、nvarchar、binary 和 varbinary 的值。
|   系统函数   |   对系统级的各种选项和对象进行操作或报告。
|   系统统计函数   |   返回有关 SQL Server 性能的信息。
|   文本和图像函数   |   可更改 text 和 image 的值。


## 常用函数，练习
```
# 字符串函数
select ASCII('中');    # 返回字符的ASCII码值   
select BIT_LENGTH('中');    # 返回字符串的比特长度   
select LENGTH('中');    # 返回字符串str中的字符数   
select CONCAT('钓','鱼','岛');    # 将s1,s2…,sn连接成字符串    任何字符串与NULL进行连接的结果都将是NULL


# 数值函数
select ABS(2);    # 返回x的绝对值
select ABS(-2);    # 返回x的绝对值
select BIN(2); #    返回x的二进制（OCT返回八进制，HEX返回十六进制）
select CEILING(2.2); # 返回大于x的最小整数值


# 日期时间函数
select CURRENT_DATE();# 返回当前的日期
select CURRENT_TIME();#    返回当前的时间
select SYSDATE();# 系统时间
select DATE_ADD(SYSDATE(),INTERVAL 1 day);# 返回日期date加上间隔时间int的结果(int必须按照关键字进行格式化),如：SELECT DATE_ADD(CURRENT_DATE,INTERVAL 6 MONTH);


# 流程函数
select (CASE WHEN '钓鱼岛' = '日本' THEN 'yes' ELSE 'no' END); # 如果testN是真，则返回resultN，否则返回default
select (CASE '钓鱼岛' WHEN '日本' THEN 'yes' ELSE 'no' END); # 如果test和valN相等，则返回resultN，否则返回default
select if('钓鱼岛'='日本','yes','no'); # 如果test是真，返回t；否则返回 f
select IFNULL('true','空'); # 如果arg1不是空，返回arg1，否则返回arg2
select IFNULL(null,'空');# 如果arg1不是空，返回arg1，否则返回arg2
```
```
# 流程控制

select FLOOR(1+RAND() * (14+1)) into @device_rand;
select @device_rand,(CASE
                        WHEN @device_rand=1 THEN 'Android 6.x'
                        WHEN 1<@device_rand && @device_rand<=3 THEN 'iOS 9 (iPad)'
                        WHEN 3<@device_rand && @device_rand<=7 THEN 'iOS 9 (iPhone)'
                        WHEN 7<@device_rand && @device_rand<=14 THEN 'Android 5.x'
                        END);
```
>更多：用户自定义函数， 表值函数， 标量值函数.  
（详见文章： 关系数据库SQL之可编程性函数（用户自定义函数））



**参考** 
`关系数据库SQL之可编程性函数（用户自定义函数）`
http://www.cnblogs.com/seayxu/p/5472302.html


`MySQL常用函数与运算符`
http://shanks.leanote.com/post/Mysql%E5%87%BD%E6%95%B0%E4%B8%8E%E8%BF%90%E7%AE%97%E7%AC%A6


---


# 结构化查询语言-sql(structured query language)

- DDL 数据定义语言
create:创建表, drop:删除表 , alter:修改表
-  DML 数据操作语言
insert:增 ,delete:删, update:改

- DQL 数据查询语言
select:查

- DCL 数据控制语言
用来设置或者更改数据库用户或角色权限的语句，包括GRANT、DENY、REVOKE等语句，在默认状态下，只有sysadmin、dbcreator、db_owner或db_securityadmin等角色的成员才有权利执行数据控制语言。
- 多表查询
    - 简单联结（inner join，left join）

    - 组合级联（union，union all）

     - 子查询


> 更多: 索引，试图，导入/导出，备份/恢复


**参考** 


`sql 语句学习  -持续更新中 - 简书`
http://www.jianshu.com/p/d00fd0d4f1cc


`SQL 学习笔记 - 简书`
http://www.jianshu.com/p/2dacd3040c74


`（大数据工程师学习路径）第四步 SQL基础课程----修改和删除 - 推酷`
http://www.tuicool.com/articles/yuUFba2


`（大数据工程师学习路径）第四步 SQL基础课程----select详解 - 推酷`
http://www.tuicool.com/articles/za6Fz2


`（大数据工程师学习路径）第四步 SQL基础课程----其他（基础练习到此为止） - 爱看球的领带 - 博客园`

http://www.cnblogs.com/yangxiao99/p/4719347.html


---
# 综合练习
## Tips ( 临时表， 自动标记序号，全局变量 )
-  临时表 ( 更多请读： oracle函数 with as(临时表)用法 & mysql的变通实现 .md )
```
#  临时表 ( 更多请读： oracle函数 with as(临时表)用法 & mysql的变通实现 .md )
drop table if exists tab; create table tab as
select * from(
    select '张三' name,'男' sex,18 age,SYSDATE() time
    union select '李四','女',28,date_sub(SYSDATE(),interval 1 day)
) tab;
 
select * from tab;
```
-  自动标记序号
```
# 自动标记序号
select (@rownum := @rownum+1) AS rowno,temp.* from
(
select * from tab,(Select (@rowNum :=0)) r
) temp
```
-  全局用户变量  user-variables
```
# 全局用户变量  user-variables

SET @t1=1, @t2=2, @t3:=4;
SELECT @t1, @t2, @t3, @t4 := @t1+@t2+@t3;


set @temp;
select max(age) into @temp from tab;
select @temp;
select * from tabA where age>@temp;
```
`user-variables` http://dev.mysql.com/doc/refman/5.7/en/user-variables.html


- 系统参数
```
# 系统参数
SELECT @@global.sql_mode, @@session.sql_mode, @@sql_mode;
SHOW VARIABLES;
select @@back_log;
SHOW VARIABLES LIKE 'back_log';
```
`set-statement`  http://dev.mysql.com/doc/refman/5.7/en/set-statement.html
` system-variables(完整) ` http://dev.mysql.com/doc/refman/5.7/en/dynamic-system-variables.html


-  客户端 编程接口
```
# 客户端 编程接口
SET @s = CONCAT("SELECT ", '*', " FROM tab");
prepare stmt FROM @s;
execute stmt;
drop prepare stmt;


PREPARE stmt1 FROM 'SELECT SQRT(POW(?,2) + POW(?,2)) AS hypotenuse';
SET @a = 3,@b = 4;
EXECUTE stmt1 USING @a, @b;
 
SET @s = 'SELECT SQRT(POW(?,2) + POW(?,2)) AS hypotenuse';
PREPARE stmt2 FROM @s;
SET @a = 6,@b = 8;
EXECUTE stmt2 USING @a, @b;
```
`prepared-statements`  http://dev.mysql.com/doc/refman/5.7/en/sql-syntax-prepared-statements.html


---
## 时间条件
```
drop table if exists tab; create table tab as
select * from(
    select '张三' name,'男' sex,18 age,SYSDATE() time
    union select '李四','女',28,date_sub(SYSDATE(),interval 1 day)
) tab;
select * from tab;
```
```
select * from tab where time > DATE_FORMAT('2016-11-30','%Y-%m-%d %H:%i:%s');
select * from tab where time > DATE_FORMAT(str_to_date('2016-11-30','%Y-%m-%d'),'%Y-%m-%d %H:%i:%s');


select * from tab where time > str_to_date('2016-11-30','%Y-%m-%d');
select * from tab where time > '2016-11-30';
```


## 多表查询
```

drop table if exists tabA; create table tabA as
select * from(
    select 1 id,'刘一' name,'男' sex,18 age,SYSDATE() time
    union select 2,'陈二','女',28,date_sub(SYSDATE(),interval 1 day)
    union select 3,'张三','女',28,date_sub(SYSDATE(),interval 2 day)
    union select 4,'李四','女',29,date_sub(SYSDATE(),interval 3 day)
    union select 5,'王五','女',29,date_sub(SYSDATE(),interval 4 day)
    union select 6,'赵六','女',30,date_sub(SYSDATE(),interval 5 day)
    union select 7,'孙七','女',30,date_sub(SYSDATE(),interval 6 day)
) tabA;
 
drop table if exists tabB; create table tabB as
select * from(
    select 6 id,'赵六' name,'男' sex,30 age,SYSDATE() time
    union select 7,'孙七','女',30,date_sub(SYSDATE(),interval 1 day)
    union select 8,'周八','女',28,date_sub(SYSDATE(),interval 2 day)
    union select 9,'吴九','女',28,date_sub(SYSDATE(),interval 3 day)
    union select 10,'郑十','女',28,date_sub(SYSDATE(),interval 4 day)
) tabB;


select * from tabA a,tabB b where a.id=b.id;             # 简写内联
select * from tabA a inner join tabB b on a.id=b.id;    # 内联
select * from tabA a left join tabB b on a.id=b.id;       # 左外
select * from tabA a right join tabB b on a.id=b.id;     # 右外
```


## 分组 & 聚合
```
# 分组 group by *
select age,count(*) from tabA group by age;# 除分组列可直接展示外,其它字段都只能在函数计算成单值才能显示(count,max,sum)


# 聚合函数 group_concat
select age,count(*),group_concat(name) from tabA group by age;# 合并数值(聚合)
select age,count(*),group_concat(name separator ';') from tabA group by age;# 改变分隔符
select age,count(*),group_concat(name order by time) from tabA group by age;# 时间升序
select age,count(*),group_concat(name order by time desc) from tabA group by age;# 时间降序
```


## distinct去重
```
select * from tabA;
select count(*) from tabA;
select count(distinct name) from tabA;
select count(distinct age) from tabA;
```
## 查询重复 & 删除重复
```
# 查找(仅显示重复)
select age,count(age),group_concat(name) from tabA group by age having count(age) > 1;
select * from tabA where age in (select age from tabA group by age having count(age) > 1);
 
# 去重(mysql缺省-非分组字段显示组内第一条)
select * from tabA group by age
 
# 去重-重复项中显示最早一条数据[min(Id)]或最近一条数据[max(id)]
select * from tabA a where a.id = (select min(id) from tabA b where a.age=b.age); # 最早一条数据[min(Id)]
select * from tabA a where a.id = (select max(id) from tabA b where a.age=b.age); # 最近一条数据[max(id)]
 
# 删除重复
select * from tabA a where a.id > (select min(id) from tabA b where a.age=b.age);
delete from tabA a where a.id > (select min(id) from tabA b where a.age=b.age);# oralce正常,mysql语法错误
delete from tabA where id in
    (select id from (select * from tabA) a where a.id > (select min(id) from (select * from tabA) b where a.age=b.age));# 改成这样(/恶心)
 
## 检查
select * from tabA;
```


#  视图
```
# 删除已存在 视图  & 创建新视图
Drop view if exists v_tabAB; create view v_tabAB as
select a.* from tabA a,tabB b where a.name=b.name;


select * from v_tabAB;
```


# 存储过程
```
drop PROCEDURE IF EXISTS  simpleproc ;
delimiter //


CREATE PROCEDURE simpleproc (OUT o_param1 INT)
BEGIN
SELECT COUNT(*) into o_param1 FROM tab;
END//


delimiter ;


CALL simpleproc(@a);
select @a;
```
`create-procedure` http://dev.mysql.com/doc/refman/5.7/en/create-procedure.html



- 扩展阅读 `MySql delimiter的作用是什么`  http://www.jb51.net/article/24815.htm


- 带参存储过程，几种执行方式
```
CREATE PROCEDURE p (OUT o_ver_param VARCHAR(25), INOUT incr_param INT)
BEGIN
  SELECT VERSION() INTO o_ver_param;
  SET incr_param = incr_param + 1;
END;
 
# 参数引用传递
SET @increment = 10;
CALL p(@version, @increment);
SELECT @version, @increment;
 
# 带参使用（prepare 编程接口）
SET @increment = 10;
PREPARE s FROM 'CALL p(?, ?)';
EXECUTE s USING @version, @increment;
SELECT @version, @increment;
 
SET @increment = 10;
PREPARE s FROM 'CALL p(@version, @increment)';
EXECUTE s;
SELECT @version, @increment;
```
`call`  http://dev.mysql.com/doc/refman/5.7/en/call.html


- 游标 Cursors
```
CREATE PROCEDURE curdemo()
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE a CHAR(16);
  DECLARE b, c INT;
  DECLARE cur1 CURSOR FOR SELECT id,data FROM test.t1;
  DECLARE cur2 CURSOR FOR SELECT i FROM test.t2;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
 
  OPEN cur1;
  OPEN cur2;
 
  read_loop: LOOP
    FETCH cur1 INTO a, b;
    FETCH cur2 INTO c;
    IF done THEN
      LEAVE read_loop;
    END IF;
    IF b < c THEN
      INSERT INTO test.t3 VALUES (a,b);
    ELSE
      INSERT INTO test.t3 VALUES (a,c);
    END IF;
  END LOOP;
 
  CLOSE cur1;
  CLOSE cur2;
END;
```
` Cursors ` http://dev.mysql.com/doc/refman/5.7/en/cursors.html
- 扩展阅读 `mysql循环方式`
```
三个标准的循环方式：WHILE循环，LOOP循环以及REPEAT循环。
一 个 非标准的循环方式：GOTO


WHILE……DO……END WHILE
REPEAT……UNTIL END REPEAT
LOOP……END LOOP
GOTO

```
http://www.soso.io/article/13660.html
http://www.blogjava.net/rain1102/archive/2011/05/16/350301.html



# tips: 存储过程中纪录日志方法
```

# 初始化
drop table if exists p_log;
create table p_log as select 1 'id','p_name','log' ,SYSDATE() date;


select * from p_log; # 存储过程,进度监控
```

```
# 标记主键,更新长度
ALTER TABLE `p_log`
MODIFY COLUMN `id`  int(1) NOT NULL AUTO_INCREMENT FIRST ,
MODIFY COLUMN `p_name`  varchar(64) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' AFTER `id`,
MODIFY COLUMN `log`  varchar(1024) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL DEFAULT '' AFTER `p_name`,
ADD PRIMARY KEY (`id`);


# 插入测试
insert into p_log value(null,'p_name','log',SYSDATE());
select * from p_log; ```


# 流程控制
```
14.6.5.1 CASE Syntax # 分支（同switch case）
14.6.5.2 IF Syntax
14.6.5.3 ITERATE Syntax    # 重新开始循环（同continue）
14.6.5.4 LEAVE Syntax  #  LEAVE    结束标签（同break）
14.6.5.5 LOOP Syntax # 循环（同for）
14.6.5.6 REPEAT Syntax    # 循环
14.6.5.7 RETURN Syntax
14.6.5.8 WHILE Syntax     # 循环
```
```
CREATE PROCEDURE doiterate(p1 INT)
BEGIN
  label1: LOOP
    SET p1 = p1 + 1;
    IF p1 < 10 THEN
      ITERATE label1; # 重新开始循环（同continue）
    END IF;
    LEAVE label1; #  LEAVE    结束标签（同break）
  END LOOP label1;
  SET @x = p1;
END;
```
`MySQL :: MySQL 5.7 Reference Manual :: 14.6.5 Flow Control Statements`
http://dev.mysql.com/doc/refman/5.7/en/flow-control-statements.html



`MySQL :: MySQL 5.7 Reference Manual :: 14.6.5.5 LOOP Syntax`
http://dev.mysql.com/doc/refman/5.7/en/loop.html



# 计划任务
```
drop EVENT IF EXISTS e_test;


CREATE EVENT `e_test`
    ON SCHEDULE EVERY 5 SECOND
    ON COMPLETION NOT PRESERVE
    ENABLE
    DO
    insert into p_log value('event','x',SYSDATE());
```
```
#1) 临时关闭事件
ALTER EVENT e_test DISABLE;
 
# 2) 开启事件
ALTER EVENT e_test ENABLE;
```
`MySQL :: MySQL 5.7 Reference Manual :: 14.1.12 CREATE EVENT Syntax`
http://dev.mysql.com/doc/refman/5.7/en/create-event.html


`详解 MySQL 的计划任务 - 开源中国社区`
http://www.oschina.net/question/4873_20927




# 数据字典
```
# 其中常用字典库 [information_schema]
select * from INFORMATION_SCHEMA.SCHEMATA;  # 数据库中所有数据库信息
select * from INFORMATION_SCHEMA.TABLES; # 存放数据库中所有数据库表信息
select * from INFORMATION_SCHEMA.COLUMNS;  # 所有数据库表的列信息
select * from INFORMATION_SCHEMA.STATISTICS;# 存放索引信息


# 获取表信息
select TABLE_SCHEMA DB,TABLE_NAME 表名,TABLE_COMMENT 描述,TABLE_ROWS 数据条数,concat(round(data_length/(1024*1024),2),'M') 容量
from INFORMATION_SCHEMA.TABLES where TABLE_SCHEMA='k12_platform' ;
 
# 获取表中字段信息
select TABLE_SCHEMA DB,COLUMN_NAME 列名,TABLE_NAME 表名,COLUMN_COMMENT 备注,COLUMN_TYPE 类型,is_nullable 可否为空,CHARACTER_MAXIMUM_LENGTH 最大值长度
from INFORMATION_SCHEMA.COLUMNS
where 1=1
and TABLE_SCHEMA='k12_platform'
and TABLE_NAME ='user'                 # 表名(查看表中所有字段信息)
and COLUMN_COMMENT like '%昵称%' # 描述列(依据描述,找具体字段及所在表)
and COLUMN_NAME like 'nick_name'; # 描述列(依据字段,查找周边关系表)
```


---


# 进阶
- 工具掌握(PL/sql，Navicat..)
- 数据字典

- 存储过程(逻辑语句， 事务控制 )
- 执行计划

- 性能调优（索引，分区，分表）
- 数据库设计

-  场景问题解决方案（去重，分组，聚合..）
- 数据库管理(锁表/释放 )
- 数据库安全

- 数据库规范&案例
- nosql延伸


---
**参考**
`MySQL 5.7参考手册(MySQL 5.7 Reference Manual)`

http://dev.mysql.com/doc/refman/5.7/en/create-table.html
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/90252887.jpg)


第三方分类  http://overapi.com/mysql
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-7-1/10071748.jpg)


** Oracle    Database SQL Reference 数据库 SQL参考（完全 参考手册 ）**
`oracle Database - Functions 函数`
http://docs.oracle.com/cd/E11882_01/timesten.112/e21642/function.htm#TTSQL446


`oracle Database - SQL Statements SQL语句`
http://docs.oracle.com/cd/E11882_01/timesten.112/e21642/state.htm#TTSQL277
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/1486632.jpg)


<!-- more -->


