title: ios免App Store审核安装几种途径
date: 2015-12-14 00:00:03 #发表日期，一般不改动
categories: ios  #文章文类
tags: [ ios ,ios免审核 ]


---
# 途径一 : 打包前,为用于打包的证书,关联调试设备
>此方式打出的ipa包可免审核安装



## 添加调试设备: 开发者官网-Certificates-Devices-all
>注意: 限制100台设备


## 证书关联设备
位置:  开发者官网-Certificates- Provisioning Profiles- [Development/Distribution]
*  新建:  开发证书,常用于真机调试. Development- iOS App Development ...选设备-download

*  新建:  发布证书,常用于内部测试.  Distribution - Ad Hoc...选设备-download
* 更新:  选择目标证书-Edit...勾选新设备-download(会自动打开xcode且更新相关配置)


## 打包
更新配置 Builder Setting-code Signing-Provisioning Profile 选择正确的证书
* 发布AppStore一般都选`projectname-dis`
* 需要给内部员工测试选择`projectname-dis-ad hoc`


## 导出ipa
打包成功后自动弹出  Organizer 或 手动选择  window- Organizer
选择 export - 再依据情况选择不同的导出项
* 上架App Store选: `Save for iOS App Store D eployment`
* 发布设备内侧选: `Save for Ad Hoc D eployment`

* 发布企业包选: `Save for Enterprise D eployment`

* 导出设备开发包: `Save for Developent D eployment`


>结果ipa文件会出现在桌面
![](http://7xnbs3.com1.z0.glb.clouddn.com/15-12-14/33383602.jpg)


## 发布包上架AppStore
使用 Application Loader


## 内测包,上传至第三方应用内侧托管平台
如: `fir.im` 平台会提供测试应用的地址及 二维码供内侧用户下载


---
# 途径二: 提交审核后,审核通过前. 使用itunes.apple审核APP中的TestFlight.

* 内部测试:审核通过前可安装使用. ( 限25名用户)
* 外部测试:签核后才可安装使用. (不限 用户)
![](http://7xnbs3.com1.z0.glb.clouddn.com/15-12-14/24893265.jpg)
---

# 途径三(推荐): 企业账号,使用企业证书打包
* 优点: 不需要提前添加设备,不限制设备数量,配合第三方内侧托管平台可快速发布,安装
* 特点: 企业应用,不可上架App Store,适合不公开应用使用. 其次是脱离AppStore管理,快速发布.


位置:  开发者官网-Certificates- Provisioning Profiles- [Development/Distribution]

*  新建:  企业证书,用于企业发布. Development- iOS App Development ...选设备-download



`ios apple企业账号申请流程`
http://my.oschina.net/u/1025290/blog/299040?fromerr=j6bC8HuY


`iOS 企业证书发布app 流程`
http://blog.csdn.net/justinjing0612/article/details/8758692


`以无线方式安装企业内部应用-官方说明`
http://help.apple.com/deployment/ios/#/apda0e3426d7


`ios apple企业账号申请流程`
http://my.oschina.net/u/1025290/blog/299040
http://bbs.9ria.com/thread-197242-1-1.html

http://www.cnblogs.com/xilinch/p/4037164.html


`iOS 开发者中的公司账号与个人账号之间有什么区别？`
https://www.zhihu.com/question/20308474
https://developer.apple.com/support/compare-memberships/


# # 企业包,上传至第三方应用内侧托管平台
如: `fir.im` 平台会提供测试应用的地址及 二维码供内侧用户下载


---
# 途径四 (非商业):  越狱+第三方工具安装 


<!-- more -->

