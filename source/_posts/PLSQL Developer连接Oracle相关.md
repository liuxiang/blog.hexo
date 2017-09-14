title: PLSQL Developer连接Oracle相关
date: 2017-3-15 00:00:02
tags: [oracle ]


---


# 安装`PL/SQL`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/9554637.jpg)



plsqlde v12 注册码：
Product Code(产品编号)：4t46t6vydkvsxekkvf3fjnpzy5wbuhphqz
serial Number(序列号)：601769
password(口令)：xs374ca
http://www.fxxz.com/soft/216837.html



---
# 下载`oracle客户度`程序
http://www.oracle.com/technetwork/indexes/downloads/index.html#database
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/68724956.jpg)


路径：`Database` > `Database Features` > `Database Instant Client`

http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html
- `OCI,OCCI,JDBC-OCI`相关程序下载
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/59505301.jpg)


`Basic` 基础版【72M】
`Basic Lite` 基础精简版【38M】
解压至任意目录（如： F:\Tool\DB Orecle\instantclient_12_1\ ）


---

# 登录窗口取消进入设置，更新`PL/SQL`配置Oracle,OCI库位置。
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/15296306.jpg)


- 重启`PL/SQL`效果(数据库 无法读取 )
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/74710081.jpg)



---

# 初始配置文件

## 获取连接文件   ` tnsnames.ora`
- 从oracle服务器中获取：
   E:\app\Administrator\product\12.1.0\dbhome_1\NETWORK\ADMIN\  ` tnsnames.ora`
- 放置oracle客户端：
   F:\Tool\DB Orecle\instantclient_12_1\  ` tnsnames.ora` 下
- 配置环境变量

TNS_ADMIN = ` F:\Tool\DB Orecle\instantclient_12_1\ `


- 重启`PL/SQL`效果(同步 ` tnsnames.ora`中数据库选择 )  [`可能要重启电脑`]
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/17366696.jpg)



---

# 管理及使用
## DAB账号登录`sys`，连接为`SYSDBA`
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/12589487.jpg)


## 新建表空间（PL/SQL不提供试图创建，常用SQL）
## 新建用户（指定表空间）
```
-- Create the user 
create user wosai
  identified by ws2014
  default tablespace BRIGHT
  temporary tablespace TEMP;
-- Grant/Revoke role privileges 
grant dba to wosai with admin option;
```



错误：ORA-65096：公用用户名或角色名无效
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/91807164.jpg)


- 解决

```
-- Create the user 
create user C##wosai
  identified by ws2014
  default tablespace BRIGHT
  temporary tablespace TEMP;
-- Grant/Revoke role privileges 
grant dba to C##wosai with admin option;
```


## 新建表
（创建表sql中表名，字段名均不带引号，生成表名及字段名为全大写，sql和程序使用时不区分大小写）
```
-- Create table
create table country
(
  id          number,
  countryname varchar2(32),
  countrycode varchar2(32)
)
;
-- Create/Recreate primary, unique and foreign key constraints 
alter table country
  add constraint pk_id primary key (ID);
```


---

# 如遇plsql中查询的数据无法显示中文
- 手动配置环境变量：
`NLS_LANG`= `SIMPLIFIED CHINESE_CHINA.ZHS16GBK`



`plsql查询数据显示为乱码解决方法 - aovenus的专栏 - 博客频道 - CSDN.NET`

http://blog.csdn.net/aovenus/article/details/12648751


`plsql中文复制乱码首先配置环境变量：NLS_LANG为SIMPLIFIED CHINESE_CHINA.ZHS16GBK`
http://blog.csdn.net/sun6220083/article/details/44342327



---
**参考** `PLSQL Developer连接Oracle11g 64位数据库配置详解` ★
http://blog.csdn.net/chen_zw/article/details/9292455/  `instantclient-basic-win32-11.2.0.1.0`



`PL/SQL通过修改配置文件的方式实现数据库的连接`
http://jingyan.baidu.com/article/c74d600080632a0f6a595d80.html


`PLSQL-Developer数据库连接工具使用方法_百度经验`
http://jingyan.baidu.com/article/48b558e3540ecf7f38c09a3c.html


`plsql 连接oracle数据库详细配置 - weinichendian的博客 - 博客频道 - CSDN.NET`  
http://blog.csdn.net/weinichendian/article/details/51735443   `oracle64位客户端 Instant Client v11.2.0.3.0(64-bit)`


---


`使用pl/sql来Oracle创建表空间和创建用户 - 海阔天空 - 博客频道 - CSDN.NET`
http://blog.csdn.net/cpp_lzth/article/details/6128297