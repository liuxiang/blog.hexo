title:  API安全(https)与会话(OAuth,参数签名)
date: 2017-06-23 00:00:00
tags: [API ]



---

# https
- 目标: 网络过程, 加密传输,对称加解密
- 作用:  劫持,监听,篡改 (广告), 隐私泄露


| / | 对称密钥如何保护 | 加密方案约定 | 互信 | 被公开的信息 |
|------ |------| ------|------|------|
|  单向验证 | 生成随机数作为密钥<br> 传输过程用公钥加密<br> -B公钥不可解密<br> -B私钥可以 | 明文反馈加密<br>使用预定的加密方案<br>+B公钥+A |随机对称密钥 |  A获得B证书,进行验证(服务端可信) |  B 公钥<br> 加密方案 <br> 加密过的对称密钥(B公钥)  |
|  双向验证 | 生成随机数作为密钥<br> 传输过程用公钥加密<br> -B公钥不可解密<br> -B私钥可以 | A公钥加密反馈加密方案<br> (仅A私钥可解) <br> 使用预定的加密方案 <br> +B公钥+A随机对称密钥  |  A获得B证书,进行验证(服务端可信)<br> B也获得A证书,进行验证(客户端可信) | B 公钥<br> A公钥 <br> 加密过的加密方案(A公钥) <br> 加密过的对称密钥(B公钥)  |


3大过程:`①公钥交换`(用于验证对方和对称密钥的加密) / `②方案协商` / `③客户端提供对称加密给服务端`


---
#  OAuth（开放授权）[ 用户托管 ]
` 理解OAuth 2.0 `
http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html

- `QQ` 网页登陆
```
QQ登录OAuth2.0总体处理流程
QQ登录OAuth2.0总体处理流程如下：
Step1：申请接入，获取appid和apikey；
Step2：开发应用，并设置协作者帐号进行测试联调；
Step3：放置QQ登录按钮；
Step4：通过用户登录验证和授权，获取Access Token；
Step5：通过Access Token获取用户的OpenID；
Step6：调用OpenAPI，来请求访问或修改用户授权的资源。 http://wiki.connect.qq.com/oauth2-0%E7%AE%80%E4%BB%8B


-  开发攻略_ Server-side

http://wiki.connect.qq.com/%E5%BC%80%E5%8F%91%E6%94%BB%E7%95%A5_server-side


- 开发攻略_Client-side
http://wiki.connect.qq.com/%E5%BC%80%E5%8F%91%E6%94%BB%E7%95%A5_client-side
```
- `QQ` app 登陆  
注:认证流程一致,仅在各初唤醒QQ的方式不同,app需要通过原生代码拉起 / 获取用户信息接口一致(openId-> get_user_info )
http://wiki.connect.qq.com/qq%E7%99%BB%E5%BD%95

```
Tencent.login	用户使用QQ账号登录应用
Tencent.logout	注销登录
Tencent.reAuth	当应用调用API返回没有权限（返回码为100030)时，可以让用户重新进行授权
Tencent.shareToQQ	可将新闻、图片、文字等分享给QQ好友、群和讨论组
Tencent.setAvatar	设置用户的QQ头像
OpenApi接口	用于调用没有被SDK分装成单独接口的OpenApi，更多功能接口请查看《API列表》
```


--- - `微信 ` 网页登录
```
1. 第三方发起微信授权登录请求，微信用户允许授权第三方应用后，微信会拉起应用或重定向到第三方网站，并且带上授权临时票据code参数；
2. 通过code参数加上AppID和AppSecret等，通过API换取access_token；
3. 通过access_token进行接口调用，获取用户基本数据资源或帮助用户实现基本操作。


https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419316505&token=&lang=zh_CN

```

- `微信` app登录
注:认证流程一致,仅在各初唤醒微信的方式不同,app需要通过原生代码拉起 / 获取用户信息接口一致(openId-> get_user_info )

```
1. 第三方发起微信授权登录请求，微信用户允许授权第三方应用后，微信会拉起应用或重定向到第三方网站，并且带上授权临时票据code参数；
2. 通过code参数加上AppID和AppSecret等，通过API换取access_token；
3. 通过access_token进行接口调用，获取用户基本数据资源或帮助用户实现基本操作。

https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419317851&token=&lang=zh_CN

```


- 总结:
OAuth(用户托管)  : 客户端与服务端之间,把原有的账号密码登录托管至第三方 授权机构,在机构登录后,颁发的 access_token (临时)用户本次业务使用.
access_token(临时会话,授权机构颁发)  : 包含了商户信息(appid)与登录用户(openId)的信息,特点是有效期控制.


---
# 会话身份传递
- session [管理后台]
缺点:依赖 cooke传递sessionId,有跨域屏障
- `账号密码` -> token [body 或 get,post]  [前端系统]
缺点:存在明文登录前提,有被劫持风险 / token被盗情况下,所有业务接口都可调用
- `app_key` + md5(`param`+`app_secret`)  [get,post 或 body] [接入第三方系统]
优点:无需登录即可开始业务,减少账号被劫持的风险 / 不用担心签名被盗用
缺点:每次交互都要加解密. 唯一风险:单一接口被恶意多次调用


---
# 参数签名
- 形式
商家密钥: app_secret

sign= MD5(` 参数字典序`+` &key= app_secret `)

sign=hash_hmac("sha256", ` 参数字典序` , app_secret )


- 作用:` 会话身份传递,防身份伪造,防数据篡改`


- 约束
1.在不知商家密钥的情况下,无法做出有效的签名,无效的签名在服务端发现时,将不受理业务。
2.客户端发起的任何行为都 不可避免 自动化攻击，但无法篡改数据。
3.自动化攻击可建立QPS监控, 机器人检测( 行为分析,报文分析)进行阻拦,https双向验证到客户端 。(爬虫,反爬虫)
注意：拼装的关键数据(价格)后端决定,避免签名前篡改。-前端校验不可信


- `微信` 支付
```
商户系统和微信支付系统主要交互说明：
步骤1：用户在商户APP中选择商品，提交订单，选择微信支付。
步骤2：商户后台收到用户支付单，调用微信支付统一下单接口。参见【统一下单API】。
步骤3：统一下单接口返回正常的prepay_id，再按签名规范重新生成签名后，将数据传输给APP。参与签名的字段名为appid，partnerid，prepayid，noncestr，timestamp，package。注意：package的值格式为Sign=WXPay
步骤4：商户APP调起微信支付。api参见本章节【app端开发步骤说明】
步骤5：商户后台接收支付通知。api参见【支付结果通知API】
步骤6：商户后台查询支付结果。，api参见【查询订单API】
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_3
```
` 微信支付 `  https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=4_3


- `QQ` 手机支付
```
商户系统和手Q支付系统主要交互说明：


步骤1：用户在商户APP中选择商品，提交订单，选择手Q支付；
步骤2：商户后台收到用户支付单，调用手Q支付统一下单接口。参见【统一下单API】；
步骤3：统一下单接口返回正常的prepay_id，再按签名规范重新生成签名后，将数据传输给APP；
步骤4：商户APP调起手Q支付；
步骤5：商户后台接收支付通知api，参见【支付结果API】
步骤6：商户后台查询支付结果api，参见【订单查询API】
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=201
```
