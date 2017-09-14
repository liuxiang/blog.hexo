title: x-www-form-urlencoded与form-data区别
date: 2016-1-21 00:00:00 #发表日期，一般不改动
categories: 网络   #文章文类
tags: [ 网络,http ]


---
# application/x-www-form-urlencoded和multipart/form-data的区别 ## application/x-www-form-urlencoded

```
General

Remote Address:192.168.3.21:8080
Request URL:http://192.168.3.21:8080/k12/service/questionBank/randomSumQuestion/10
Request Method:POST
Status Code:200 OK


Response Headers
view source
Access-Control-Allow-origin:*
Content-Type:application/json;charset=utf-8
Date:Thu, 21 Jan 2016 10:34:50 GMT
Server:Apache-Coyote/1.1
Transfer-Encoding:chunked


Request Headers
view source
Accept:*/*
Accept-Encoding:gzip, deflate
Accept-Language:zh-CN,zh;q=0.8
Cache-Control:no-cache
Connection:keep-alive
Content-Length:6
Content-Type:application/x-www-form-urlencoded
Host:192.168.3.21:8080
Origin:chrome-extension://mkhojklkhkdaghjjfdnphfphiaiohkef
User-Agent:Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36


Form Data
view source
view URL encoded
code:1
```


## multipart/form-data
```
General

Remote Address:192.168.3.21:8080
Request URL:http://192.168.3.21:8080/k12/service/questionBank/randomSumQuestion/10
Request Method:POST
Status Code:200 OK


Response Headers
view source
Access-Control-Allow-origin:*
Content-Type:application/json;charset=utf-8
Date:Thu, 21 Jan 2016 10:38:32 GMT
Server:Apache-Coyote/1.1
Transfer-Encoding:chunked


Request Headers
view source
Accept:*/*
Accept-Encoding:gzip, deflate
Accept-Language:zh-CN,zh;q=0.8
Cache-Control:no-cache
Connection:keep-alive
Content-Length:227
Content-Type:multipart/form-data; boundary=----WebKitFormBoundaryEBMAXATV8VmHR4yq
Host:192.168.3.21:8080
Origin:chrome-extension://mkhojklkhkdaghjjfdnphfphiaiohkef
User-Agent:Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36


Request Payload
------WebKitFormBoundaryEBMAXATV8VmHR4yq
Content-Disposition: form-data; name="code"


1
------WebKitFormBoundaryEBMAXATV8VmHR4yq
Content-Disposition: form-data; name="xxx"


2
------WebKitFormBoundaryEBMAXATV8VmHR4yq--
```


# 小结论
`application/x-www-form-urlencoded` 适合简单键值文字参数

` multipart/form-data ` 适合大批量数据和文件附件上传


# 技巧
使用chrome插件`Advanced REST client`,`Postman - REST Client`,`DHC`
测试,同时开启F12观察`network`,获取报文内容


# 技巧升级

对于不可F12的chrome插件.如`postman` ,可以通过一个特别的入口进入查看
chrome://inspect/#apps
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/49600881.jpg)


**参考**
`Content-Type: multipart/form-data`
http://my.oschina.net/xinxingegeya/blog/395484


`关于 Content-Type:application/x-www-form-urlencoded 和 Content-Type:multipart/rel_哖少_新浪博客`
http://blog.sina.com.cn/s/blog_b2fa1f6a01012n38.html


---
# 关系文章
post[form-data 与 x-www-form-urlencoded区别]-无参数.md

post[form-data 与 x-www-form-urlencoded区别]-有参数.md

application/x-www-form-urlencoded和multipart/form-data的区别.md


<!-- more -->
