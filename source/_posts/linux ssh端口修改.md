title: linux ssh端口修改
date: 2016-07-21 00:00:02
tags: [linux,ssh]
 
---
# 编辑`sshd`服务配置文件
``` linux
sudo vim /etc/ssh/sshd_config
```
 
# 更改端口
```  cpp
# Port 22 => Port 1997
```
 
# 重启sshd服务
```  cpp
sudo /etc/init.d/sshd restart
```


<!-- more -->