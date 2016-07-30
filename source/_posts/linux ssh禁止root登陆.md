title: linux ssh禁止root登陆
date: 2016-07-20 00:00:01
tags: [ linux ,ssh]


---


# 新建管理用户
```
adduser sysadmin
passwd sysadmin    # 修改密码
```


# 同步用户环境变量
```
cp -rf .bashrc /home/sysadmin/
cp -rf cfg /home/sysadmin/
```


# 赋予用户超级权限
```
sudo   vim /etc/sudoers # 或 visudo 


root ALL=(ALL) ALL # 参照物
sysadmin ALL=(ALL) ALL #  sudo ** 需要输入密码
 

:wq! # 强制保存退出
```
```
# 每次输入的时候不输入当前用户的密码可以这样设置（不推荐这样用）

sysadmin ALL=(ALL) NOPSSWD:ALL  #  sudo ** 不需要输入密码

```


# 关闭root登陆
```
sudo vim /etc/ssh/sshd_config


PermitRootLogin yes => PermitRootLogin no
```


# 重启sshd服务
```
sudo /etc/init.d/sshd restart
```


---
 
# 新建普通用户（视情况而定）
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
(admin is not in the sudoers file.  This incident will be reported.)
```


<!-- more -->
