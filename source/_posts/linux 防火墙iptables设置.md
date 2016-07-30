title: linux 防火墙iptables设置
date: 2016-07-21 00:00:01
tags: [linux, iptables ]


---


# 限制端口服务( 生效小 延迟)
```
# 限制端口服务( 生效小 延迟)

iptables -A INPUT -i lo -j ACCEPT #允许来自于lo接口的数据包
iptables -A INPUT -p tcp --dport 1997 -j ACCEPT #ssh端口22->1997
# iptables -A INPUT -p tcp --dport 21 -j ACCEPT #FTP端口21
iptables -A INPUT -p tcp --dport 80 -j ACCEPT #web服务端口80
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT #tomcat
# iptables -A INPUT -p tcp --dport xxxx -j ACCEPT #mysql
# iptables -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT #允许icmp包通过,也就是允许ping
# iptables -A INPUT -p tcp -s 45.96.174.68 -j ACCEPT #如果要添加内网ip信任（接受其所有TCP请求）
iptables -A INPUT -m state --state ESTABLISHED -j ACCEPT #允许所有对外请求的返回包
iptables -P INPUT DROP #过滤所有非以上规则的请求


iptables -L -n --line-numbers # 所有iptables以序号标记显示
```
```
iptables -L -n --line-numbers # 所有iptables以序号标记显示

iptables -D INPUT 7 # 删除具体行规则

```


---
# 保存,重启-立即生效

```
# 保存,重启-立即生效

/etc/rc.d/init.d/iptables save
service iptables restart


iptables -L -n --line-numbers # 所有iptables以序号标记显示
```


---
# 清空规则(重置)
```
# 清空规则(重置)

iptables -P INPUT ACCEPT # 恢复默认允许访问(不恢复就麻烦了~)
iptables -F # 清空默认所有规则
iptables -X # 清空自定义的所有规则
iptables -Z # 计数器置0


iptables -L -n --line-numbers #  所有iptables以序号标记显示
```


---
# 开机启动-chkconfig
```
# 开机启动-chkconfig

service iptables save # 保存现状
chkconfig iptables on  # 添加到自启动chkconfig


chkconfig --list | grep '3:on'  # 检查开机启动
```
```
chkconfig iptables off  # 删除
chkconfig iptables on  # 恢复


chkconfig --list | grep '3:on'  # 检查开机启动
```


---
**参考* *
`阿里云Centos配置iptables防火墙 | KK文章`
http://blog.kkyun.com/?p=50


`iptables命令_Linux iptables 命令用法详解：Linux上常用的防火墙软件`
http://man.linuxde.net/iptables



<!-- more -->
