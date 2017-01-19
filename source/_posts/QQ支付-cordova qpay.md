title: QQ支付-cordova qpay
date: 2016-12-16 00:00:00
tags: [cordova qpay]


---
# 业务流程
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=201
![](http://p.qpic.cn/qpaywikipic/0/qpaywikipic_582045e17a9263438/0)



# 生成订单
- 参考统一下单接口
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=58
```
<xml>
  <appid>1111223451</appid> <!-- 应用ID -->
  <body>123</body> <!-- 商品描述  -->
  <device_info>1234567890abc</device_info> <!-- 设备信息 -->
  <limit_pay></limit_pay> <!-- 支付方式限制  -->
  <mch_id>1301278501</mch_id> <!-- 商户ID -->
  <nonce_str>fecf31a13be2309093db5df934848583</nonce_str> <!-- 随机字符串 -->
  <notify_url>https://qpay.qq.com/cgi-bin/pay/qpay_unified_order.cgi</notify_url><!-- 支付结果通知地址 -->
  <out_trade_no>2016061235213702</out_trade_no> <!-- 商户订单号  -->
  <sign>f0c328d362858713b66feafb802615d8</sign><!-- 统一下单签名 -->
  <spbill_create_ip>10.123.9.102</spbill_create_ip> <!-- 终端ip -->
  <total_fee>1</total_fee> <!-- 价格  -->
  <trade_type>NATIVE</trade_type> <!-- 支付场景  -->
</xml>
```


- 签名规范
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=57

```
假设传送的参数如下： 
appid： d930ea5d5a258f4f 
mch_id： 10000100 
device_info： 1000 
body： test 
nonce_str： ibuaiVcKdpRxkhJA 
 
第一步：对参数按照key=value的格式，并按照参数名ASCII字典序排序如下： 
stringA="appid=d930ea5d5a258f4f&body=test&device_info=1000&mch_id=10000100&nonce_str=ibuaiVcKdpRxkhJA"; 


第二步：拼接API密钥： 
stringSignTemp="stringA&key=192006250b4c09247ec02edce69f6a2d" 
sign=MD5(stringSignTemp).toUpperCase()="9A0A8659F005D6984697E2CA0A9CF3B7" 


最终得到最终发送的数据： 
<xml> 
<appid>d930ea5d5a258f4f</appid> 
<mch_id>10000100</mch_id> 
<device_info>1000<device_info> 
<body>test</body> 
<nonce_str>ibuaiVcKdpRxkhJA</nonce_str>
<sign>9A0A8659F005D6984697E2CA0A9CF3B7</sign> 
<xml> 
```


- API密钥
https://qpay.qq.com/account/api_cert.shtml
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-12-23/82075208-file_1482463058867_b57.png)



# 调起支付
- android
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=165

```
（1）初始化PayApi，并将数据填写完整：
PayApi api = new PayApi();
api.appId = APP_ID; // 在http://open.qq.com注册的AppId,参与支付签名，签名关键字key为appId       
api.serialNumber = ...; // 支付序号,用于标识此次支付
api.callbackScheme = ...; // QQ钱包支付结果回调给urlscheme为callbackScheme的activity.，参看后续的“支付回调结果处理” 
api.tokenId = ...; // QQ钱包支付生成的token_id
api.pubAcc = ...; // 手Q公众帐号id.参与支付签名，签名关键字key为pubAcc
api.pubAccHint = ...; // 支付完成页面，展示给用户的提示语：提醒关注公众帐号
api.nonce = ...; // 随机字段串，每次支付时都要不一样.参与支付签名，签名关键字key为nonce
api.timeStamp = ...; // 时间戳，为1970年1月1日00:00到请求发起时间的秒数
api.bargainorId = ...; // 商户号.参与支付签名，签名关键字key为bargainorId
api.sig = ...; // 商户Server下发的数字签名，生成的签名串，参看“数字签名”
api.sigType = "HMAC-SHA1"; // 签名时，使用的加密方式，默认为"HMAC-SHA1"
 
（2）在启动QQ钱包支付前，判断一下数据是否完整，再启动QQ钱包支付：
if (api.checkParams()) {
     openApi.execApi(api);
} 
```


- cordova plugin：com.wosai.qpay
```
QPay.mqqPay({
  tokenId: '1V39f4eaf286fe3718731871b4fe96dc',// 订单号
  nonce: '',// 随机数
  bargainorId: '1424912901',// 商户号
  appKey: 'CuWbc7F0XrTpnHBa',// appkey
  sig: ''
}, function (res) {
  alert("res:" + res)
}, function (res) {
  alert("error:" + res)
});
```


- APP调用QQ钱包支付-签名规范
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=165

```
数字签名
 
为了保障商户利益与安全，商户App调用QQ钱包支付时启用另一套签名机制。该签名机制与“商户后台与QQ钱包支付后台的签名机制”是不同的。
 
1、源串构造方法
（1）将需要参与签名的所有参数按key进行字典升序排列。
（2）将第1步中排序后的参数(key=value)用&拼接起来；
（3）key中存在大小写字母，保持大小写字母的存在。不要将key进行统一转换为大写或小写操作；
（4）如果value为空, 生成格式为“key=”，这点与后台之间签名方法是不一样的；
（5）签名原始串中，字段名和字段值都采用原始值，不进行URL Encode。
 
举例：
调用某个接口，接口有如下字段：
appId、nonce、tokenId、pubAcc、bargainorId
 
实际调用接口时，各字段的值：
appId=100619284、nonce=ksjfwierwfjk、tokenId=1000000002、pubAcc=、bargainorId=1900000109
 
正确的签名原始串是：
appId=100619284&bargainorId=1900000109&nonce=ksjfwierwfjk&pubAcc=&tokenId=1000000002
 
常见的错误有：
appId=100619284&bargainorId=1900000109&nonce=ksjfwierwfjk&tokenId=1000000002
appid=100619284&bargainorid=1900000109&nonce=ksjfwierwfjk&pubacc=&tokenid=1000000002
appId=100619284&nonce=ksjfwierwfjk&tokenId=1000000002&pubAcc=&bargainorId=1900000109
 
2、 密钥构造方法
（1）在http://open.qq.com申请appId，并获得appKey；
（2）构造到密钥的方式：在应用的appkey末尾加上一个字符的“&”，即appkey&。
 
示例：
appkey 值为 d139ae6fb0175e5659dce2a7c1fe84d5
正确的密钥为：d139ae6fb0175e5659dce2a7c1fe84d5&
 
3、 生成签名值方法
（1）使用HMAC-SHA1加密算法，使用”密钥构造方法“中得到的密钥对“源串构造方法”中得到的源串进行加密（注：一般程序语言中会内置HMAC-SHA1加密算法的函数，例如PHP5.1.2之后的版本可直接调用hash_hmac函数）；
（2）然后将加密后的字符串进行Base64编码（注：一般程序语言中会内置Base64编码函数，例如PHP中可直接调用 base64_encode() 函数）；
（3）最后得到的签名值sig结果如下：
c6xXw0tNABhOMc869h1bfxTp9Mk= 
```