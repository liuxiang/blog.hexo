title: 打开QQ群链接
date: 2017-1-11 00:00:00
tags: [打开QQ群]


---
#  获取方式一： QQ群分享-链接分享

```
https://jq.qq.com/?_wv=1027&k=43b8mqv

```


#   获取方式二：QQ群管理
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/8806496-file_1484817960373_a59b.png)
http://qun.qq.com/join.html
 
- 网页
```
//shang.qq.com/wpa/qunwpa?idkey=1cd46704013588e4a44e91315e1ca7fbbf57d44da54faa7555da7cc3261293b1

```
- android
```
/****************
*
* 发起添加群流程。群号：创业吧(8182842) 的 key 为： k2JRDCGgPm8Lrf8HFQO63ZPIq53PapvL
* 调用 joinQQGroup(k2JRDCGgPm8Lrf8HFQO63ZPIq53PapvL) 即可发起手Q客户端申请加群 创业吧(8182842)
*
* @param key 由官网生成的key
* @return 返回true表示呼起手Q成功，返回fals表示呼起失败
******************/
public boolean joinQQGroup(String key) {
    Intent intent = new Intent();
    intent.setData(Uri.parse("mqqopensdkapi://bizAgent/qm/qr?url=http%3A%2F%2Fqm.qq.com%2Fcgi-bin%2Fqm%2Fqr%3Ffrom%3Dapp%26p%3Dandroid%26k%3D" + key));
   // 此Flag可根据具体产品需要自定义，如设置，则在加群界面按返回，返回手Q主界面，不设置，按返回会返回到呼起产品界面    //intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
    try {
        startActivity(intent);
        return true;
    } catch (Exception e) {
        // 未安装手Q或安装的版本不支持
        return false;
    }
}
```
- iphone
```
- (BOOL)joinGroup:(NSString *)groupUin key:(NSString *)key{
NSString *urlStr = [NSString stringWithFormat:@"mqqapi://card/show_pslcard?src_type=internal&version=1&uin=%@&key=%@&card_type=group&source=external", @"8182842",@"1cd46704013588e4a44e91315e1ca7fbbf57d44da54faa7555da7cc3261293b1"];
NSURL *url = [NSURL URLWithString:urlStr];
if([[UIApplication sharedApplication] canOpenURL:url]){
[[UIApplication sharedApplication] openURL:url];
return YES;
}
else return NO;
}
```
- 二维码地址（也可用于配置微信公众号）
```
http://qm.qq.com/cgi-bin/qm/qr?k=oBdEbu5t5-F4-GutaeK8ySZMZCU7it_m

```
