title: ionic打包未签名apk
date: 2015-12-14 00:00:05 #发表日期，一般不改动
categories: ionic  #文章文类
tags: [ ionic,android打包 ]


---
# 常规打包`cordova build --release android`
可能会出现的错误
![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-14/77923767.jpg)


```
:compileReleaseJava                 
注: 某些输入文件使用或覆盖了已过时的 API。                                                                                                                                                                              
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。                                                                                                                                                                  
注: 某些输入文件使用了未经检查或不安全的操作。                                                                                                                                                                          
注: 有关详细信息, 请使用 -Xlint:unchecked 重新编译。                                                                                                                                                                                   
:lintVitalRelease                   
F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values\arrays.xml:3: Error: "country_codes" is not translated in "ar" (Arabic), "bg" (Bulgarian), "ca" (Catalan), "cs" (Czech), "da" (Danish), "de" (G
erman), "el" (Greek), "es" (Spanish), "eu" (Basque), "fi" (Finnish), "fr" (French), "he" (Hebrew), "hi" (Hindi), "hu" (Hungarian), "id" (Indonesian), "it" (Italian), "iw" (Hebrew), "ja" (Japanese), "ko" (Korean), "nl
" (Dutch), "pl" (Polish), "pt" (Portuguese), "ru" (Russian), "sk" (Slovak), "sl" (Slovene), "sv" (Swedish), "tr" (Turkish), "zh-rCN" (Chinese: China), "zh-rTW" (Chinese: Taiwan, Province of China) [MissingTranslation
]
  <string-array name="country_codes">
                ~~~~~~~~~~~~~~~~~~~~
F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values\strings.xml:3: Error: "app_name" is not translated in "ar" (Arabic), "bg" (Bulgarian), "ca" (Catalan), "cs" (Czech), "da" (Danish), "de" (Germa
n), "el" (Greek), "es" (Spanish), "eu" (Basque), "fi" (Finnish), "fr" (French), "he" (Hebrew), "hi" (Hindi), "hu" (Hungarian), "id" (Indonesian), "it" (Italian), "iw" (Hebrew), "ja" (Japanese), "ko" (Korean), "nl" (D
utch), "pl" (Polish), "pt" (Portuguese), "ru" (Russian), "sk" (Slovak), "sl" (Slovene), "sv" (Swedish), "tr" (Turkish), "zh-rCN" (Chinese: China), "zh-rTW" (Chinese: Taiwan, Province of China) [MissingTranslation]
    <string name="app_name">赛学霸</string>                                                                                                                                                                             
            ~~~~~~~~~~~~~~~
F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values\strings.xml:4: Error: "launcher_name" is not translated in "ar" (Arabic), "bg" (Bulgarian), "ca" (Catalan), "cs" (Czech), "da" (Danish), "de" (
German), "el" (Greek), "es" (Spanish), "eu" (Basque), "fi" (Finnish), "fr" (French), "he" (Hebrew), "hi" (Hindi), "hu" (Hungarian), "id" (Indonesian), "it" (Italian), "iw" (Hebrew), "ja" (Japanese), "ko" (Korean), "n
l" (Dutch), "pl" (Polish), "pt" (Portuguese), "ru" (Russian), "sk" (Slovak), "sl" (Slovene), "sv" (Swedish), "tr" (Turkish), "zh-rCN" (Chinese: China), "zh-rTW" (Chinese: Taiwan, Province of China) [MissingTranslatio
n]
    <string name="launcher_name">@string/app_name</string>
            ~~~~~~~~~~~~~~~~~~~~
F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values\strings.xml:5: Error: "activity_name" is not translated in "ar" (Arabic), "bg" (Bulgarian), "ca" (Catalan), "cs" (Czech), "da" (Danish), "de" (
German), "el" (Greek), "es" (Spanish), "eu" (Basque), "fi" (Finnish), "fr" (French), "he" (Hebrew), "hi" (Hindi), "hu" (Hungarian), "id" (Indonesian), "it" (Italian), "iw" (Hebrew), "ja" (Japanese), "ko" (Korean), "n
l" (Dutch), "pl" (Polish), "pt" (Portuguese), "ru" (Russian), "sk" (Slovak), "sl" (Slovene), "sv" (Swedish), "tr" (Turkish), "zh-rCN" (Chinese: China), "zh-rTW" (Chinese: Taiwan, Province of China) [MissingTranslatio
n]
    <string name="activity_name">@string/launcher_name</string>
            ~~~~~~~~~~~~~~~~~~~~


   Explanation for issues of type "MissingTranslation":
   If an application has more than one locale, then all the strings declared
   in one language should also be translated in all other languages.


   If the string should not be translated, you can add the attribute
   translatable="false" on the <string> element, or you can define all your
   non-translatable strings in a resource file called donottranslate.xml. Or,
   you can ignore the issue with a tools:ignore="MissingTranslation"
   attribute.


   By default this detector allows regions of a language to just provide a
   subset of the strings and fall back to the standard language strings. You
   can require all regions to provide a full translation by setting the
   environment variable ANDROID_LINT_COMPLETE_REGIONS.


   You can tell lint (and other tools) which language is the default language
   in your res/values/ folder by specifying tools:locale="languageCode" for
   the root <resources> element in your resource file. (The tools prefix
   refers to the namespace declaration http://schemas.android.com/tools.)


F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-ar\strings.xml:64: Error: "menu_settings" is translated here but not found in default locale [ExtraTranslation]
  <string name="menu_settings">???????</string>
          ~~~~~~~~~~~~~~~~~~~~
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-bg\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-ca\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-cs\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-da\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-de\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-el\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-es\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-eu\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-fi\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-fr\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-he\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-hi\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-hu\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-id\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-it\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-iw\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-ja\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-ko\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-nl\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-pl\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-pt\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-ru\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-sk\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-sl\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-sv\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-tr\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-zh-rCN\strings.xml:64: Also translated here
    F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\res\values-zh-rTW\strings.xml:64: Also translated here


   Explanation for issues of type "ExtraTranslation":
   If a string appears in a specific language translation file, but there is
   no corresponding string in the default locale, then this string is probably
   unused. (It's technically possible that your application is only intended
   to run in a specific locale, but it's still a good idea to provide a
   fallback.).


   Note that these strings can lead to crashes if the string is looked up on
   any locale not providing a translation, so it's important to clean them
   up.


5 errors, 0 warnings
                                                                                                                                                                                                                       :
lintVitalRelease                                                                                                                                                                                                       F
AILED         
              
FAILURE: Build failed with an exception.
              
* What went wrong:
Execution failed for task ':lintVitalRelease'.
> Lint found fatal errors while assembling a release target.
              
To proceed, either fix the issues identified by lint, or modify your build script as follows:
...
android {
    lintOptions {
        checkReleaseBuilds false
        // Or, if you prefer, you can continue to check for errors in release builds,
        // but continue the build even when errors are found:
        abortOnError false
    }
}
...
              
* Try:        
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.
              
BUILD FAILED  
              
Total time: 22.092 secs
              
F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\cordova\node_modules\q\q.js:126
                    throw e;
                    ^
Error code 1 for command: cmd with args: /s /c "F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\gradlew cdvBuildRelease -b F:\work\wosai\code.wosai\edu.k12\k12student\platforms\android\build.gradle -Dor
g.gradle.daemon=true"
ERROR building one of the platforms: Error: cmd: Command failed with exit code 1
You may not have the required environment or OS to build this project
Error: cmd: Command failed with exit code 1
    at ChildProcess.whenDone (C:\Users\Administrator\AppData\Roaming\npm\node_modules\cordova\node_modules\cordova-lib\src\cordova\superspawn.js:139:23)
    at emitTwo (events.js:87:13)
    at ChildProcess.emit (events.js:172:7)
    at maybeClose (internal/child_process.js:817:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:211:5)
```


# 已经提示了处理建议
```
* What went wrong:
Execution failed for task ':lintVitalRelease'.
> Lint found fatal errors while assembling a release target.
              
To proceed, either fix the issues identified by lint, or modify your build script as follows:
...
android {
    lintOptions {
        checkReleaseBuilds false
        // Or, if you prefer, you can continue to check for errors in release builds,
        // but continue the build even when errors are found:
        abortOnError false
    }
}
...
```
查看错误日志发现问题基本是 translation 错误
# 解决方法
在 platforms/android/build.gradle 文件的android {} 节点中增加
```
android {
  lintOptions{
          abortOnError false
          checkReleaseBuilds false

  }
}
```
如图:
![](http://7xnbs3.com1.z0.glb.clouddn.com/15-12-14/57618196.jpg)


<!-- more -->


# 参考
`Error when running cordova build –release android - Stack Overflow`
http://stackoverflow.com/questions/30345879/error-when-running-cordova-build-release-android


`Translations errors in cordova 5 · Issue #66 `
https://github.com/wymsee/cordova-imagePicker/issues/66

