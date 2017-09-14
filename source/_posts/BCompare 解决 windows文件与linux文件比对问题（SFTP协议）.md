title: BCompare 解决 windows文件与linux文件比对问题（SFTP协议）
date: 2015-01-25 00:00:00
categories:  BCompare

tags: [ BCompare , windows文件与linux文件比对 ]


---


# 作用
运维人员可以依据此方法针对变化文件直接远程同步
- 优点：操作轻量，非常快，避免项目整包上传，节省时间


# 先展示
* windos目录:
G:\workspace\wosai\Shopping\OnlineShoping\WebContent\WEB-INF


* linux目录:
profile:root@121.40.50.157?/usr/local/apache-tomcat-6.0.32/webapps/ROOT/WEB-INF



![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/56122133.jpg)


# 保存会话: Shift+Ctrl+S

![](http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/40699125.jpg)





---


# 配置篇
直接比对可能出现问题,原因是因为没有配置


## 情况一: 直接使用BCompare远程连接


`profile:root@121.40.50.157?/usr/local/apache-tomcat-6.0.32/webapps/ROOT/WEB-INF`

![](http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/76344883.jpg)


* 处理:
使用sftp先建立远程连接


![](http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/10786977.jpg)



* 再使用BCompare远程连接


`profile:root@121.40.50.157?/usr/local/apache-tomcat-6.0.32/webapps/ROOT/WEB-INF`
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/13022252.jpg)


## 情况二:  直接使用sftp连接 


`sftp://root:*mima*@121.40.50.157?/usr/local/apache-tomcat-6.0.32/webapps/ROOT/WEB-INF`



- 未配置情况下,直接粘贴的地址,只能定位到根目录(上一级,有可能会出错,也可能不会)


-  上一级(可能性)出错
     -  处理


切换连接: `s ftp://root:*mima*@121.40.50.157?/usr/local/apache-tomcat-6.0.32/webapps/ROOT/WEB-INF `  为` BCompare远程连接`


`profile:root@121.40.50.157?/usr/local/apache-tomcat-6.0.32/webapps/ROOT/WEB-INF`


* BCompare 针对性远程连接配置



![](http://7xnbs3.com1.z0.glb.clouddn.com/16-6-16/84080662.jpg)





<!-- more -->
