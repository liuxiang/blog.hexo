title: chrome 远程调试(inspect)移动端webview,找不到待调试页面
date: 2016-3-9 00:00:00
categories:  chrome


tags: [ chrome ,远程调试 ]


---
# chrome 远程调试移动端webview, 找不到 待调试 页面


# 需要开启调试功能`setWebContentsDebuggingEnabled`,增加如下代码
```
if (Build.VERSION.SDK_INT >=Build.VERSION_CODES.KITKAT) {
     WebView.setWebContentsDebuggingEnabled(true);
  }
```


以上配置方法适用于安卓应用内所有的WebView情形。
 
安卓WebView是否可调试并不取决于应用中manifest的标志变量debuggable，如果你想只在debuggable为true时候允许WebView远程调试，请使用以下代码段：


```
if (Build.VERSION.SDK_INT>= Build.VERSION_CODES.KITKAT) {
    if (0 != (getApplicationInfo().flags &=ApplicationInfo.FLAG_DEBUGGABLE)) { 
              WebView.setWebContentsDebuggingEnabled(true);
     }  
  }
```


# cordova webViewEngine 调试开关控制(可通过android sdk进行断点调试)
`project_name`\platforms\android\CordovaLib\src\org\apache\cordova\engine\SystemWebViewEngine.java
```
//Determine whether we're in debug or release mode, and turn on Debugging!
ApplicationInfo appInfo = webView.getContext().getApplicationContext().getApplicationInfo();
if ((appInfo.flags & ApplicationInfo.FLAG_DEBUGGABLE) != 0 &&
     android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.KITKAT) {
     enableRemoteDebugging();
}




    @TargetApi(Build.VERSION_CODES.KITKAT)
    private void enableRemoteDebugging() {
        try {
            WebView.setWebContentsDebuggingEnabled(true);
        } catch (IllegalArgumentException e) {
            Log.d(TAG, "You have one job! To turn on Remote Web Debugging! YOU HAVE FAILED! ");
            e.printStackTrace();
        }
    }
```
---


# android配置文件声明调试
`AndroidManifest.xml`  - `android:debuggable`

```
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme"
        android:debuggable="true">
```


---


**参考**
`移动端Web开发调试之Chrome远程调试(Remote Debugging)`

http://blog.csdn.net/freshlover/article/details/42528643


`Chrome调试Android应用（Debug）`
http://ask.dcloud.net.cn/article/69


` IBM Worklight - How do I enable WebView debugging in Android? `
https://stackoverflow.com/questions/20396372/ibm-worklight-how-do-i-enable-webview-debugging-in-android


` Disable remote debugging in Appgyver `
https://stackoverflow.com/questions/31043235/disable-remote-debugging-in-appgyver/31044036#31044036


` Error when I try to build framework file with cordova `
https://stackoverflow.com/questions/25186129/error-when-i-try-to-build-framework-file-with-cordova


` 玩转cordova之二--增强的webview `
http://www.ituring.com.cn/article/130434


<!-- more -->


