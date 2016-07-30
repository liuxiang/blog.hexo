title: linux Tips
date: 2016-05-17 00:00:00
tags: [linux,Tips]


---


- 查找进程
ps -ef|grep wiki
 
- 查看`进程/服务`，具体的安装路径 （方法一）
ps -ef | grep java
ps -ef | grep nginx
ps -ef | grep tomcat
```
root      1573     1  0 Jul15 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx
nobody    1574  1573  0 Jul15 ?        00:19:25 nginx: worker process    
root     31106 28615  0 14:27 pts/0    00:00:00 grep nginx
```


- 查看`进程/服务`，具体的安装路径（方法二）
`ls -l /proc/$PID/exe` ($PID为对应的pid号)
```
[sysadmin@iZ23j23f6aaZ ~]$ ps -ef|grep tomcat
root      7420     1  5 11:08 pts/0  ...
[sysadmin@iZ23j23f6aaZ ~]$ sudo ls -l /proc/7420/exe
lrwxrwxrwx 1 root root 0 Jul 25 11:08 /proc/7420/exe -> /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.75.x86_64/jre/bin/java


[sysadmin@iZ23j23f6aaZ ~]$ ps -ef|grep nginx
root      4824     1  0 Jul22 ?       ...
[sysadmin@iZ23j23f6aaZ ~]$ sudo ls -l /proc/4824/exe
lrwxrwxrwx 1 root root 0 Jul 22 18:23 /proc/4824/exe -> /usr/local/nginx/sbin/nginx
```


- 停止进程
kill -7 xxx
 
- 查看那个进程占用了xxx端口
lsof -i:xxx


<!-- more -->