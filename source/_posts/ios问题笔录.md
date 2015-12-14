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

<!-- more -->
---
