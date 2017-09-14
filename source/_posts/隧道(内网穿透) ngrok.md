title: 隧道(内网穿透) ngrok
date: 2017-3-21 00:00:00
tags: [ 隧道 ]


---


`官网`https://ngrok.com/ 

`下载` https://ngrok.com/download


`中文`  http://ngrok.cn/


---
# 示例：将本地计算机的端口80上的Web服务器公开到互联网

```
ngrok http 8080

```
```
ngrok by @inconshreveable                                                                               (Ctrl+C to quit)


Session Status                online
Version                       2.1.18
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://ab940a47.ngrok.io -> localhost:8080
Forwarding                    https://ab940a47.ngrok.io -> localhost:8080


Connections                   ttl     opn     rt1     rt5     p50     p90
                              9       0       0.07    0.03    33.37   40.13


HTTP Requests
-------------


GET  /static/js/jquery-1.11.1.min.js 200 OK
GET  /API/demo/monitor/report        200 OK
GET  /WEB-INF/views/app/main.html    404 Not Found
GET  /demo                           200 OK
GET  /web/sysConfig                  200 OK
POST /API/demo/monitor/report        200 OK
GET  /static/css/style.css           200 OK
GET  /static/js/jquery-1.11.1.min.js 200 OK
GET  /demo                           200 OK
GET  /app/controller2main.html       200 OK
```
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/96821335.jpg)

 
# Web UI 管理页面
[`http://localhost:4040`]( http://localhost:4040 )
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/64319008.jpg)
 
# 重新请求`Replay`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/26203212.jpg)
 
# 安装您的`Authtoken`
```
ngrok authtoken <YOUR_AUTHTOKEN>

```


# 自定义子域名称
```
ngrok http -subdomain=liuxiang 8080

```
```
隧道会话失败：只有付费计划可能会绑定自定义子域。
无法为未经身份验证的客户端绑定自定义子域“liuxiang”。
注册：https：//ngrok.com/signup


如果您已经注册，请确保您的authtoken已安装。
您的authtoken可在您的仪表板上：https：//dashboard.ngrok.com


ERR_NGROK_305
```


---
# 显式指定配置文件位置

```
ngrok http -config=/conf/ngrok.yml 8000

```
- 指定具有项目特定覆盖的附加配置文件

```
ngrok start -config ~/ngrok.yml -config ~/projects/example/ngrok.yml demo admin

```
# 默认配置文件位置
`ngrok会尝试从默认位置$HOME/.ngrok2/ngrok.yml`
> 注意：$ HOME是操作系统定义的当前用户的主目录。 它不是环境变量$ HOME
```
OS X /Users/example/.ngrok2/ngrok.yml
Linux /home/example/.ngrok2/ngrok.yml
Windows C:\Users\example\.ngrok2\ngrok.yml
```
既 `C:\Users\Administrator\ .ngrok2\ngrok.yml `


- 定义两个名为“httpbin”和“demo”的隧道
```
tunnels:
  httpbin:
    proto: http
    addr: 8000
    subdomain: alan-httpbin
  demo:
    proto: http
    addr: 9090
    hostname: demo.inconshreveable.com
    inspect: false
    auth: "demo:secret"
```
- 启动隧道名为“httpbin”
```
ngrok start httpbin
```
- 从配置文件启动三个命名的隧道
```
ngrok start admin ssh metrics
```
- 启动配置文件中定义的所有隧道
```
ngrok start --all
```
---
`ngrok中文版文档`
http://ngrok.cn/docs.html#config-location



---
# 国内版
`NATAPP基于ngrok的国内高速内网穿透服务` `★`

https://natapp.cn/


`Ngrok国内免费服务器——糖果科技`  `免费`

http://www.qydev.com/


`Sunny-Ngrok内网转发`

https://www.ngrok.cc/



`ngrok国内服务器 | ITTUN`
http://www.ittun.com/


`魔法隧道 | 内网穿透 | 花生壳 | nat123 | ngrok | frp | 网络穿透 | 穿透技术 | 穿透内网`
http://mofasuidao.cn/



`路由侠-局域网变公网`
http://www.luyouxia.com/


`内网IP使用ngrok穿透的方法（小白系列一） - 免流相关版面 - 恩山无线论坛 - Powered by Discuz!`
http://www.right.com.cn/forum/thread-192357-1-1.html


---
**参考**
`使用ngrok让微信公众平台通过80端口访问本机 - liuxiyangyang的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/liuxiyangyang/article/details/22922265


`内网映射公网利器ngrok Windows下配置及使用教程`
http://blog.csdn.net/lipc_/article/details/52012642


`国内 ngrok 内网 穿透 服务 有和 www.tunnel.mobi 类似网站吗? - 知乎`
https://www.zhihu.com/question/38150862
