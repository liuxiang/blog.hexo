title: ipv6
date: 2017-1-3 00:00:00
tags: [ipv6]


---
# 简介
网络地址转换(NAT)



# 网友
https://bbs.aliyun.com/read/284699.html?spm=5176.bbsr284699.0.0.CPENAv&page=2



```
目前来看，是只能申请IPv6的隧道地址，因为目前国内还没有普及IPv6的网络喔。


苹果的意思是，新提交的应用必须兼容IPV6-only协议，你现在有没有IPv6地址不重要，重要的是能在未来能直接通过IPv6连接


我的问题搞定了，我花钱买了vip dns 
看来我上次怀疑的是对的。 
以前我们是通过ip链接的，所以美国测试的能连上。 
然后用了域名，美国那边可能连不上这个垃圾dns服务器结果就没法解析。。。 
我看到阿里的vip dns有备注里“xxxxxx海外” ，估计免费的dns压根不支持海外。 
反正我花钱之后再次递交，第二天就好了！ 


个人建议，您用设置不同的解析记录来区分ipv4和ipv6的记录。
如 ipv6.yourdomain.com 解析到AAAA，www.yourdomain.com 解析普通的A记录。这俩个主机名同时在Web里绑定。这样，是否会好些？
```


# 问题
http://ipv6-test.com/validate.php


![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/40359048-file_1484818385165_1033c.png)

https://bbs.aliyun.com/read/290628.html 


![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/50457131-file_1484818397697_151a0.png)
https://bbs.aliyun.com/read/295314.html



# 示例
```
nslookup www.6box.cn dns64.6box.cn
```
# 赛学霸
```
C:\Users\Administrator>Nslookup www.saixueba.com dns64.6box.cn
DNS request timed out.
    timeout was 2 seconds.
服务器:  UnKnown
Address:  222.28.155.25


非权威应答:
名称:    www.saixueba.com
Addresses:  2001:da8:20d:400::792b:4235
          121.43.66.53
```
```
C:\Users\Administrator>Nslookup kexue.saixueba.com dns64.6box.cn
服务器:  UnKnown
Address:  222.28.155.25


非权威应答:
名称:    kexue.saixueba.com
Addresses:  2001:da8:20d:400::7837:f474
          120.55.244.116
```


- 购买海外云解析
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/80019689-file_1484818340912_10ae5.png)


---
# 处理
http://www.solve6.com/


- 方案一：使用教育网的NAT64+DNS64服务
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/97591629-file_1484818351454_c057.png)



 - 方案二：使用IPv6隧道
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/74309866-file_1484818362144_31c4.png)


# 文章
`转给您身边苹果开发者，IPv6被拒如何破？`

http://mp.weixin.qq.com/s?plg_nld=1&plg_uin=1&mid=2247483863&idx=1&plg_nld=1&scene=23&plg_auth=1&__biz=MzI0NTI4ODA2Mw%3D%3D&plg_dev=1&srcid=0701GkquSx0l7Z6CchTAHFFn&plg_usr=1&plg_vkey=1&sn=622478d2775c5f8835fe1ed16964552e#rd



`IPv6审核被拒解决方案 - 你的IPv6审核专家`
http://www.solve6.com/
 


---
# mac 搭建ipv6环境
```
第一步:通过数据线连接iphone和mac
第二步:打开iphone的个人热点并选择仅USB
第三步：打开网络偏好设置,确保你的Mac的Wi-Fi是打开的,并且没有连接任何网络
第四步：打开系统偏好设置,按住option(alt)键点击共享
第五步：选择iPhone USB -> Wi-Fi -> 创建NAT64
用你另一台iPhone链接你Mac所创建的IPv6测试网络
```


`iOS-不用网线搭建IPv6网络测试环境 - SUPER_F - 博客园` `★`

http://www.cnblogs.com/SUPER-F/p/IPV6.html


`MAC或iOS 创建 IPv6 WIFI热点_百度经验`
https://jingyan.baidu.com/article/6181c3e0b83071152ef153f0.html



`通过自己的Mac来搭建本地IPv6网络 - 简书`
http://www.jianshu.com/p/70ca0bf3acc7


`本地 Mac 搭建 IPv6 测试环境 - 番薯大佬的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/potato512/article/details/51680203

 