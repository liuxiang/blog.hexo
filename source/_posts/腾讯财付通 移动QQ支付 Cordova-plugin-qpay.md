title: 腾讯财付通 移动QQ支付 Cordova-plugin-qpay
date: 2016-12-23 00:00:00
tags: [Cordova-plugin-qpay]


---
# 腾讯财付通 移动QQ支付 Cordova-plugin-qpay
- 场景介绍
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=160
 
- 支付流程
https://qpay.qq.com/qpaywiki/showdocument.php?pid=38&docid=201
![](http://p.qpic.cn/qpaywikipic/0/qpaywikipic_582045e17a9263438/0)  


- 商户接入指引
http://kf.qq.com/faq/150107UZvUZB160405mumyaq.html
 
- 普通商户如何接入APP支付？`★特别注意,如未填写将无法使用APP支付.且无法变更,只能重新申请`
![](http://file.service.qq.com/user-files/uploads/201611/1445a222fa15c23d2a688ee6041eea51.jpg)
http://kf.qq.com/faq/150107UZvUZB161117vyEBBR.html


---
# 卸载
```
cordova plugin remove com.wosai.qpay
```
 
# 安装
```
cordova plugin add https://github.com/liuxiang/cordova-plugin-qpay.git --variable qpayappid=YOUR_QPAY_APPID
```
 
---
# 官方测试账号（快速验证qpay plugin）
注：ios可调试到支付。 android因为没有官方的签名文件导致服务正确匹配sha1，会提示商户中找不到appid。
- 1.安装插件
```
cordova plugin add plugins-ws/com.wosai.qpay --variable qpayappid=100619284
```
 
- 获取QQ支付订单（orderCode or tokenId）
访问demo地址可获得 http://fun.svip.qq.com/mqqopenpay_demo.php
 
- 2.程序调用
```
QPay.mqqPay({tokenId:'1V39f4eaf286fe3718731871b4fe96dc'} ,function(res){console.log("res",res)},function(res){console.log("error",res)});
```
 
- 3.更新ios应用名`config.xml`
```
<widget id="com.tencent.qqwallet.example"
```
 
- 4.编译，xcode运行
```
ionic build ios
```
 
---
# 能力
- 是否安装QQ(限：android)
```
QPay.isMqqInstalled(function(res){console.log(res)});
```
- 是否支持移动QQ支付(限：android)
```
QPay.isMqqSupportPay(function(res){console.log(res)})
```
- 获取订单（后端实现，插件留白）
```
QPay.getOrderno(function(res){console.log("res",res)},function(res){console.log("error",res)})
```
- ★移动QQ支付（android | ios）
```
QPay.mqqPay({
  tokenId: '1V39f4eaf286fe3718731871b4fe96dc',// 订单号
  nonce: '',// 随机数
  bargainorId: '1234567890',// 商户号
  appKey: '88888888888888',// appkey
  sig: ''
}, function (res) {
  alert("res:" + res)
}, function (res) {
  alert("error:" + res)
});
```
- 建议先用官方测试账号验证（未填字段会在插件中补充-仅限测试）
```
QPay.mqqPay_demo({tokenId:'1V39f4eaf286fe3718731871b4fe96dc'} ,function(res){console.log("res",res)},function(res){console.log("error",res)});
```
 
---
#  签名纠正功能(后端签名,也可参考此代码段)
注：签名依赖密钥等敏感信息，建议后台进行。插件提供此功能，仅用于后台签名校验。上线请关闭`debug = false`
## android 见`qpay.java`
注：APP_KEY默认填写了访问示例使用的app_key. 调试自由app支付，请修改。
```
boolean debug = true;// 是否开启debug
if (debug) {
  String sig_online = signApi(api);// 请手动更新APP_KEY
  if (api.sig != sig_online) {
    System.out.println("签名错误-纠正");
    api.sig = sig_online;
  }
}
```
```
/**
* 签名步骤建议不要在app上执行，要放在服务器上执行.
*/
public String signApi(PayApi api) throws Exception {
  // 按key排序
  StringBuilder stringBuilder = new StringBuilder();
  stringBuilder.append("appId=").append(api.appId);
  stringBuilder.append("&bargainorId=").append(api.bargainorId);
  stringBuilder.append("&nonce=").append(api.nonce);
  stringBuilder.append("&pubAcc=").append("");
  stringBuilder.append("&tokenId=").append(api.tokenId);
 
  String APP_KEY = "d139ae6fb0175e5659dce2a7c1fe84d5";// 调试使用,应该服务器保护
  byte[] byteKey = (APP_KEY + "&").getBytes("UTF-8");
  // 根据给定的字节数组构造一个密钥,第二参数指定一个密钥算法的名称
  SecretKey secretKey = new SecretKeySpec(byteKey, "HmacSHA1");
  // 生成一个指定 Mac 算法 的 Mac 对象
  Mac mac = Mac.getInstance("HmacSHA1");
  // 用给定密钥初始化 Mac 对象
  mac.init(secretKey);
  byte[] byteSrc = stringBuilder.toString().getBytes("UTF-8");
  // 完成 Mac 操作
  byte[] dst = mac.doFinal(byteSrc);
 
  // Base64
  // api.sig = Base64.encodeToString(dst, Base64.NO_WRAP);
  // api.sigType = "HMAC-SHA1";
  return Base64.encodeToString(dst, Base64.NO_WRAP);
}
```
 
## ios 见`qpay.m`
注：APP_KEY默认填写了访问示例使用的app_key. 调试自由app支付，请修改。
```
BOOL debug = true;// 调试
if(debug){
    // 获取签名串
    NSString *appKey = @"d139ae6fb0175e5659dce2a7c1fe84d5";// 测试使用，正式环境请删除
    NSString *sig_online = [self getMySignatureWithAppId:appId
                                             bargainorId:bargainorId
                                                   nonce:nonce
                                                 tokenId:tokenId
                                              signingKey:appKey];
 
    if(sig != sig_online){
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"签名错误－纠正" message:nil delegate:nil cancelButtonTitle:@"OK" otherButtonTitles:nil, nil];
        [alert show];
        sig = sig_online;
    }
}
```
```
// 生成签名串
- (NSString *)getMySignatureWithAppId:(NSString *)appId
                          bargainorId:(NSString *)bargainorId
                                nonce:(NSString *)nonce
                              tokenId:(NSString *)tokenId
                           signingKey:(NSString *)signingKey{
 
    // 1. 将 appId,bargainorId,nonce,tokenId 拼成字符串
    NSString *source = [NSString stringWithFormat:@"appId=%@&bargainorId=%@&nonce=%@&pubAcc=&tokenId=%@",
                        appId,bargainorId,nonce,tokenId];
 
    // 2. 将刚才拼好的字符串，用key来加密
    NSData *signature = [source dataUsingEncoding:NSUTF8StringEncoding];
    NSData *signingData = [[signingKey stringByAppendingString:@"&"] dataUsingEncoding:NSUTF8StringEncoding];  // 约定在key末尾拼一个‘&’符号
    NSData *digest = [self hmacSha1:signature key:signingData];
 
    // 3. 将加密后的data以base64形式输出
    NSString *signatureBase64 = [digest base64Encoding];
    return signatureBase64;
}
```
 
---
# 后端指引
 
## 1.生成订单
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
<!--
-->
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-12-23/82075208-file_1482463058867_b57.png)
 
## 2.调起支付
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
# 2.1代码示例
- android
```
/**
* 签名步骤建议不要在app上执行，要放在服务器上执行.
*/
public String signApi(PayApi api) throws Exception {
  // 按key排序
  StringBuilder stringBuilder = new StringBuilder();
  stringBuilder.append("appId=").append(api.appId);
  stringBuilder.append("&bargainorId=").append(api.bargainorId);
  stringBuilder.append("&nonce=").append(api.nonce);
  stringBuilder.append("&pubAcc=").append("");
  stringBuilder.append("&tokenId=").append(api.tokenId);
 
  String APP_KEY = "d139ae6fb0175e5659dce2a7c1fe84d5";// 调试使用,应该服务器保护
  byte[] byteKey = (APP_KEY + "&").getBytes("UTF-8");
  // 根据给定的字节数组构造一个密钥,第二参数指定一个密钥算法的名称
  SecretKey secretKey = new SecretKeySpec(byteKey, "HmacSHA1");
  // 生成一个指定 Mac 算法 的 Mac 对象
  Mac mac = Mac.getInstance("HmacSHA1");
  // 用给定密钥初始化 Mac 对象
  mac.init(secretKey);
  byte[] byteSrc = stringBuilder.toString().getBytes("UTF-8");
  // 完成 Mac 操作
  byte[] dst = mac.doFinal(byteSrc);
 
  // Base64
  // api.sig = Base64.encodeToString(dst, Base64.NO_WRAP);
  // api.sigType = "HMAC-SHA1";
  return Base64.encodeToString(dst, Base64.NO_WRAP);
}
```
- ios
```
// 生成签名串
- (NSString *)getMySignatureWithAppId:(NSString *)appId
                          bargainorId:(NSString *)bargainorId
                                nonce:(NSString *)nonce
                              tokenId:(NSString *)tokenId
                           signingKey:(NSString *)signingKey{
 
    // 1. 将 appId,bargainorId,nonce,tokenId 拼成字符串
    NSString *source = [NSString stringWithFormat:@"appId=%@&bargainorId=%@&nonce=%@&pubAcc=&tokenId=%@",
                        appId,bargainorId,nonce,tokenId];
 
    // 2. 将刚才拼好的字符串，用key来加密
    NSData *signature = [source dataUsingEncoding:NSUTF8StringEncoding];
    NSData *signingData = [[signingKey stringByAppendingString:@"&"] dataUsingEncoding:NSUTF8StringEncoding];  // 约定在key末尾拼一个‘&’符号
    NSData *digest = [self hmacSha1:signature key:signingData];
 
    // 3. 将加密后的data以base64形式输出
    NSString *signatureBase64 = [digest base64Encoding];
    return signatureBase64;
}
```


---
# 可能遇到的问题
## ios build error
- 错误
```
Library not found for -lcrt1.3.1.o
```
- 解决
```
deployment target from ios5.0 then change it to ios6.0 or later
```
 
## cordova 方法控制台需要在第二次调用时才会回调方法,可测试一下几个方法
```
Wechat.isInstalled(function(res){console.log("res",res)},function(res){console.log("error",res)})
YCQQ.checkClientInstalled(function(res){console.log("res",res)},function(res){console.log("error",res)})
 
QPay.getOrderno(function(res){console.log("res",res)},function(res){console.log("error",res)})
QPay.mqqPay({tokenId:'1V39f4eaf286fe3718731871b4fe96dc'} ,function(res){console.log("res",res)},function(res){console.log("error",res)});
```
注:经测试,代码调用不会出现首次首次失效的情况
 
## xcode插件失效
```
2016-12-06 17:15:03.982 xcodebuild[9374:245800] [MT] PluginLoading: Required plug-in compatibility UUID 8A66E736-A720-4B3C-92F1-33D9962C69DF for plug-in at path '~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/Unity4XC.xcplugin' not present in DVTPlugInCompatibilityUUIDs
Build settings from command line:
    ARCHS = armv7 armv7s arm64
    CONFIGURATION_BUILD_DIR = /Users/wosai/Desktop/work/hybrid Dev/sxb_physics/platforms/ios/build/device
    SDKROOT = iphoneos10.0
    SHARED_PRECOMPS_DIR = /Users/wosai/Desktop/work/hybrid Dev/sxb_physics/platforms/ios/build/sharedpch
    VALID_ARCHS = armv7 armv7s arm64
```
- 处理办法1
```
curl -s https://raw.githubusercontent.com/ForkPanda/RescueXcodePlug-ins/master/RescueXcode.sh | sh
```
http://stackoverflow.com/questions/35110910/xcode-7-pluginloading-required-plug-in-compatibility-uuid
 
- 处理办法2
```
xcode-select --install
```
http://stackoverflow.com/questions/20732327/xcode-5-required-plug-in-not-present-in-dvtplugincompatibilityuuids
 
- 处理办法3
```
find ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins -name Info.plist -maxdepth 3 | xargs -I{} defaults write {} DVTPlugInCompatibilityUUIDs -array-add `defaults read /Applications/Xcode.app/Contents/Info.plist DVTPlugInCompatibilityUUID`
```
http://joeshang.github.io/2015/04/10/fix-xcode-upgrade-plugin-invalid/
 
## SDK 'iOS 10.0'问题
```
Check dependencies
Signing for "赛学霸物理" requires a development team. Select a development team in the project editor.
Code signing is required for product type 'Application' in SDK 'iOS 10.0'
 
** BUILD FAILED **
```
- 解决办法
```
打开xcode,General中勾选:Automatically manage signing
```
http://blog.csdn.net/h643342713/article/details/52728782