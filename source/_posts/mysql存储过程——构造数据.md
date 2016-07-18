title: mysql存储过程——构造数据
date: 2015-11-025 00:00:00 #发表日期，一般不改动
categories: mysql #文章文类
tags: [mysql,存储过程,构造数据] #文章标签，多于一项时用这种格式
photos:


---
-- 创建测试table
```
-- ----------------------------
-- Table structure for makedata
-- ----------------------------
DROP TABLE IF EXISTS `makedata`;
CREATE TABLE `makedata` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `type` varchar(255) DEFAULT NULL,
  `create_date` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=12 DEFAULT CHARSET=utf8;
```
-- 查
```
select * from makedata;
insert INTO `makedata`(name,age,type,create_date) value('name',11,1,SYSDATE());
```
-- 截断表
```
truncate table makedata;
```
-- 创建函数
```
create procedure p_makeData_test()
begin
    select count(*) from makedata;
end;
```


-- 调用函数
```
call p_makeData_test();
```


-- 删除存储过程
```
drop PROCEDURE IF EXISTS p_makeData;
```


-- 造数据-存储过程
```
create procedure p_makeData()
begin
    declare i,endNum int;/* 起终点 */
    declare rand_age,rand_type int;
    
    set i=0,endNum=10;
        
    -- 循环
    l1:loop
        set rand_age = rand()*100;
        set rand_type = rand()*10;


        insert INTO `makedata`(name,age,type,create_date) 
            value('name',rand_age,rand_type,SYSDATE());
        
        -- 离开条件
        set i = i+1;
        if i > endNum then
            leave l1;
        end if;


    end loop;
end;
```


-- 调用函数
```
call p_makeData();
```
<!-- more -->


# 赠品: 
## mysql循环
3中标准的循环方式： while 循环 、 loop 循环和repeat循环。
还有一种非标准的循环： 
goto。 鉴于goto 语句的跳跃性会造成使用的的思维混乱，所以不建议使用。
```
WHILE……DO……END WHILE
REPEAT……UNTIL END REPEAT
LOOP……END LOOP
GOTO
```
##delimiter的用法
　告知解释器遇到 delimiter后面的符号时作用相当于分号，这样可以避免在shell 中写mysql脚本时，与分号发生冲突.
   只有遇到另外的一个 // 时，才会执行所写的语句
   http://www.cnblogs.com/yujinghui/archive/2013/04/22/3036787.html

