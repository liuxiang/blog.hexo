title: jira安装&破解
date: 2017-1-24 00:00:00
tags: [ jira ]


---


# 获取下载地址

https://www.atlassian.com/software/jira/download


# 具体下载地址

- windows
https://downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-7.3.0-x64.exe
- linux 
https://downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-7.3.0-x64.bin
(服务器下载： `wget  https://...`)


# 破解资源
http://down.51cto.com/search.php?q=jira7.2%E7%A0%B4%E8%A7%A3

http://down.51cto.com/data/2246274  （免费）


# 安装条件 `jdk 1.8+`
```
[root@iZ23j23f6aaZ bin]# service jira start


To run JIRA in the foreground, start the server with start-jira.sh -fg
executing using dedicated user: jira
                .....
          .... .NMMMD.  ...
        .8MMM.  $MMN,..~MMMO.
        .?MMM.         .MMM?.


     OMMMMZ.           .,NMMMN~
     .IMMMMMM. .NMMMN. .MMMMMN,
       ,MMMMMM$..3MD..ZMMMMMM.
        =NMMMMMM,. .,MMMMMMD.
         .MMMMMMMM8MMMMMMM,
           .ONMMMMMMMMMMZ.
             ,NMMMMMMM8.
            .:,.$MMMMMMM
          .IMMMM..NMMMMMD.
         .8MMMMM:  :NMMMMN.
         .MMMMMM.   .MMMMM~.
         .MMMMMN    .MMMMM?.


      Atlassian JIRA
      Version : 7.2.6
                  


If you encounter issues starting or stopping JIRA, please see the Troubleshooting guide at http://confluence.atlassian.com/display/JIRA/Installation+Troubleshooting+Guide




Server startup logs are located in /usr/local/jira/logs/catalina.out
Using CATALINA_BASE:   /usr/local/jira
Using CATALINA_HOME:   /usr/local/jira
Using CATALINA_TMPDIR: /usr/local/jira/temp
Using JRE_HOME:        /usr/local/jira/jre/
Using CLASSPATH:       /usr/local/jira/bin/bootstrap.jar:/usr/local/jira/bin/tomcat-juli.jar
Using CATALINA_PID:    /usr/local/jira/work/catalina.pid
*************************************************************************************************************************************
**********     Wrong JVM version! You are running with .. but JIRA requires at least 1.8 to run.      **********
*************************************************************************************************************************************
```


# 转windows安装
- 问题`(Table 'jira.pluginstate' doesn't exist)`
```
JIRA Startup  Failed


org.ofbiz.core.entity.GenericDataSourceException: SQL Exception
while executing the following:SELECT pluginkey, pluginenabled FROM pluginstate
(Table 'jira.pluginstate' doesn't exist)
```
解决办法：手动到数据库中新建此表


- 无法创建管理员账号（Cannot add user,all the user directories are read-only）

 解决办法：重建数据库编码为`utf8`


# 访问

http://192.168.3.198:8888/secure/Dashboard.jspa
admin><admin


# jira 配置邮件




 

  http://mailhelp.mxhichina.com/smartmail/detail.vm?spm=0.0.0.0.4bTq0q&knoId=5871700


---
# 乱码问题处理
`jira汉化完毕，只有仪表板显示是乱码？`
http://www.confluence.cn/questions/6724711


---
**参考**
`烂泥：jira7.2安装、中文及破解-烂泥行天下` -`linux安装`
http://www.ilanni.com/?p=12119


`Windows环境jira 7.1.2安装破解 - ShanDong_Chu - 博客频道 - CSDN.NET` `windows安装`
http://blog.csdn.net/shandong_chu/article/details/52400218


`软件项目开发环境构建之三：JIRA7.2.3安装-布布分享-bubufx.com`   -`linux安装`
http://bubufx.com/detail-1717328.html


`jira+Confluence+FishEye安装破解集成 - 陶呵呵的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/u010414666/article/details/51689247


---
`JIRA 6.3.6安装破解汉化 - 简书`
http://www.jianshu.com/p/a088ab149c78


