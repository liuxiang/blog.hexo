title: emoji表情mysql支持
date: 2016-09-22 00:00:00
tags: [emoji,mysql]
 
---


# 异常 ：mysql Incorrect string value \xF0\x9F\x98\x84\xF0\x9F

尝试插入 类似 这样的string（其实是emoji表情），这些uft8 占位过多，数据库如果用标准utf8 格式插入不了这样的字符串


- 解决
使用uft8mb4 格式  
(1)设置表格式为utf8mb4
(2)设置连接格式为utf8mb4


http://blog.csdn.net/taitoubiyan1/article/details/50595407


`mysql - java.sql.SQLException: Incorrect string value: '\xF0\x9F\x91\xBD\xF0\x9F...' - Stack Overflow`
http://stackoverflow.com/questions/13653712/java-sql-sqlexception-incorrect-string-value-xf0-x9f-x91-xbd-xf0-x9f


`【emoji表情】阿里云数据库RDS支持emoji表情 - 推酷`  ★ ★ ★
http://www.tuicool.com/articles/2qYbQ3I


`搜索 - 推酷 : mysql emoji`
http://www.tuicool.com/search?kw=mysql+emoji&t=1
