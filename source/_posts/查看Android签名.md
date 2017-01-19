title: 查看Android签名
date: 2016-12-19 00:00:00
tags: [Android签名]


---
# 方式一：通过签名文件 `app.keystore` 中获取
```
keytool -list -v -keystore app.keystore

```
```
F:\work\android_sign>keytool -list -v -keystore app.keystore
输入密钥库口令:
 
密钥库类型: JKS
密钥库提供方: SUN
 
您的密钥库包含 1 个条目
 
别名: app.keystore
创建日期: 2015-6-29
条目类型: PrivateKeyEntry
证书链长度: 1
证书[1]:
所有者: CN=wosaitech, OU=wosaitech, O=wosaitech, L=杭州, ST=浙江, C=cn
发布者: CN=wosaitech, OU=wosaitech, O=wosaitech, L=杭州, ST=浙江, C=cn
序列号: 61f0b791
有效期开始日期: Mon Jun 29 10:31:31 CST 2015, 截止日期: Fri May 12 10:31:31 CST 2215
证书指纹:
         MD5: FD:A1:F4:B4:FD:B7:85:1C:34:77:7E:BB:B7:F0:40:70
         SHA1: 2D:57:FB:81:F3:43:62:81:2E:50:63:46:42:BA:0A:8B:1F:92:BC:39
         SHA256: 27:EE:91:AE:B3:BF:6D:49:36:8B:AB:0F:7E:B6:83:C1:9A:B3:91:34:AA:43:62:63:AE:04:38:EC:25:25:2B:31
         签名算法名称: SHA256withRSA
         版本: 3
 
扩展:
 
#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: B9 5C 2F EC 1D 02 67 02   54 19 03 10 32 10 7C C9  .\/...g.T...2...
```
- 结果
```
SHA1: 2D:57:FB:81:F3:43:62:81:2E:50:63:46:42:BA:0A:8B:1F:92:BC:39

```


# 方式二：通过程序包`app.apk`中获取

- 先解压`app.apk`
- 再到解压目录中使用`keytool  -printcert ` （ -printcert          打印证书内容）
```
keytool -printcert -file .\META-INF\APP_KEYS.RSA

```
```
F:\work\android_sign\android-release-signed>keytool -printcert -file .\META-INF\APP_KEYS.RSA
所有者: CN=wosaitech, OU=wosaitech, O=wosaitech, L=杭州, ST=浙江, C=cn
发布者: CN=wosaitech, OU=wosaitech, O=wosaitech, L=杭州, ST=浙江, C=cn
序列号: 61f0b791
有效期开始日期: Mon Jun 29 10:31:31 CST 2015, 截止日期: Fri May 12 10:31:31 CST 2215
证书指纹:
         MD5: FD:A1:F4:B4:FD:B7:85:1C:34:77:7E:BB:B7:F0:40:70
         SHA1: 2D:57:FB:81:F3:43:62:81:2E:50:63:46:42:BA:0A:8B:1F:92:BC:39
         SHA256: 27:EE:91:AE:B3:BF:6D:49:36:8B:AB:0F:7E:B6:83:C1:9A:B3:91:34:AA:43:62:63:AE:04:38:EC:25:25:2B:31
         签名算法名称: SHA256withRSA
         版本: 3
 
扩展:
 
#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: B9 5C 2F EC 1D 02 67 02   54 19 03 10 32 10 7C C9  .\/...g.T...2...
0010: 7A EE B3 E1                                        z...
]
]
```
- 结果
```
SHA1: 2D:57:FB:81:F3:43:62:81:2E:50:63:46:42:BA:0A:8B:1F:92:BC:39
```