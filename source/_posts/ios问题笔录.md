title: ios问题笔录
date: 2015-12-14 00:00:01 #发表日期，一般不改动
categories: ios #文章文类
tags: [ios, ios问题笔录]

---
# UUID找不到?
错误:
```
Code Sign error: No matching provisioning profile found: Your build settings specify a provisioning profile with the UUID “5789f696-49dc-4b81-977c-48f780c54273”, however, no such provisioning profile was found.
```
## UUID “5789f696-49dc-4b81-977c-48f780c54273”是什么?
UUID是"Provisioning Profile"对应的实际文件名,证明:
![](http://7xnbs3.com1.z0.glb.clouddn.com/15-12-14/94102932.jpg)

## 解决问题,清理无效的UUID
1.打开路径`cd  /Users/*/Library/MobileDevice` `open Provisioning Profiles`,
2.删除"5789f696-49dc-4b81-977c-48f780c54273.mobileprovision"文件.
3.重新到`苹果开发者网站`下载新的`Provisionning Profiles`证书文件, download 会自定更新xcode
4.再试报错的行为(打包),看此错误是否解决.


---
#  Command /usr/bin/codesign failed with exit code 1 
codesign 签名证书有问题
```
CodeSign /Users/wosai/Library/Developer/Xcode/DerivedData/爱科学-fhrouhlgzqrnrqcorivoclqidoop/Build/Products/Debug-iphoneos/爱科学.app
    cd "/Users/wosai/Desktop/work/hybrid Dev/k12student/platforms/ios"
    export CODESIGN_ALLOCATE=/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/codesign_allocate
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
 
Signing Identity:     "iPhone Developer: Xiang Liu (W66R7RU3TA)"
Provisioning Profile: "aikexue_dev"
                      (48442213-e40e-4b23-97d1-bb8b90b535b7)
 
    /usr/bin/codesign --force --sign 046D9F6D7C99A7C383E4FF8D76A80878BBFCB0BF --entitlements /Users/wosai/Library/Developer/Xcode/DerivedData/爱科学-fhrouhlgzqrnrqcorivoclqidoop/Build/Intermediates/爱科学.build/Debug-iphoneos/爱科学.build/爱科学.app.xcent --timestamp=none /Users/wosai/Library/Developer/Xcode/DerivedData/爱科学-fhrouhlgzqrnrqcorivoclqidoop/Build/Products/Debug-iphoneos/爱科学.app
 
046D9F6D7C99A7C383E4FF8D76A80878BBFCB0BF: no identity found
Command /usr/bin/codesign failed with exit code 1
```


`IOS 开发 证书显示 此证书签发者无效 解决办法`
http://blog.csdn.net/ddd998/article/details/50673592
```
钥匙串中的所有证书 都 提示此证书签发者无效
经查找得知系统证书WWDR在2016年2月14日失效，需要更新WWDR系统证书
下载证书地址https://developer.apple.com/certificationauthority/AppleWWDRCA.cer下载之后 双击安装
到这  还需要一步
1.在登录里面删除过期的证书WWDR
2.在系统里面 删除过期的证书WWDR 就可以完美的解决了。所有的证书 都可以使用了


找不到过期证书？点击显示-->显示已过期的证书。

```


<!-- more -->

