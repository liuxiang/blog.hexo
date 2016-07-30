title: linux nginx权限控制策略
date: 2016-07-21 00:00:03
tags: [linux, nginx ]


---

# 方式一：更改目录权限，开放`其它用户`访问
- 优点：可以提供给所有者(root)以外的用户进行维护
- 缺点：`其它用户`包含了定义的管理员用户和其它所有用户.存在安全隐患

```
chmod 777 nginx
```

# 方式二：更改`html`目录所有者 （推荐）
- 优点:区别于其它用户的权限,避免了权限公开
- 缺点:文件的原所有者(非root)将失去管理权.但可以还原所有者来归还权限
     - root用户具备ALL权限,可以操作其他用户的文件
```
sudo chown -R sysadmin /usr/local/nginx/html 
sudo chown -R  sysadmin: sysadmin   /usr/local/nginx /html  #  群组一并修改
```
- tomcat 同理
```
sudo chown -R sysadmin  /usr/local/apache-tomcat-*/webapps

```


# 方式三：更改目录安装位置，于维护用户的目录下
- 优点：由`nginx`专有用户 管理（前提依然需要更换文件所有者）
- 缺点：更像是`nginx`独有的服务，且在维护其它服务时会频繁的切换用户
```
./configure  --prefix=/home/nginx/nginx   



sudo chown -R sysadmin nginx/

sudo chown -R sysadmin: sysadmin  nginx/ #  群组一并修改
```
- 注意问题
`nginx 部署的项目无法加载jquery.js或加载出片段问题,多个解决办法.md   `



<!-- more -->
