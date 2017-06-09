title: linux文本操作
date: 2017-2-9 00:00:00
tags: [ linux ]


---


# 显示最后`10000`行
`tail -n 10000 catalina.out`
或 `tail -10000 catalina.out`



- 输出文件

`tail -10000 catalina.out > catalina.out.0209`



# 查询文件总行数
`wc -l catalina.out`


# 查询目标字符串行数位置` 2017-02-09 12:00:06 `
注意：`时间选择需要精确,否则会卡死控制台，耗死服务器，锁死文件`


起点行数：`grep -n "2017-02-09 12:00:06" catalina.out`
```
[root@iZ23ult4m4yZ logs]# grep -n "2017-02-09 12:00:06" catalina.out
210048:[k12Service] 2017-02-09 12:00:06,285 WARN  [com.wosai.teach.dao.VersionDao.getVersionByCode()] (VersionDao.java:54)     - list.size() > 0 || code is app
```
终点行数：`grep -n "2017-02-09 12:59:59,3" catalina.out`

```
[root@iZ23ult4m4yZ logs]# grep -n "2017-02-09 12:59:59, 3 " catalina.out
312707:[k12Service] 2017-02-09 12:59:59,344 INFO  [com.wosai.teach.restful.CommonInterface.checkVersion()] (CommonInterface.java:71)     - 

312712:[k12Service] 2017-02-09 12:59:59,345 WARN  [com.wosai.teach.restful.CommonInterface.checkVersion()] (CommonInterface.java:74)     - 版本信息不完全,可能是非法访问
```


---
# 打印出起始行和结束行之间的内容
`sed -n  ' 210048 , 210049 p'   catalina.out `


- 输出文件


`sed -n  ' 210048 , 312712 p'   catalina.out >  catalina.out.0209.12 `


# 过滤打印
`cat catalina.out.0209.12 | grep 操作 计时 `
```
[k12Service]  操作计时：188ms  类名: com.wosai.teach.restful.knowPoint.KnowPointLoadExpReadInterFace  方法名:getMain() result:
[k12Service]  操作计时：29ms  类名: com.wosai.teach.restful.sys.SysConfigInterface  方法名:sysConfig() result:
[k12Service]  操作计时：120ms  类名: com.wosai.teach.restful.order.FindProductListInterface  方法名:getMain() result:
[k12Service]  操作计时：112ms  类名: com.wosai.teach.restful.order.FindProductListInterface  方法名:getMain() result:
[k12Service]  操作计时：224ms  类名: com.wosai.teach.restful.knowPoint.KnowPointLoadExpReadInterFace  方法名:getMain() result:
```
- 输出文件
` cat catalina.out.0209.12 | grep 操作 计时 >  catalina.out.0209.12.grep`
```
[root@iZ23ult4m4yZ logs]# vi catalina.out
catalina.out               catalina.out.0209.12       catalina.out.0209.12.grep 
```
- 下载文件
` sz catalina.out.0209.12.grep `


---
**参考**
`linux基本命令-文本过滤与处理 - halazi100 - 博客频道 - CSDN.NET`
http://blog.csdn.net/halazi100/article/details/42266885


`Linux下查看文件内容的命令 - 梨璐2012 - 博客园`
http://www.cnblogs.com/luying--lulu/p/5314963.html


`linux中查看文件指定行的数据_百度经验`
http://jingyan.baidu.com/article/15622f24125872fdfdbea560.html


` Linux 中查看文件第n行内容的命令`
http://blog.csdn.net/fireblue1990/article/details/51534271
