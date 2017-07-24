title: https,网络分层
date: 2016-12-16 00:00:04
tags: [https,网络分层]

---


# 网络分层
网络层次可划分为五层因特网协议栈和七层因特网协议栈
`系统分析师教程 2010.pdf-第四章  数据通信与计算机网络 -网络体系结构与协议`



| 5层    | 7层(OSI/RM ) |                                         | 4层(TCP/IP) |                    |
|--------|--------------|-----------------------------------------|-------------|--------------------|
| 应用层 | 应用         | 面向用户，机器与用户间的界面            | 应用层      | 同[应用+表示+会话] |
| 　     | 表示         | 语义化内容和含义                        |             |                    |
| 　     | 会话         | 一个终端用户与交互系统进行通讯的过程    |             |                    |
| 传输层 | 传输层       | 数据通信最高层/两台计算机通信可靠传输层 | 传输层      | 同[传输层]         |
| 网络层 | 网络层       | 通信子网最高层/路由选择                 | 网络互联层  | 同[网络层]         |
| 链路层 | 数据链路层   | 数据通道                                | 网络接口层  | 同[链路层+物理]    |
| 物理层 | 物理层       | bit流传输                               |             |                    |




- 协议栈（Protocol Stack）
```
应用层（HTTP，FTP，TFTP，TELNET，DNS，EMAIL等）
表示层（VTP）
会话层
传输层（TCP，UDP）
网络层（IP）
数据链路层（WI-FI，以太网，令牌环，FDDI，MAC等）
物理层[2] 
```


`网络分层_百度百科`
http://baike.baidu.com/item/%E7%BD%91%E7%BB%9C%E5%88%86%E5%B1%82


---
# Https简介
HTTPS（全称：Hyper Text Transfer Protocol over Secure Socket Layer），是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL




# HTTPS和HTTP的区别
场景 | http | https
-|-|
传输 | 明文 | 密文
证书 | 不需要 | ca申请证书(免费/收费)
协议 | http是超文本传输协议 | https 则是具有安全性的ssl加密传输协议
端口 | 80 | 443
状态 | 无状态 | 由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议


`https_百度百科`
http://baike.baidu.com/item/https


---
# https通信过程
- https的实现原理
有两种基本的加解密算法类型：
1）对称加密：密钥只有一个，加密解密为同一个密码，且加解密速度快，典型的对称加密算法有DES、AES等；
2）非对称加密：密钥成对出现（且根据公钥无法推知私钥，根据私钥也无法推知公钥），加密解密使用不同密钥（公钥加密需要私钥解密，私钥加密需要公钥解密），相对对称加密速度较慢，典型的非对称加密算法有RSA、DSA等。


- https通信的优点
1）客户端产生的密钥只有客户端和服务器端能得到；
2）加密的数据只有客户端和服务器端才能得到明文；
3）客户端到服务端的通信是安全的。


`深入理解http/https协议`
http://www.mamicode.com/info-detail-1631526.html


`openssl基本原理 + 生成证书 + 使用实例 - oldmtn的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/oldmtn/article/details/52208747


`Https单向认证和双向认证 - duanbokan的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/duanbokan/article/details/50847612

---


openssl、x509、crt、cer、key、csr、ssl、tls 这些都是什么鬼?

http://www.cnblogs.com/yjmyzz/p/openssl-tutorial.html


`那些证书相关的玩意儿(SSL,X.509,PEM,DER,CRT,CER,KEY,CSR,P12等)`
http://www.cnblogs.com/guogangj/p/4118605.html



---
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/448896-file_1484817404992_914a.png)
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/46324672-file_1484817412376_123a9.png)
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/14880190-file_1484817424981_12d7f.png)


`苹果强制使用HTTPS传输了怎么办？——关于HTTPS，APP开发者必须知道的事 - 推酷`
http://www.tuicool.com/articles/nya6BbM


---
`记录启用https的全过程 - 推酷`
http://www.tuicool.com/articles/ZVjMZ3Y
 
# 密钥，CA证书
- SSL证书分为四大类
    - DV证书:DV证书即Domain Validation Certificate
    - OV证书:OV证书即Organization Validation CErtificate
    - EV证书:EV证书即Extended Validation Certificate 
    - 自签名证书: 自签名证书很少被部署到正式的网站上，一般是被用在内部的测试环境中. (工具openssl)
https://github.com/michaelliao/itranswarp.js/blob/master/conf/ssl/gencert.sh


`给Nginx配置一个自签名的SSL证书 - 推酷`
http://www.tuicool.com/articles/VNj2um


`更多：自动shell脚本 gencert.sh`
https://github.com/search?utf8=%E2%9C%93&q=Enter+your+domain+filename%3Agencert.sh&type=Code&ref=searchresults


`为满足微信小程序 HTTPS 的要求，我们在部署 HTTPS 网站时，该如何选择 SSL 证书？ - 推酷`
http://www.tuicool.com/articles/3yaqqiN


`利用腾讯云免费证书打造全https站 - 推酷`
http://www.tuicool.com/articles/rMNRVrr


`https 免费证书获取指引 - 推酷`
http://www.tuicool.com/articles/z2m2auZ
- 1, 通过 let's encrypt
- 2, 腾讯云免费 ssl 证书申请
- 3, CDN 服务商一键提供


- 证书校验的几种方式：
    - 1, 邮箱验证 admin@cnodejs.org
    - 2, 文件校验 http:// cnodejs.org/5d41402abc4 b2a76b9719d911017c592.html
    - 3, dns 校验 http:// 5d41402abc4b2a76b9719d911017c592.cnodejs.org 的域名 cname 到某个地址


`七牛云推出SSL证书免费服务 并宣布HTTPS调价 - 推酷`
http://www.tuicool.com/articles/M73EvqA


---
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/64482030-file_1484817437675_9d43.png)
`如何快速上线全站 HTTPS - 推酷`
 http://www.tuicool.com/articles/MneaAbj
 

