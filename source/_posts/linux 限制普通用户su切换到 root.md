title: linux 限制普通用户su切换到 root
date: 2016-07-20 00:00:02
tags: [linux,su]


---


# 对允许切换root的用户添加到`wheel`分组
```
usermod -g  wheel sysadmin
```
- 查看用户所在用户组
```
[root@iZ23huf4wo4Z ~]# groups sysadmin
sysadmin : wheel



[root@iZ23huf4wo4Z ~]# id sysadmin #   查看用户情况
uid=502(sysadmin) gid=10(wheel) groups=10(wheel)
```
- 查看用户组下成员
```
more /etc/group | grep wheel

grep GID /etc/passwd

```
```
wheel:x:10:


[root@iZ23huf4wo4Z ~]# grep 10: /etc/passwd
uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
sysadmin:x:500:10::/home/sysadmin:/bin/bash
```


# 开启限制`wheel`组外登陆配置
```
sudo vim /etc/pam.d/su
# auth required pam_wheel.so use_uid (解开此注释)
```
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/43413358.jpg)

 
#  Test：切换
```
su root
或
su -
```
 
# 查看当前登录
```
[sysadmin@iZ23zj3xeplZ ~]$ id
uid=503(sysadmin) gid=10(wheel) groups=10(wheel)
```
还有`whoami` 和 `cd ` `pwd` 看看默认的路径


---


`云服务器 ECS CentOS 系统限制普通用户切换到 root 管理员账号`
https://help.aliyun.com/knowledge_detail/37566.html


<!-- more -->
