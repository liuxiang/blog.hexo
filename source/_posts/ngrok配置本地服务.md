title: ngrok配置本地服务
date: 2017-4-12 00:00:00
tags: [ ngrok ]


---


# 一、 ngrok配置本地服务
- ` ngrok.yml`
```
tunnels:
  h80:
    proto: http
    addr: 80
  h8080:
    proto: http
    addr: 8080
```
- 运行


```
ngrok.exe start -config ngrok.yml --all

或指定
ngrok.exe start -config ngrok.yml h80
ngrok.exe start -config ngrok.yml h8080
```


- 启动日志
```
ngrok by @inconshreveable                                       (Ctrl+C to quit)


Session Status                online
Update                        update available (version 2.2.4, Ctrl-U to update)
Version                       2.1.18
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://3c9e34ca.ngrok.io -> localhost:80
Forwarding                    http://a85188d8.ngrok.io -> localhost:8080
Forwarding                    https://3c9e34ca.ngrok.io -> localhost:80
Forwarding                    https://a85188d8.ngrok.io -> localhost:8080


Connections                   ttl     opn     rt1     rt5     p50     p90
                              38      0       0.08    0.04    21.30   97.93
```


- 访问（8080的后端服务单独 端口隧道 ）
http://3c9e34ca.ngrok.io/louyu?host=http://a85188d8.ngrok.io/bright/web/



---
# 利用nginx反向代理掉8080服务
- `nginx.conf`
```
        location / {
            root   html;
            index  index.html index.htm;
        }


        location /bright {
            root   html;
            proxy_pass  http://localhost:8080;
        }
```


- 访问（仅使用一个80端口隧道）

```
http://3c9e34ca.ngrok.io/louyu?host=http://3c9e34ca.ngrok.io/bright/web

```


## 缺陷
1.海外服务器,速度慢
2.360安全提醒
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/78635931.jpg)

 
---
# 二、（国内）NATAPP基于ngrok的国内高速内网穿透服务
https://natapp.cn/


-  ` 教程 `   https://natapp.cn/article/natapp_newbie
- 使用本地配置文件 ` config.ini `   https://natapp.cn/article/config_ini
我的 authtoken  `c4392fc2e74ed855`



`自定义子域名需购买付费产品`
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/5497063.jpg)
 
## 效果
- `config.ini`
```
#将本文件放置于natapp同级目录 程序将读取 [default] 段
#在命令行参数模式如 natapp -authtoken=xxx 等相同参数将会覆盖掉此配置
[default]
authtoken= c4392fc2e74ed855                       #对应一条隧道的authtoken
clienttoken=                    #对应客户端的clienttoken,将会忽略authtoken,若无请留空,
logto=none                      #log 日志文件, 可以是 none 代表不记录 或者 stdout 代表直接屏幕输出 ,默认为none
loglevel=DEBUG                  #日志等级 DEBUG, INFO, WARNING, ERROR 默认为 DEBUG
http_proxy=                     #代理设置 如 http://http://10.123.10.10:3128
```


- 启动[ windows ] ：运行>` natapp.exe`
```
Powered By NATAPP       Please visit https://natapp.cn          (Ctrl+C to Quit)
Tunnel Status           Online
Version                 2.3.4
Forwarding              http://ik2ui2.natappfree.cc -> 127.0.0.1:80
Web Interface           http://127.0.0.1:4040
Total Connections       14
```


- 访问
```
http://ik2ui2.natappfree.cc/louyu?host=http://ik2ui2.natappfree.cc/bright/web/

```


- 免费缺点：
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/45480419.jpg)



---
# 三、完全免费服务 `Ngrok国内免费服务器——糖果科技`
http://www.qydev.com/
运行实例： `ngrok -config=ngrok.cfg -subdomain xxx 80`


- 运行
```
ngrok -config=ngrok.cfg -subdomain liuxiang 80

```
- 启动日志
```
ngrok                                                           (Ctrl+C to quit)


Tunnel Status                 online
Version                       1.7/1.7
Forwarding                    http://wosai.tunnel.qydev.com -> 127.0.0.1:80
Forwarding                    https://wosai.tunnel.qydev.com -> 127.0.0.1:80
Web Interface                 127.0.0.1:4040
# Conn                        21
Avg Conn Time                 3486.01ms
```


---
# 四、自建ngrok服务
开源代码：`inconshreveable/ngrok`  https://github.com/inconshreveable/ngrok


- ngrok开发者指南

https://github.com/inconshreveable/ngrok/blob/master/docs/DEVELOPMENT.md


- 大致过程
```
一、安装git
二、安装go环境
三、编译ngrok
四、为域名生成证书
五、在软件源代码目录下面会生成一些证书文件，我们需要把这些文件拷贝到指定位置
六、如果是在国内的服务器需要改，香港或者国外的服务器不需要
七、编译服务端（根据自己系统的来）
八、编译客户端
九、客户端配置文件
十、服务端启动
十一、客户端使用
```


**参考**
`ngrok服务器搭建步骤 - 简书` `★`
http://www.jianshu.com/p/b254547b9fe5
http://www.jianshu.com/p/694dd87510f8


`自建ngrok服务 - 简书`
http://www.jianshu.com/p/fcc73e019936


`自建Ngrok服务与使用方法 - 季樊 - 博客园`
http://www.cnblogs.com/pwenlee/p/5302880.html


`在阿里云搭建自己的ngrok服务 - 推酷`
http://www.tuicool.com/articles/ZraURrq


---
`映射公网花生壳、PubYun、NoIP、DynDNS、Ngrok、Tunnel、localtu... - 简书`
http://www.jianshu.com/p/b11d70d9dd52


`自搭Ngrok实现树莓派内网穿透 - 简书`
http://www.jianshu.com/p/91f01e30a9b0



