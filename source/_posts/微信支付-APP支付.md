title: 微信支付-APP支付
date: 2016-12-16 00:00:02
tags: [微信支付]


---


# 业务流程

https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_3
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/54482401-file_1484817226181_73db.png)


- 过程说明
```
商户系统和微信支付系统主要交互说明：


步骤1：用户在商户APP中选择商品，提交订单，选择微信支付。
步骤2：商户后台收到用户支付单，调用微信支付统一下单接口。参见【统一下单API】。
步骤3：统一下单接口返回正常的prepay_id，再按签名规范重新生成签名后，将数据传输给APP。参与签名的字段名为appId，partnerId，prepayId，nonceStr，timeStamp，package。注意：package的值格式为Sign=WXPay
步骤4：商户APP调起微信支付。api参见本章节【app端开发步骤说明】
步骤5：商户后台接收支付通知。api参见【支付结果通知API】
步骤6：商户后台查询支付结果。，api参见【查询订单API】
```


# 生成订单（ 微信 ）
参考：统一下单接口 `<trade_type>APP</trade_type>`
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_1
```
<xml>
   <appid>wx2421b1c4370ec43b</appid>
   <attach>支付测试</attach>
   <body>APP支付测试</body>
   <mch_id>10000100</mch_id>
   <nonce_str>1add1a30ac87aa2db72f57a2375d8fec</nonce_str>
   <notify_url>http://wxpay.wxutil.com/pub_v2/pay/notify.v2.php</notify_url>
   <out_trade_no>1415659990</out_trade_no>
   <spbill_create_ip>14.23.150.211</spbill_create_ip>
   <total_fee>1</total_fee>
   <trade_type>APP</trade_type>
   <sign>0CB01533B8C1EF103065174F50BCA001</sign>
</xml> 
```


- API签名规范
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=4_3

```
假设传送的参数如下：
appid： wxd930ea5d5a258f4f
mch_id： 10000100
device_info： 1000
body： test
nonce_str： ibuaiVcKdpRxkhJA
 
第一步：对参数按照key=value的格式，并按照参数名ASCII字典序排序如下：
stringA="appid=wxd930ea5d5a258f4f&body=test&device_info=1000&mch_id=10000100&nonce_str=ibuaiVcKdpRxkhJA";


第二步：拼接API密钥：
stringSignTemp="stringA&key=192006250b4c09247ec02edce69f6a2d"
sign=MD5(stringSignTemp).toUpperCase()="9A0A8659F005D6984697E2CA0A9CF3B7"
 
最终得到最终发送的数据：
<xml>
<appid>wxd930ea5d5a258f4f</appid>
<mch_id>10000100</mch_id>
<device_info>1000<device_info>
<body>test</body>
<nonce_str>ibuaiVcKdpRxkhJA</nonce_str>
<sign>9A0A8659F005D6984697E2CA0A9CF3B7</sign>
<xml>  
```
微信提供相关接口在线签名验证工具： https://pay.weixin.qq.com/wiki/tools/signverify/


- API密钥
商户平台中设置  https://pay.weixin.qq.com
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/41729331-file_1484817256396_ffb0.png)

 
# 调起支付
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_5
- android
```
IWXAPI api;
PayReq request = new PayReq();
request.appId = "wxd930ea5d5a258f4f";
request.partnerId = "1900000109";
request.prepayId= "1101000000140415649af9fc314aa427",;
request.packageValue = "Sign=WXPay";
request.nonceStr= "1101000000140429eb40476f8896f4c9";
request.timeStamp= "1398746574";
request.sign= "7FFECB600D7157C5AA49810D2D8F28BC2811827B";
api.sendReq(req);
```
- cordova plugin ： cordova-plugin-wechat

https://github.com/xu-li/cordova-plugin-wechat

```
var dto = response.data.object.dto;
// var params = {
//   appid: resData.appid,
//   noncestr: resData.nonce_str, // nonce:随机字符串，不长于32位。推荐随机数生成算法
//   package: resData.package,
//   partnerid: resData.partnerid, // merchant id : 商户号
//   prepayid: resData.prepay_id, // prepay id:微信生成的预支付回话标识，用于后续接口调用中使用，该值有效期为2小时
//   timestamp: resData.timestamp, // timestamp:时间戳 String(10)
//   sign:resData.sign
// };
 
window.Wechat && window.Wechat.sendPaymentRequest(dto, function () {
  console.log("Success");
  orderCheck(response.data.object.code);// 订单校验
}, function (reason) {
  // alert("Failed: " + reason);
  console.log("Failed:", reason);
});
```


- 该sign生成字段名列表见调起支付API
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_12


- ` API签名规范`(同上)
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=4_3


- 依赖API密钥 (同上)
