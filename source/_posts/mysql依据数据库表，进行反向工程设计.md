title: mysql依据数据库表，进行反向工程设计
date: 2016-07-08 00:00:00
tags: [ mysql依据数据库表 , 反向工程设计 ]


---


# 反向工程设计( 源表用到的其它表字段 )
```
select 
a.TABLE_NAME 表名,a.COLUMN_NAME 列名
,b.TABLE_NAME 表名,b.TABLE_COMMENT 备注
from INFORMATION_SCHEMA.COLUMNS a,INFORMATION_SCHEMA.TABLES b
where 1=1
and replace(a.column_name,'_id','') = b.table_name 
and replace(a.column_name,'_id','') != a.TABLE_NAME 
and a.TABLE_SCHEMA='k12platform' 
and a.TABLE_SCHEMA=b.TABLE_SCHEMA
and a.TABLE_NAME ='exp' # 表名(查看表中所有字段信息)
and a.COLUMN_NAME like '%_id'; # 描述列(依据字段,查找周边关系表)
```


-------------------------------------------------------------------------------------
# 演示
**select 'exp',f_tableRel('exp'); **# 主动表关系(源表用到的其它表字段)
```
exp.exp_type_id> exp_type;
exp.grade_id> grade;
exp.school_id> school;
exp.subject> subject
```


** select 'exp',f_tableRel_quote('exp'); **  # 表被动关系(哪些表用到此表的关系)
```
exp.exp_id< black_exp_relation.exp_id;
exp.exp_id< exp_rec.exp_id;
exp.exp_id< homework.exp_id;
exp.exp_id< honor.exp_id;
exp.exp_id< honor_rec.exp_id;
exp.exp_id< log_exp_use.exp_id;
exp.exp_id< log_user_exp_is_read.exp_id;
exp.exp_id< plan_detail.exp_id;
exp.exp_id< sub_exp.exp_id;
exp.exp_id< user_resource.exp_id
```
** select 'exp',f_tableRel_col('exp'); **  # 表中字段关系(各字段与其它表字段雷同信息)
```
exp.web_url <=> qb_activity.web_url;
exp.subject <=> user_bag.subject;
exp.subject <=> file_bag.subject;
exp.subject <=> alipay_log_pay.subject;
exp.subject <=> alipay_log_notify.subject;
exp.is_expire <=> user.is_expire;
exp.ios_url <=> file_bag.ios_url;
exp.icon1 <=> user.icon1;
exp.icon1 <=> third_party_login.icon1;
exp.apk_url <=> user_bag.apk_url;
exp.apk_url <=> file_bag.apk_url;
exp.apk_package_name <=> user_bag.apk_package_name;
exp.apk_package_name <=> file_bag.apk_package_name;
exp.apk_class_name <=> user_bag.apk_class_name;
exp.apk_class_name <=> file_bag.apk_class_name
```


** select 'exp',f_tableRel_quote_two('exp'); ** # 被动二度关系表(用到此表的中间表,再关系到中间表的二度关系表)
```
exp.*id< user_resource[exp_id].user_id< user.user_id;
exp.*id< log_user_exp_is_read[exp_id].user_id< user.user_id;
exp.*id< honor_rec[exp_id].user_id< user.user_id;
exp.*id< exp_rec[exp_id].user_id< user.user_id;
exp.*id< sub_exp[exp_id].sub_id< sub.sub_id;
exp.*id< exp_rec[exp_id].homework_id< homework.homework_id
```


** select 'exp',f_tableRel_two('exp'); **  # 主动二度关系表(用到源表的中间表,再找到用到中间表id的主动二度表)
```
exp.subject_id>subject.id> qb_school_question.subject_id;
exp.subject_id>subject.id> qb_platform_question.subject_id;
exp.subject_id>subject.id> qb_personal_question.subject_id;
exp.subject_id>subject.id> qb_exercise.subject_id;
exp.subject_id>subject.id> personal_bag_question.subject_id;
exp.school_id>school.id> user.school_id;
exp.school_id>school.id> qb_school_question.school_id;
exp.school_id>school.id> qb_platform_question.school_id;
exp.school_id>school.id> qb_personal_question.school_id;
exp.school_id>school.id> exp.school_id;
exp.school_id>school.id> depclass.school_id;
exp.grade_id>grade.id> user_bag.grade_id;
exp.grade_id>grade.id> question_special.grade_id;
exp.grade_id>grade.id> qb_school_question.grade_id;
exp.grade_id>grade.id> qb_platform_question.grade_id;
exp.grade_id>grade.id> qb_personal_question.grade_id;
exp.grade_id>grade.id> qb_exercise.grade_id;
exp.grade_id>grade.id> personal_bag_question.grade_id;
exp.grade_id>grade.id> microcourse.grade_id;
exp.grade_id>grade.id> 
```


---


# 函数-反向工程设计:主动表关系( 源表用到的其它表字段 )
```
DROP FUNCTION IF EXISTS f_tableRel;


CREATE FUNCTION f_tableRel(tableName varchar(20)) 
RETURNS varchar(1024)
BEGIN 
DECLARE result varchar(1024);

select 
GROUP_CONCAT(a.TABLE_NAME,'.',CONCAT(a.COLUMN_NAME,'> ',b.TABLE_NAME) SEPARATOR ';\r\n') into result
from INFORMATION_SCHEMA.COLUMNS a,INFORMATION_SCHEMA.TABLES b
where 1=1
and replace(a.column_name,'_id','') = b.table_name # 找到包含A表中*_Id字段的目标表,B表
and replace(a.column_name,'_id','') != a.TABLE_NAME # 忽略自身表
and a.TABLE_SCHEMA='k12platform' 
and a.TABLE_SCHEMA=b.TABLE_SCHEMA
and a.TABLE_NAME =tableName; # 表名(查看表中所有字段信息)




return result;
END;
```



## 调用函数
```

select 'exp',f_tableRel('exp');
```



# 函数-反向工程设计:表被动关系(哪些表用到此表的关系)
```

DROP FUNCTION IF EXISTS f_tableRel_quote;


CREATE FUNCTION f_tableRel_quote(tableName varchar(20)) 
RETURNS varchar(1024)
BEGIN 
DECLARE result varchar(1024);

select 
GROUP_CONCAT(CONCAT(tableName,'.',CONCAT(tableName,'_id'),'< ',a.TABLE_NAME,'.',a.COLUMN_NAME) SEPARATOR ';\r\n') into result
from INFORMATION_SCHEMA.COLUMNS a
where 1=1
and a.TABLE_SCHEMA='k12platform' 
and a.TABLE_NAME != tableName
and a.COLUMN_NAME = CONCAT(tableName,'_id');# 用到字段[表名_id]的表,即被引用表


return result;
END;
```



## 调用函数
```

select 'exp',f_tableRel_quote('exp');
```



# 函数-反向工程设计:表中字段关系(各字段与其它表字段雷同信息)
```

DROP FUNCTION IF EXISTS f_tableRel_col;


CREATE FUNCTION f_tableRel_col(tableName varchar(20)) 
RETURNS varchar(1024)
BEGIN 
DECLARE result varchar(1024);

select 
GROUP_CONCAT(CONCAT(a.TABLE_NAME,'.',a.COLUMN_NAME,' <=> ',b.TABLE_NAME,'.',b.COLUMN_NAME) ORDER BY a.COLUMN_NAME DESC SEPARATOR ';\r\n') into result
from INFORMATION_SCHEMA.COLUMNS a,INFORMATION_SCHEMA.COLUMNS b
where 1=1
and a.column_name = b.column_name # 用到相同表字段的表(存在着间接关系,或)
and a.column_name not like '%time'
and a.column_name not like '%date'
and a.column_name not like '%id' # 非Id字段
and a.TABLE_NAME != b.TABLE_NAME
and a.TABLE_SCHEMA='k12platform' 
and a.TABLE_SCHEMA=b.TABLE_SCHEMA
and a.TABLE_NAME = tableName; # 表名(查看表中所有字段信息)


return result;
END;
```



## 调用函数
```

select 'exp',f_tableRel_col('exp');
```



# 函数-反向工程设计:被动二度关系表(用到此表的中间表,再关系到中间表的二度关系表)
```

DROP FUNCTION IF EXISTS f_tableRel_quote_two;


CREATE FUNCTION f_tableRel_quote_two(tableNameA varchar(20)) 
RETURNS varchar(1024)
BEGIN 
DECLARE result varchar(1024);

select 
GROUP_CONCAT(
CONCAT(tableNameA,'.',tableNameA,'_id','< ',rel.TABLE_NAME,'[',tableNameA,'_id].',rel.COLUMN_NAME,'< ',b.TABLE_NAME,'.',b.COLUMN_NAME)
ORDER BY b.COLUMN_NAME DESC SEPARATOR ';\r\n') into result

# 独立查询
# CONCAT('exp','.*id','< ',rel.TABLE_NAME,'[','exp','_id].',rel.COLUMN_NAME,'< ',b.TABLE_NAME,'.',b.COLUMN_NAME)


from INFORMATION_SCHEMA.COLUMNS rel,INFORMATION_SCHEMA.COLUMNS b
where 1=1

and rel.TABLE_NAME  in (
# f_tableRel_quote('exp'); # 表被动关系(哪些表用到此表的关系)
select TABLE_NAME from INFORMATION_SCHEMA.COLUMNS 
where column_name = concat(tableNameA,'_id') and TABLE_SCHEMA='k12platform'
)
and rel.column_name = concat(b.TABLE_NAME,'_id')# 通过中间表,查找二度关系表(二度关系表仅取id字段)
and b.column_name = concat(b.TABLE_NAME,'_id')# 二度关系表,仅显示id字段

and rel.TABLE_NAME != b.TABLE_NAME
and rel.TABLE_SCHEMA='k12platform'
and rel.TABLE_SCHEMA=b.TABLE_SCHEMA
and b.TABLE_NAME != tableNameA; # 二度关系表,忽略自身表

return result;
END;
```



## 调用函数
```

select 'exp',f_tableRel_quote_two('exp');
```



# 函数-反向工程设计:主动二度关系表(用到源表的中间表,再找到用到中间表id的主动二度表)
```

DROP FUNCTION IF EXISTS f_tableRel_two;


CREATE FUNCTION f_tableRel_two(tableNameA varchar(20)) 
RETURNS varchar(1024)
BEGIN 
DECLARE result varchar(1024);

select 
GROUP_CONCAT(
CONCAT(tableNameA,'.',rel.TABLE_NAME,'_id','>',rel.TABLE_NAME,'.',rel.COLUMN_NAME,'> ',two.TABLE_NAME,'.',two.column_name)
ORDER BY rel.TABLE_NAME DESC SEPARATOR ';\r\n') into result


# 独立查询
# CONCAT(rel.TABLE_NAME,'.',rel.COLUMN_NAME,'> ',two.TABLE_NAME,'.',two.column_name)
from INFORMATION_SCHEMA.COLUMNS rel,INFORMATION_SCHEMA.COLUMNS two
where 1=1
and rel.TABLE_SCHEMA='k12platform' 
and rel.TABLE_SCHEMA=two.TABLE_SCHEMA


# and rel.table_name in ('exp_type','grade','school','subject')
and rel.table_name in (
# f_tableRel('exp'); # 主动表关系(用到的其它表关系)
select 
b.TABLE_NAME
from INFORMATION_SCHEMA.COLUMNS a,INFORMATION_SCHEMA.TABLES b
where 1=1
and replace(a.column_name,'_id','') = b.table_name # 找到包含A表中*_Id字段的目标表,B表
and replace(a.column_name,'_id','') != a.TABLE_NAME # 忽略自身表
and a.TABLE_SCHEMA='k12platform' 
and a.TABLE_SCHEMA=b.TABLE_SCHEMA
and a.TABLE_NAME =tableNameA # 表名(查看表中所有字段信息)
)


and (rel.COLUMN_NAME = 'id' or rel.column_name = CONCAT(rel.TABLE_NAME,'_id'))
and concat(rel.table_name,'_id') = two.COLUMN_NAME;


return result;
END;
```



## 调用函数
```

select 'exp',f_tableRel_two('exp');
```



<!-- more -->
