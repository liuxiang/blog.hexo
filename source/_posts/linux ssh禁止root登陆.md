title: linux ssh禁止root登陆
date: 2016-07-20 00:00:01
tags: [ linux ,ssh]


---


# 1. 新建管理用户
```
adduser sysadmin
passwd sysadmin    # 修改密码
```


# 1.1 同步用户环境变量
```
cp -rf .bashrc /home/sysadmin/
cp -rf cfg /home/sysadmin/
```


# 1.2  赋予用户超级权限
```
sudo   vim /etc/sudoers # 或 visudo 


root ALL=(ALL) ALL # 参照物
sysadmin ALL=(ALL) ALL #  sudo ** 需要输入密码
 

:wq! # 强制保存退出
```
```
# 每次输入的时候不需要输入当前用户的密码可以这样设置（不推荐这样用）

sysadmin      ALL=(ALL) NOPASSWD: ALL   #  sudo ** 不需要输入密码
```


# 2. 关闭root登陆
```
sudo vim /etc/ssh/sshd_config


PermitRootLogin yes => PermitRootLogin no
```


# 3. 重启sshd服务
```
sudo /etc/init.d/sshd restart
```


---
 
# 4. 新建普通用户（视情况而定）
```
adduser admin
passwd admin    # 修改密码
```


# 同步用户环境变量
```
cp -rf .bashrc /home/ admin /
cp -rf cfg /home/ admin /
```


# Test：不可sudo执行超级权限
```
[test@iZ23huf4wo4Z ~]$ sudo vim /etc/ssh/sshd_config
[sudo] password for test: 
test is not in the sudoers file.  This incident will be reported.
```


<!-- more -->
