title: nginx.conf 配置自动www前缀
date: 2016-12-10 00:00:00
tags: [ngin.conf]


---
# nginx.conf 配置自动www前缀(会改写地址栏) ```
server{
    listen       80;
    server_name  saixueba.com;
    location / {
             rewrite ^(.*)$ http://www.saixueba.com/$1 permanent;
        }
}
```
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/60383016-file_1484816953830_c781.png)

 
---
注意:部分浏览器会自动加www前缀(360极速浏览器),
**怎么分辨** 首发请求网络中就带有了www前缀,说明浏览器处理的.没有(如上)就是程序处理的.
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/86307962-file_1484816982497_68aa.png)



---
# 同时监听
```
 server {
        listen       80;
        server_name   saixueba.com    www.saixueba.com;


        # saixueba.com 
        location / {
            proxy_pass http://120.55.244.116/sxb_index/;
        }
}
```