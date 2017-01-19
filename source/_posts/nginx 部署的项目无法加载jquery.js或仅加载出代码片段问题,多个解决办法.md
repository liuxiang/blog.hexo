title: nginx 部署的项目无法加载jquery.js或仅加载出代码片段问题,多个解决办法
date: 2016-07-22 00:00:00
tags: [linux, nginx ]


---


# 问题现象 `jquery.js`文件无法加载，导致页面样式错乱
- 前端浏览器体现：
`GET http://***/jquery.js net::ERR_INCOMPLETE_CHUNKED_ENCODING`


- 后端`tail -f /usr/local/nginx/logs/error.log`错误日志体现：
`nginx /usr/local/nginx/proxy_temp/* failed (13: Permission denied)`

```
2016/07/22 17:54:59 [crit] 4281#0: *441 open() "/usr/local/nginx/proxy_temp/2/00/0000000002" failed (13: Permission denied) while reading upstream, client: 115.197.165.164, server: , request: "GET /teach_pc/js/jquery-1.11.2.min.js HTTP/1.1", upstream: "http://127.0.0.1:8080/teach_pc/js/jquery-1.11.2.min.js", host: "121.40.195.52"

```


- 如后端并没有此提示或问题出在tomcat服务容器中，有一种可能是数据磁盘满了。
```
df -h # 查看磁盘占用情况
```
在任意目录 vi 写入内容，也会有提示`E514: write error (file system full?) `(文件系统已满)



- 问题原因
```
看一下nginx 反向代理参数说明
 
proxy_connect_timeout 600; #nginx跟后端服务器连接超时时间(代理连接超时)
proxy_read_timeout 600; #连接成功后，后端服务器响应时间(代理接收超时)
proxy_send_timeout 600; #后端服务器数据回传时间(代理发送超时)
proxy_buffer_size 32k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
proxy_buffers 4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers*2）
proxy_temp_file_write_size 64k; #设定缓存文件夹大小，大于这个值，将从upstream服务器传


问题就出在proxy_temp_file_write_size上，当你的文件超过该参数设置的大小时，nginx会先将文件写入临时目录(缺省为nginx安装目下/proxy_temp目录),
 
如果nginx对prxoy_temp没有权限就会写不进去，结果就是只显示部分页面
`` `
http://www.nginx.cn/695.html


- 正常不会出现此问题，造成原因是因为全局替换了文件所有者
```
chown -R root /usr/local/nginx
chown -R sysadmin /usr/local/nginx # 或此
```
> 此命令的-R会递归替换内部所有子文件（含 proxy_temp ）的所有者为root
> 而nginx默认是使用nobody用户启动，由此nobody用户将无法更新目录 proxy_temp下的文件了


---


- 解决办法一：更改 `proxy_temp/` 文件权限
    -  破坏了目录的安全管理策略，不建议

```
因为权限问题不能写入Nginx的缓存文件夹里，修改Nginx的缓存文件夹权限，问题解决！
chmod -R 777 proxy_temp/
```


-  解决方法二：更新nginx.conf中nobody用户为root
     -  破坏了目录的安全管理策略，不建议
```
发现都是nobody的进程，但是nginx的目录都是root用户，另外集群tomcat也是属于root用户，而且root启动，查看nginx.conf:
user nobody
改成：user root
停止nginx -s stop
重启nginx -c  nginx.conf
```
http://www.2cto.com/os/201203/122371.html


- 解决方法三：更新`proxy_temp`文件夹的所有者归还给`nobody`
     - 推荐
```
chown -R  nobody  /usr/local/nginx/ proxy_temp


# 更多影响到的其它文件
chown -R  nobody  /usr/local/nginx/ client_body_temp

chown -R  nobody  /usr/local/nginx/ fastcgi_temp

chown -R  nobody  /usr/local/nginx/ scgi_temp

chown -R  nobody  /usr/local/nginx/ uwsgi_temp

```


<!-- more -->
