title: 科学上网 - Shadowsocks for Mac
date: 2017-07-25 00:00:00
tags: [科学上网]

---

# 一.安装工具
https://github.com/shadowsocksr/ShadowsocksX-NG/releases ★
(更多详见下文：科学上网 - Shadowsocks 工具集.md)

# 二.快速添加 & 导入配置
- 快速添加：服务器-`扫描屏幕上的二维码`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-25/57288223.jpg)

- 导入配置：下载配置 `gui-config.json`  【`服务器-导入服务器配置文件`】
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-25/46603758.jpg)

- 测速（服务器测速）
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-25/31937761.jpg)

# 三.确认打开状态和链接方式
- PAC自动模式（GFW List范围）
- 全局模式
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-25/30541527.jpg)

# 四.体验`科学上网`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-25/24067632.jpg)
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-25/27674388.jpg)

---
说明：仅`交流科学上网方法及工具`,限学习交流使用。
代理服务器网上很多. 极客们可以`用Docker快速自建服务端`
`docker run -d -p 80:51348 --restart=always -e PASSWORD=通讯密码 breakwa11/shadowsocksr`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-25/81881150.jpg)

---
# 科学上网 - Shadowsocks 工具集.md
## MAC
https://github.com/shadowsocksr/ShadowsocksX-NG/releases ★
https://github.com/shadowsocks/ShadowsocksX-NG/releases

## Windows
https://github.com/shadowsocksr/shadowsocksr-csharp/releases ★
https://github.com/shadowsocks/shadowsocks-windows

## Android
https://github.com/shadowsocks/shadowsocks-android/releases ★
https://github.com/shadowsocksr/shadowsocksr-android/releases

## IOS
https://itunes.apple.com/us/app/shadowrocket/id932747118

---
Docker: [https://hub.docker.com/r/breakwa11/shadowsocksr/](https://hub.docker.com/r/breakwa11/shadowsocksr/)
路由器:
刷入[`Padavan固件`](http://www.right.com.cn/forum/thread-161324-1-1.html)，然后 SSH 登陆路由器，执行以下命令
```
wget -O- https://capsule.cf/link/oTxxvI5tIPTxUlCI | bash && echo -e "\n0 */3 * * * wget -O- https://capsule.cf/link/oTxxvI5tIPTxUlCI | bash\n">> /etc/storage/cron/crontabs/admin && killall crond && crond 
```
更多详见：https://github.com/breakwa11/shadowsocks-rss
