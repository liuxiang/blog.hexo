title: linux-nginx 安装&维护
date: 2016-06-25 00:00:00
tags: [linux,nginx]


---
# yum安装
```
yum list zlib

yum install zlib

```


---
# nginx依赖
- 下载模块依赖性Nginx需要依赖下面3个包
1.gzip 模块需要 zlib 库 ( 下载: http://www.zlib.net/ )
2.rewrite 模块需要 pcre 库 ( 下载: http://www.pcre.org/ )
3.ssl 功能需要 openssl 库 ( 下载: https://www.openssl.org/source/ ) openssl-fips符合FIPS标准Openssl 联邦信息处理标准


```
wget  http://zlib.net/zlib-1.2.8.tar.gz
wget  ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz
wget https://www.openssl.org/source/openssl-fips-2.0.13.tar.gz
```
```
wget http://nginx.org/download/nginx-1.11.6.tar.gz
```


---
# 编译器安装
```
yum install -y gcc gcc-c++

```


# 系列安装



## openssl ：
依赖
```
yum install -y perl

or
https://www.perl.org/get.html   linux => download

```
安装
```
[root@localhost] tar -zxvf openssl-fips-2.0.13.tar.gz
[root@localhost] cd  openssl-fips-2.0.13
[root@localhost] ./config && make && make install
```
```
yum list  openssl

yum install  openssl

```


# pcre:
```
[root@localhost] tar -zxvf  pcre-8.39.tar.gz
[root@localhost] cd pcre-8.39
[root@localhost] ./configure && make && make install
```
```
yum list  pcre

yum install  pcre

```


# zlib:
```
[root@localhost] tar -zxvf  zlib-1.2.8.tar.gz
[root@localhost] cd zlib-1.2.8
[root@localhost] ./configure && make && make install
```
```
yum list zlib

yum install zlib

```


# 最后安装nginx
``` 
tar -zxvf nginx-1.11.6.tar.gz
cd nginx-1.11.6
./configure &&   make && make install
```
```
yum list  nginx

yum install  nginx

```

默认位置: `/usr/local/nginx`


## 或：指定目录安装
``` 

tar -zxvf nginx-1.11.6.tar.gz
cd nginx-1.11.6
./configure --prefix=/home/nginx/nginx # 更换nginx安装路径
make && make install
```


---
**参考**
`Nginx安装 – 运维与架构`
http://www.nginx.cn/


`Linux 安装Nginx详细图解教程 - grhlove123的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/grhlove123/article/details/47834673



---

# 常见问题
-  ./configure: error: C compiler cc is not found
```
yum -y install gcc

```
http://www.2cto.com/os/201408/329177.html


---
-  linux下安装安装pcre-8.32 configure: error: You need a C++ compiler for C++ support
```
yum install -y gcc gcc-c++

```

http://blog.sina.com.cn/s/blog_7253980a0101guiz.html


---
- nginx执行` ./configure `
```
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```
`rewrite 模块需要 pcre 库`


-  执行`nginx -t`
/usr/local/nginx/sbin/nginx: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory

`ln -s /usr/local/lib/libpcre.so.1 /lib64`


**参考**
`error while loading shared libraries: libpcre.so.1:解决方法 | 爱积累爱分享`
http://www.iitshare.com/error-while-loading-shared-libraries-libpcre-so-1.html


`Nginx常见错误与解决方法_百度文库`
http://wenku.baidu.com/link?url=VU2Dbvjke-tDdDuuRo0kr3EphcO5Eesi23YY8nkYagdg_QAZpGcjS9VogLdS_rPhJj8wKR8uJLru8ZB83dSjF-Ofnuudkm3ZTL08Y5_j6Fy


---
# 部署
```
# 服务部署
souceCode 同步至  /usr/local/nginx/html
# 转发配置
vi /usr/local/nginx/conf/nginx.conf
```


---
# nginx 维护

- 校验配置
`/usr/local/nginx/sbin/nginx -t`

或  /usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
```
[root@iZ23j23f6aaZ sbin]# /usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```
- 启动
/usr/local/nginx/sbin/nginx


- 状态
ps -A | grep nginx

- 重启
`/usr/local/nginx/sbin/nginx -s reload`
```
service nginx restart （Permission denied 拒绝访问）

kill -HUP `cat /usr/local/nginx/logs/nginx.pid`  （效果未知,待确认）

```


- 停止
/usr/local/nginx/sbin/nginx  -s stop

kill  `cat /usr/local/nginx/logs/nginx.pid`



---
`nginx启动、重启、关闭 - jian_xie - 博客园`
http://www.cnblogs.com/jianxie/p/3990377.html


`nginx代理服务器安装 | lkoky888`
http://fromwiz.com/share/s/0_NRH32B-h7w2OHAEQ0D9QUj1WgccU02BkgA2OEmfS2kMoLJ


---
# 自定义命令
file：`root/cfg/cfg`
```
# start > nginx
# stop     > nginx -s stop
# reload> nginx -s reload
# test    > nginx -t
alias nginx='/usr/local/nginx/sbin/nginx'
alias nginxid='cat /usr/local/nginx/logs/nginx.pid' # 如机器重启后,因使用 ps -ef | grep nginx
```
```
ps -ef | grep nginx # 查看 nginx进程

ps -A | grep nginx # 查看nginx状态

kill  `cat /usr/local/nginx/logs/nginx.pid` # 杀进程，停止

```


# 非root，因使用sudo  +绝对程序路径
```
nginxid
sudo /usr/local/nginx/sbin/nginx -t
sudo /usr/local/nginx/sbin/nginx -s stop
sudo /usr/local/nginx/sbin/nginx
nginxid

```


<!-- more -->
