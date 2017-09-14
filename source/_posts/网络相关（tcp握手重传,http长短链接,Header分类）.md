title:  网络相关（tcp握手重传,http长短链接,Header分类）

date: 2017-07-05 00:00:00
tags: [网络 ]



---

# TCP
特点：三次握手 /  四次挥手 /  可靠连接 /  丢包重传
核心的： tcp是可以可靠传输协议，它的所有特点都为这个可靠传输服务


---
位码即tcp标志位，有6种标示：
`SYN(synchronous建立联机)` 
`ACK(acknowledgement 确认)`
`PSH(push传送)`
`FIN(finish结束)`
`RST(reset重置)` 
`URG(urgent紧急)`


`Sequence number(顺序号码)`
`Acknowledge number(确认号码)`

---
ack=seq+`len`
ack总是seq+`len（包的大小）`，这样发送方明确知道server收到那些东西了。
但是特例是三次握手和四次挥手，虽然len都是0，但是`syn和fin都要占用一个seq号`，`所以这里的ack都是seq+1`


---


那么tcp是怎么样来保障可靠传输呢？

tcp在传输过程中都有一个ack，接收方通过ack告诉发送方收到那些包了。这样发送方能知道有没有丢包，进而确定重传。

#  tcp建连接的三次握手

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-5/44504274.jpg)




- 三个红框表示建立连接的三次握手：
第一步：client 发送 `syn` 到server 发起握手；`client-seq随机数`
第二步：server 收到 syn后回复`syn+ack`给client； `server-seq随机数``ack确认（client seq）`
第三步：client 收到`syn+ack`后，回复server一个`ack表示收到`了server的`syn`（此时client的`48287端口`的连接已经是`established`） `server-ack`


握手的`核心目的`是告知对方seq（绿框是client的初始seq，蓝色框是server 的初始seq），对方回复ack（收到的seq+包的大小），这样发送端就知道有没有丢包了。



握手的`次要目的`是告知和`协商一些信息`，图中黄框。
`MSS–最大传输包`
`SACK_PERM–是否支持Selective ack(用户优化重传效率）`
`WS–窗口计算指数`（有点复杂的话先不用管）



**这就是tcp为什么要握手建立连接，就是为了解决tcp的可靠传输。**


# tcp断开连接的四次挥手
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-5/1347664.jpg)
 
- 四个红框表示断开连接的四次挥手：
第一步： client主动发送`fin`包给server
第二步： server回复ack（对应第一步fin包的ack）给client，表示`server知道client要断开了`
第三步： server发送`fin`包给client，表示`server也可以断开了`
第四部： client回复`ack`给server，表示既然`双发都发送fin包表示断开`，`那么就真的断开吧`

---

# 三次握手中协商的其它信息


`MSS 最大一个包中能传输的信息`（不含tcp、ip包头）
`MSS+包头就是MTU（最大传输单元）`，如果`MTU过大可能在传输的过程中被卡住过不去造成卡死`（这个大小的包一直传输不过去），跟丢包还不一样。

---



| 请求的三次握手 | 端到端 | 目的 | 内容 |
|:--------------:|----------------|------------------------------------------------------------------------|--------------------------------------------|
| 第一次 | Client->Server | 1.建立连接+丢包保障(包大小) <br> 2.协商:MSS最大传输包+SACK_perm重传效率 | 1.[SYN]seq=* len=* <br>2.MSS=* SACK_perm=* |
| 第二次 | Server->Client | 收到&并确认 | [SYN,ACK]seq=* ACK=c-seq+len len=* MSS=* |
| 第三次 | Client->Server | 收到&并确认 | [SYN,ACK]seq=* ACK=s-seq |


| 断开的四次握手 | 端到端         | 目的           | 内容      |
|:--------------:|----------------|----------------|-----------|
| 第一次         | Client->Server | client请求断线 | [FIN,ACK] |
| 第二次         | Server->Client | 收到&并确认    | [ACK]     |
| 第三次         | Server->Client | server请求断线 | [FIN,ACK] |
| 第四次         | Client->Server | 收到&并确认    | [ACK]     |




---
# 重传策略
场景:client[1,2,3,4,5] server[1,2,4,5] `数据包3`丢了


##  超时重传机制: client没有收到server回复的数据包3的ack,超时就重传

` 关于建连接时SYN超时`  在Linux下，默认重试次数为5次，重试的间隔时间从1s开始每次都翻售，5次的重试时间间隔为1s, 2s, 4s, 8s, 16s，总共31s，第5次发出后还要等32s都知道第5次也超时了，所以，总共需要 1s + 2s + 4s+ 8s+ 16s + 32s = 2^6 -1 = 63s，TCP才会把断开这个连接
`关于SYN Flood攻击` 一些恶意的人就为此制造了SYN Flood攻击——给服务器发了一个SYN后，就下线了，于是服务器需要默认等63s才会断开连接，这样，攻击者就可以把服务器的syn连接的队列耗尽，让正常的连接请求不能处理。于是，Linux下给了一个叫tcp_syncookies的参数来应对这个事——当SYN队列满了后，TCP会通过源地址端口、目标地址端口和时间戳打造出一个特别的Sequence Number发回去（又叫cookie），如果是攻击者则不会有响应，如果是正常连接，则会把这个 SYN Cookie发回来，然后服务端可以通过cookie建连接（即使你不在SYN队列中）。

## 快速重传机制
于是，TCP引入了一种叫Fast Retransmit 的算法，不以时间驱动，而以数据驱动重传。也就是说，如果，包没有连续到达，就ack最后那个可能被丢了的包，如果发送方连续收到3次相同的ack，就重传。Fast Retransmit的好处是不用等timeout了再重传


## SACK 方法 [` 数据碎版` ]
另外一种更好的方式叫：Selective Acknowledgment (SACK)（参看RFC 2018），这种方式需要在TCP头里加一个SACK的东西，ACK还是Fast Retransmit的ACK，SACK则是汇报收到的数据碎版
这样，在发送端就可以根据回传的SACK来知道哪些数据到了，哪些没有到。于是就优化了Fast Retransmit的算法。当然，这个协议需要两边都支持。在 Linux下，可以通过tcp_sack参数打开这个功能（linux 2.4后默认打开）。


![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-5/6094124.jpg)


这里还需要注意一个问题——接收方`Reneging`，所谓Reneging的意思就是接收方有权把已经报给发送端`SACK里的数据给丢了`。这样干是不被鼓励的，因为这个事会把问题复杂化了，但是，接收方这么做可能会有些极端情况，比如要把内存给别的更重要的东西。所以，发送方也不能完全依赖SACK，还是要依赖ACK，并维护Time-Out，如果后续的ACK没有增长，那么还是`要把SACK的东西重传`，另外，接收端这边永远不能把SACK的包标记为Ack。



注意：`SACK会消费发送方的资源`，试想，如果一个攻击者给数据发送方发`一堆SACK的选项`，这会导致发送方开始要重传甚至遍历已经发出的数据，这会消耗很多发送端的资源。详细的东西请参看《TCP SACK的性能权衡》


# Duplicate SACK – 重复收到数据的问题
Duplicate SACK又称D-SACK，其主要`使用了SACK来告诉发送方有哪些数据被重复接收了`


示例一：ACK丢包 `[ACK( 4000) > SACK( 3000-3500) ]`
下面的示例中，丢了两个ACK，所以，发送端重传了第一个数据包（3000-3499），于是接收端发现重复收到，于是回了一个`SACK=3000-3500`，因为`ACK都到了4000`意味着收到了4000之前的所有数据，所以这个SACK就是D-SACK——`旨在告诉发送端我收到了重复的数据`，而且我们的发送端
还知道，数据包没有丢，丢的是ACK包。


| Transmitted     Segment | Received     Segment | ACK Sent     (Including SACK Blocks) |
|:-----------------------:|----------------------|--------------------------------------|
| 3000-3499               | 3000-3499            | 3500 (ACK dropped)                   |
| 3500-3999               | 3500-3999            | 4000 (ACK dropped)                   |
| 3000-3499               | 3000-3499            | 4000, SACK=3000-3500                 |


示例二，网络延误 `[ACK(3000) > SACK(1000-1500)]` (+ACK重复,SACK碎片补偿)
下面的示例中，网络包（1000-1499）被网络给延误了，导致发送方没有收到ACK，而后面到达的三个包触发了“Fast Retransmit算法”，所以重传，但重传时，被延误的包又到了，所以，回了一个`SACK=1000-1500`，因为`ACK已到了3000`，所以，`这个SACK是D-SACK——标识收到了重复的包`。
这个案例下，发送端知道之前因为“Fast Retransmit算法”触发的重传不是因为发出去的包丢了，也不是因为回应的ACK包丢了，而是因为网络延时
了。


| Transmitted     Segment | Received     Segment | ACK Sent     (Including SACK Blocks) |
|:-----------------------:|----------------------|--------------------------------------|
| 500-999                 | 500-999              | 1000                                 |
| 1000-1499               | (delayed)            |                                      |
| 1500-1999               | 1500-1999            | 1000, SACK=1500-2000                 |
| 2000-2499               | 2000-2499            | 1000, SACK=1500-2500                 |
| 2500-2999               | 2500-2999            | 1000, SACK=1500-3000                 |
| 1000-1499               | 1000-1499            | 3000                                 |
|                         | 1000-1499            | 3000, SACK=1000-1500                 |


`引入了D-SACK，有这么几个好处`：
1）可以让发送方知道，是发出去的包丢了，还是回来的ACK包丢了。
2）是不是自己的timeout太小了，导致重传。
3）网络上出现了先发的包后到的情况（又称reordering）
4）网络上是不是把我的数据包给复制了。
`Linux下的tcp_dsack参数用于开启这个功能（Linux 2.4后默认打开）`


---
# TCP(HTTP)长连接和短连接区别和怎样维护长连接
在`HTTP/1.0中，默认使用的是短连接`。也就是说，浏览器和服务器每进行一次HTTP操作，就建立一次连接，但任务结束就中断连接
从 `HTTP/1.1起，默认使用长连接`，用以保持连接特性。使用长连接的HTTP协议，会在`响应头`有加入这行代码： `Connection:keep-alive`


`模拟一下长连接的情况`，client向server发起连接，server接受client连接，双方建立连接。Client与server完成一次读写之后，它们之间的连接并不会主动关闭，后续的读写操作会继续使用这个连接

首先说一下TCP/IP详解上讲到的TCP保活功能，保活功能主要为服务器应用提供，服务器应用希望知道客户主机是否崩溃，从而可以代表客户使用资源。如果客户已经消失，使得服务器上保留一个半开放的连接，而服务器又在等待来自客户端的数据，则服务器将`永远等待客户端的数据`，`保活`功能就是`试图在服务器端检测到这种半开放的连接`。

- tomcat/conf/service.xml
```
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
```


--- #  HTTP 协议格式 和 HTTP Header

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-5/90636666.jpg)

 

请求字段(Request)
能够接受


` DevDocs — HTTP / Headers `

http://devdocs.io/http-headers/


** 请求字段(Request) **


| Header     | 描述                                                    | 示例                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Accept-?   | 自 方 支持说明 <br> (类型,字符集,编码,语言,时间版本) | Accept-Charset: utf-8     <br>Accept-Encoding:gzip, deflate     <br>Accept-Language: en-US     <br>Accept-Datetime: Thu, 31 May 2007 20:35:00 GMT     <br>TE: trailers, deflate                                                                                                                                                                                                                                                                                                                                                                                                      |
|            | 告知对方(安全,cookie,要求匹配,跨域)                   | Authorization: Basic   QWxhZGRpbjpvcGVuIHNlc2FtZQ==     <br>Cookie: $Version=1; Skin=new;     <br>Cache-Control: no-cache     <br>If-Match: "737060cd8c284d8af7ad3082f209582d"     <br>If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT     <br>If-None-Match: "737060cd8c284d8af7ad3082f209582d"     <br>If-Range: "737060cd8c284d8af7ad3082f209582d"     <br>If-Unmodified-Since: Sat, 29 Oct 1994 19:43:31 GMT     <br>Origin: http://www.example-social-network.com                                                                                                              |
| Connection | 连接预期                                                | Connection:   keep-alive     <br>Connection: Upgrade                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Content-?  | 交换信息(长度,编码,类型)                            | Content-Length:   348     <br>Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==     <br>Content-Type: application/x-www-form-urlencoded                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|            | 自方信息 <br> (时间,期望,请求方信息,目标域,设备,代理)  | Date: Tue, 15 Nov 1994 08:12:31   GMT     <br>Expect: 100-continue     <br>From: user@example.com     <br>Host:en.wikipedia.org:80     <br>Host: en.wikipedia.org     <br>Referer: http://en.wikipedia.org/wiki/Main_Page     <br>User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0)   Gecko/20100101 Firefox/21.0     <br>     <br>Max-Forwards: 10     <br>Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==     <br>Via: 1.0 fred, 1.1 example.com (Apache/1.1)     <br>     <br>Range: bytes=500-999     <br>Pragma: no-cache     <br>Warning: 199 Miscellaneous warning |

**响应 字段(Response) **



| Header   | 说明                         | 示例                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Access-? | 自 方 支持说明(域,编码,动作) | Access-Control-Allow-Origin: *     <br>Accept-Patch: text/example;charset=utf-8     <br>Accept-Ranges: bytes     <br>Allow: GET, HEAD                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 　       | 缓存相关                     | Age: 12     <br>Cache-Control: max-age=3600                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 　       | 连接预期                     | Connection: close                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 　       | 交换信息(内容,状态)          | Content-Disposition:   attachment; filename="fname.ext"     <br>Content-Encoding: gzip     <br>Content-Language: da     <br>Content-Length: 348     <br>Content-Location: /index.htm     <br>Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==     <br>Content-Range: bytes 21010-47021/47022     <br>Content-Type: text/html; charset=utf-8     <br>Status: 200 OK                                                                                                                                                                                                         |
| 　       | 自 方信息<br>(时间,tag版本,失效时,代理,服务容器)  | Date:   Tue, 15 Nov 1994 08:12:31 GMT     <br>ETag: "737060cd8c284d8af7ad3082f209582d"     <br>Expires: Thu, 01 Dec 1994 16:00:00 GMT     <br>Last-Modified: Tue, 15 Nov 1994 12:45:26 GMT     <br>Link: </feed>; rel="alternate"     <br>Location: http://www.w3.org/pub/WWW/People.html     <br>Pragma: no-cache     <br>Proxy-Authenticate: Basic     <br>Public-Key-Pins: max-age=2592000; pin-<br>sha256="E9CZ9INDbd+2eRQozYqqbQ2yXLVKB9+xcprMF+44U1g=";     <br>Refresh: 5; url=http://www.w3.org/pub/WWW/People.html     <br>Server: Apache/2.4.1 (Unix) |
| 　       |  告知对方<br> (cookie,传输, 重定向, 协议头,途径要求,警告,认证模式 )     | Set-Cookie:   UserID=JohnDoe; Max-Age=3600; Version=1 <br>Location: http://www.w3.org/pub/WWW/People.html <br>Transfer-Encoding: chunked     <br>Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11     <br>Vary: *     <br>Via: 1.0 fred, 1.1 example.com (Apache/1.1)     <br>Warning: 199 Miscellaneous warning     <br>WWW-Authenticate: Basic     <br>X-Frame-Options: deny                                                                                                                                                                                                                                    |

完整见： http://www.jianshu.com/p/b963643ffe72


---
**参考**
`就是要你懂 TCP` -  ` 什么是工程效率，什么是知识效率`


http://www.tuicool.com/articles/rUzEJ3


` 关于TCP 半连接队列和全连接队列 | 阿里中间件团队博客 `
http://jm.taobao.org/2017/05/25/525-1/
`另外每个具体问题都是最好学习的机会，光看书理解肯定是不够深刻的，请珍惜每个具体问题，碰到后能够把来龙去脉弄清楚。`


` tcp/ip 上，丢包重传机制 - DBC12345666的专栏 - 博客频道 - CSDN.NET `
http://blog.csdn.net/dbc12345666/article/details/42499889


` TCP(HTTP)长连接和短连接区别和怎样维护长连接 - jayxu无捷之径的博客 - 博客频道 - CSDN.NET `
http://blog.csdn.net/ls5718/article/details/51757467


`HTTP 协议格式 和 HTTP Header`
www.tuicool.com/articles/jMFfIv


`HTTP Header简介 - 简书`
http://www.jianshu.com/p/b963643ffe72
http://kb.cnblogs.com/page/92320/


