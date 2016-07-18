title: nginx 域名配置策略讨论
date: 2016-06-15 00:00:00
categories: nginx

tags: [nginx ]


---


# 方式一
- 根域名（ wosaitech.com ）转发到`html/wosai`,预留其它同级目录如“womao，weixin...”
- 子集服务（ wosaitech.com/zhaoping ）转发到'html/zhaoping'
    - 好处：wosai属于域名官网项目，zhaoping属于独立项目，分开部署清晰

    - 好处：ip访问与域名访问，后缀一致，在维护中方便更替。支持变化域名后，ip同时保持服务。

    - 坏处：每增加子集服务（html/*）都必须增加配置，否则会被基础转发引导到`html/wosai/*`中寻找，找不到就出错
```
   server {
        listen       80;
        server_name  wosaitech.com www.wosaitech.com;


        #charset koi8-r;
        #access_log  logs/host.access.log  main;


        # 所有('/'开头)访问
        location / {
            root   html/wosai;
            index  index.html index.htm;
        }


        # 由于域名访问将根“/”目录转发到了其它位置(html/wosai).
        # 导致所有域名下的服务都需要放置域名转发目录(html/wosai)下,否则无法访问
        # 规避办法：未具体服务(*)进行再次转发(html/*)
location /education{
proxy_pass http://127.0.0.1/education;
}
location /wosaiIntro{
proxy_pass http://127.0.0.1/wosaiIntro;
}
location /zhaoping{
proxy_pass http://127.0.0.1/zhaoping;
}
location /apk{
proxy_pass http://127.0.0.1/apk;
}
location /teach{
proxy_pass http://127.0.0.1/teach;
}
     }
```


# 方式二(`讨论结论采用此方式`)

- 基于`方式一`思考，将 子集服务（ wosaitech.com/zhaoping ），正常部署到`html/wosai/zhaoping`中
    - 好处： 每增加子集服务（html/wosai/*）都不需要增加配置维护。减少风险

    - 坏处：与子集目录同级的`wosai`官网项目的文件目录，会形成干扰。混淆视听
    -  坏处：遇到`wosai`官网项目的升级，要注意隐性的子集目录，及时还原。慎重清理



<!--
     - 坏处(此条目忽略)：代码中配置的访问路径`wosaitech.com/zhaoping`需要修改为`wosaitech.com/wosai/zhaoping`

-->



- 增加二级域名(weixin.wosaitech.com)的独立转发配置（ html/teach/ ）
     - 好处：足够独立的子集服务(weixin.wosaitech.com)，放在`html/wosai/teach`是不太合适的。方便管理
```

   server {
        listen       80;
        server_name  wosaitech.com www.wosaitech.com;


        #charset koi8-r;
        #access_log  logs/host.access.log  main;


        # 所有('/'开头)访问
        location / {
            root   html/wosai;
            index  index.html index.htm;
        }
     }
   server {
        listen       80;
        server_name  weixin.wosaitech.com;


        #charset koi8-r;
        #access_log  logs/host.access.log  main;


        # 所有('/'开头)访问
        location / {
            root   html/teach/;
            index  index.html index.htm;
        }
     }
```


# 方式三

- 基于`方式一`的坏处思考，将 子集服务（ wosaitech.com/zhaoping ），也独立`公共`二级域名，可以避免
    - 好处：" 与子集目录同级的`wosai`官网项目的文件目录形成干扰 " 不会存在，因为抽离出来了(html/public/zhaoping)
    - 坏处：不能使用` wosaitech.com/zhaoping `，而要用`public. wosaitech.com/zhaoping `
    -  坏处：在 ` wosaitech.com`与 `public. wosaitech.com/zhaoping `间交互时可能出现问题
```

   server {
        listen       80;
        server_name  wosaitech.com www.wosaitech.com;


        #charset koi8-r;
        #access_log  logs/host.access.log  main;


        # 所有('/'开头)访问
        location / {
            root   html/wosai;
            index  index.html index.htm;
        }
     }
   server {
        listen       80;
        server_name  public.wosaitech.com;


        #charset koi8-r;
        #access_log  logs/host.access.log  main;


        # 所有('/'开头)访问
        location / {
            root   html/ public /;
            index  index.html index.htm;
        }
     }
   server {
        listen       80;
        server_name  weixin.wosaitech.com;


        #charset koi8-r;
        #access_log  logs/host.access.log  main;


        # 所有('/'开头)访问
        location / {
            root   html/teach/;
            index  index.html index.htm;
        }
     }
```


<!-- more -->

