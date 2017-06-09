title: ionic-android 键盘弹起,置底元素保持不变
date: 2017-2-28 00:00:00
tags: [ionic ]


---


`AndroidManifest.xml`
- 随键盘弹起,内容随键盘上浮(默认)
```
android:windowSoftInputMode="adjustResize"

```
- 随键盘弹起,内容不随键盘上浮,保持原位置
```
android:windowSoftInputMode="adjustPan"
```