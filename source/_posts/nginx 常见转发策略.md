title: nginx 常见转发策略
date: 2015-12-23 00:00:02 #发表日期，一般不改动
categories: nginx #文章文类
tags: [nginx]
 
---
 
# nginx 常见转发策略
```
        location / {
            root   html/wosai;
            index  index.html index.htm;
        }
 
                location /uncle {
                                proxy_pass http://backend;
                }
 
                location /ws_wxService{
                               #proxy_pass http://127.0.0.1:8080;
                               proxy_pass http://x.wosaitech.cn;
                }
 
                # 地址栏重写
                location /release{
                               #proxy_pass https://git.oschina.net/wosaitech/release;
                                rewrite ^(.*)$  https://git.oschina.net/wosaitech/release;
                }
```


# 无`www`域名，自动重定向到带`www`域名下。注意后缀要留
```
    server{
        listen       80;
        server_name  saixueba.com;
        location / {
             rewrite ^(.*)$ http://www.saixueba.com/$1 permanent;
        }
 
    }
```


# 负载均衡
位置: vim /usr/local/nginx/conf/nginx.conf
```
    #include host-conf/tomcat.conf;
    upstream tomcat{
        server 125.210.208.44:8080;
        server 125.210.208.44:28081;
    }
    server {
        listen       8081;
        server_name  tomcat;
 
         location / {
    proxy_pass http://tomcat;
         }
    }
 
或(Tengine):
upstream backend {
        server 127.0.0.1:8080;
        server 127.0.0.1:28080;
}
server {
            #程序类
                location / {
                                proxy_pass http://backend;
                }
} 
```


# 多server配置
```
server {
        listen       80;
        server_name  localhost;
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
 
        location / {
            root   html/;
            index  index.html index.htm;
        }
 
        ...
}
 
 
server {
        listen       80;
        server_name  hzwomao.com www.hzwomao.com;
 
        location /{
                        proxy_pass http://127.0.0.1:8099;
        }   
 
        ...
    }
 
server {
        listen       80;
        server_name  wosaitech.com www.wosaitech.com;
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
 
        location / {
            root   html/wosai;
            index  index.html index.htm;
        }
 
        ...
}
 
```
 
## 下载centos服务器配置
```
sz /usr/local/nginx/conf/nginx.conf
sz /usr/local/nginx/conf/host-conf/tomcat.conf
```
 
# 赠品
## 前端与后端交互,使用路径几种方式
* ${pageContext.request.contextPath}     `jsp可用` `js不可用` <font color=red>`遇到代码封装到js中EL表达式不可用`</font>
* window.document.location.href             `jsp不可用` `js可用` <font color=red>`遇到服务器转发配置是,会出错`</font>
* 相对路径 `jsp可用` `js可用`
>注意事项: 相对的前缀路径不是地址栏显示的路径而是后端打开的页面路径.
(如果系统是单页应用时,相对的路径都是index.html页面所在路径)
 
<!-- more -->