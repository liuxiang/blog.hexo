title: linux 服务器批量安全控制
date: 2016-07-28 00:00:00
tags: [ linux , 安全 ]


---


# linux ssh端口修改.md
```

sudo   sed -i "s/#Port 22/Port 1997/g" /etc/ssh/sshd_config
sudo cat /etc/ssh/sshd_config |grep Port
```



# linux ssh禁止root登陆.md
- 新建替代root的管理用户
```

adduser sysadmin
passwd sysadmin    # 修改密码


grep sysadmin /etc/passwd # 查看用户是否存在

```
- 同步用户环境变量
```
cp -rf .bashrc /home/sysadmin/
cp -rf cfg /home/sysadmin/
```
- 为新建的用户（sysadmin）赋予`sudo`权限
```
echo 'sysadmin  ALL=(ALL)    NOPASSWD:ALL' >> /etc/sudoers 
# 或手动: sudo vim  /etc/sudoers

sudo cat /etc/sudoers |grep "ALL=(ALL)"

```
- 关闭root用户的ssh通道（即禁止root登陆）
```
# 更新配置
sudo sed -i "s/PermitRootLogin yes/PermitRootLogin no/g" /etc/ssh/sshd_config
# 查看
sudo   cat /etc/ssh/sshd_config |grep PermitRootLogin


# 重启生效
sudo /etc/init.d/sshd restart
```

# linux 限制普通用户su切换到 root.md
```

# 修改sysadmin用户分组为wheel
usermod -g  wheel sysadmin
# 查看
id sysadmin


# 更新配置（wheel组外用户不允许切换至root）
sudo vim /etc/pam.d/su # auth required pam_wheel.so use_uid (解开此注释)
# 查看
sudo   cat /etc/pam.d/su |grep 'required'
```

# linux 防火墙iptables设置.md
```
sudo iptables -A INPUT -i lo -j ACCEPT #允许来自于lo接口的数据包
sudo   iptables -A INPUT -p tcp --dport 1997 -j ACCEPT #ssh端口22->1997
#  sudo  iptables -A INPUT -p tcp --dport 21 -j ACCEPT #FTP端口21
sudo   iptables -A INPUT -p tcp --dport 80 -j ACCEPT #web服务端口80
sudo   iptables -A INPUT -p tcp --dport 8080 -j ACCEPT #tomcat
#  sudo   iptables -A INPUT -p tcp --dport xxxx -j ACCEPT #mysql
#  sudo   iptables -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT #允许icmp包通过,也就是允许ping
#  sudo   iptables -A INPUT -p tcp -s 45.96.174.68 -j ACCEPT #如果要添加内网ip信任（接受其所有TCP请求）
sudo   iptables -A INPUT -m state --state ESTABLISHED -j ACCEPT #允许所有对外请求的返回包
sudo   iptables -P INPUT DROP #过滤所有非以上规则的请求


# 查看（ 所有iptables以序号标记显示 ）
sudo iptables -L -n --line-numbers


# 开机启动-chkconfig
sudo service iptables save # 保存现状
sudo chkconfig iptables on # 添加到自启动chkconfig
chkconfig --list | grep '3:on' # 检查开机启动
```



# linux nginx权限控制策略.md
```

sudo chown -R sysadmin /usr/local/nginx/html 
sudo chown -R sysadmin /usr/local/apache-tomcat-*/webapps


cd /usr/local/nginx/html && ll
cd  /usr/local/apache-tomcat-*/webapps  && ll
```



---


# 恢复(建议使用root用户)
# 四 . linux ssh端口修改.md
```

sudo   sed -i "s/Port 1997/#Port 22/g" /etc/ssh/sshd_config
sudo cat /etc/ssh/sshd_config |grep Port



sudo /etc/init.d/sshd restart

```



# 二 . linux ssh禁止root登陆.md
- 恢复root用户的ssh通道（即 恢复 root登陆）
```
sudo sed -i "s/PermitRootLogin no/PermitRootLogin yes/g" /etc/ssh/sshd_config
sudo cat /etc/ssh/sshd_config |grep PermitRootLogin


sudo /etc/init.d/sshd restart
```
- 回收：新建的用户（sysadmin）赋予sudo权限

```
sudo vim /etc/sudoers #  手动删除或注释  'sysadmin  ALL=(ALL)    NOPASSWD:ALL'  
sudo cat /etc/sudoers |grep "ALL=(ALL)"
```
- 回收：新建替代root的管理用户
```

userdel sysadmin #  仅删除用户帐号，而不删除相关文件。( 需手动删除用户主目录和邮件目录)
#  -f 删除用户登入目录以及目录中所有文件。
rm -rf /home/sysadmin

rm -rf /var/mail/sysadmin


# 或
userdel -r sysadmin # -r 彻底删除（含用户主目录和邮件目录）


grep sysadmin /etc/passwd

```


#  三. linux 限制普通用户su切换到 root.md
```

sudo   vim /etc/pam.d/su # auth required pam_wheel.so use_uid (还原注释)

sudo   cat /etc/pam.d/su |grep 'required'
```


# 一.linux 防火墙iptables设置.md

```
# 清空规则(重置)
sudo   iptables -P INPUT ACCEPT # 恢复默认允许访问(不恢复就麻烦了~)
sudo   iptables -F # 清空默认所有规则
sudo   iptables -X # 清空自定义的所有规则
sudo   iptables -Z # 计数器置0


# 查看（ 所有iptables以序号标记显示 ）

sudo   iptables -L -n --line-numbers


# 开机启动-chkconfig
sudo service iptables save # 保存现状
sudo   chkconfig iptables off # 删除
chkconfig --list | grep '3:on' # 检查开机启动
```



# 五. linux nginx权限控制策略.md
```

sudo chown -R root /usr/local/nginx/html 
sudo chown -R root /usr/local/apache-tomcat-*/webapps


cd /usr/local/nginx/html && ll
cd  /usr/local/apache-tomcat-*/webapps  && ll
```



 <!-- more -->

