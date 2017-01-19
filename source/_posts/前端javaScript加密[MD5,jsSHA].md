title: 前端javaScript加密[MD5,jsSHA]
date: 2016-11-04 00:00:00
tags: [ 加密 ]



---
# JavaScript MD5实现
`JavaScript MD5实现 [blueimp]`

https://github.com/blueimp/JavaScript-MD5


`JavaScript MD5实现 [ pajhome.org ]`
http://pajhome.org.uk/crypt/md5/index.html
```
  var sign = hex_md5(result);
  return sign.toUpperCase();
```


---
# 安全散列标准的完整的JavaScript实现
github： https://github.com/Caligatio/jsSHA/
在线：https://caligatio.github.io/jsSHA/

```
sign: function (jsapi_ticket, url) {
var ret = {
jsapi_ticket: jsapi_ticket,
nonceStr: this.createNonceStr(),
timestamp: this.createTimestamp(),
url: url
};
var string = this.raw(ret);
shaObj = new jsSHA(string, 'TEXT');
ret.signature = shaObj.getHash('SHA-1', 'HEX');
return ret;
}
```


---
# 第三方-安全规范
### `微信 SDK- 签名规范`
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=4_3
```
签名算法
签名生成的通用步骤如下：
第一步，设所有发送或者接收到的数据为集合M，将集合M内非空参数值的参数按照参数名ASCII码从小到大排序（字典序），使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串stringA。
特别注意以下重要规则：
◆ 参数名ASCII码从小到大排序（字典序）；
◆ 如果参数的值为空不参与签名；
◆ 参数名区分大小写；
◆ 验证调用返回或微信主动通知签名时，传送的sign参数不参与签名，将生成的签名与该sign值作校验。
◆ 微信接口可能增加字段，验证签名时必须支持增加的扩展字段
第二步，在stringA最后拼接上key得到stringSignTemp字符串，并对stringSignTemp进行MD5运算，再将得到的字符串所有字符转换为大写，得到sign值signValue。
key设置路径：微信商户平台(pay.weixin.qq.com)-->账户设置-->API安全-->密钥设置
```


`商户回调API安全`
在普通的网络环境下，HTTP请求存在DNS劫持、运营商插入广告、数据被窃取，正常数据被修改等安全风险。商户回调接口使用HTTPS协议可以保证数据传输的安全性。所以微信支付建议商户提供给微信支付的各种回调采用HTTPS协议。请参考：`HTTPS搭建指南`  https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=10_4


`微信公众平台支付接口调试工具`
https://pay.weixin.qq.com/wiki/tools/signverify/


### ` 微信JS- SDK- 签名算法`
https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115&token=&lang=zh_CN
```
签名生成规则如下：
参与签名的字段包括noncestr（随机字符串）, 有效的jsapi_ticket, timestamp（时间戳）, url（当前网页的URL，不包含#及其后面部分） 。 对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串string1。
这里需要注意的是所有参数名均为小写字符。 对string1作sha1加密，字段名和字段值都采用原始值，不进行URL 转义
```


`微信 JS 接口签名校验工具`
http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=jsapisign


---
# 参数名ASCII码从小到大排序（字典序）-微信SDK常用到（参数列表加密前排序）
参见： `微信 SDK- 签名规范`
```
raw: function (args) {
            var keys = Object.keys(args);
            keys = keys.sort()
            var newArgs = {};
            keys.forEach(function (key) {
                newArgs[key.toLowerCase()] = args[key];
            });
 
            var string = '';
            for (var k in newArgs) {
                string += '&' + k + '=' + newArgs[k];
            }
            string = string.substr(1);
            return string;
        }
```