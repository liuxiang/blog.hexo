title: React native相关
date: 2016-12-20 00:00:02
tags: [React native]


---
# React Native简介
React Native使你能够在Javascript和React的基础上获得完全一致的开发体验，构建世界一流的原生APP。



支持系统：Android 4.1 (API 16) 以及 >= iOS 7.0



`github`  https://facebook.github.io/react-native/docs/getting-started.html


`React Native 中文网`
http://reactnative.cn/


`从零开始的Android新项目10 - React Native & Redux - 马克宅只是个码农 - 博客频道 - CSDN.NET`
http://blog.csdn.net/marktheone/article/details/52238413


---
## `github` React Native 资料
`React-Native学习指南` `star 7074`

https://github.com/reactnativecn/react-native-guide



`教程和处理React Native的资源的列表`   `star 9094`
https://github.com/jondot/awesome-react-native


`React资料整理，基于个人书签收集导出`
https://github.com/OXOYO/F2E-Tutorial-Collect/blob/master/React.md
`React-Native入门指南`
https://github.com/vczero/react-native-lesson


---
# 工具
- IDE

`atom`  https://atom.io/

`WebStorm`
`Sublime`


-  代码提示


`React Native研发代码编辑工具之WebStorm配置`
http://www.tuicool.com/articles/fQ3IriM


`ReactNative-LiveTemplate`
https://github.com/virtoolswebplayer/ReactNative-LiveTemplate


---


# 几类移动应用开发方式区别
类型 | 技术 | 特点 | 性能 | 技术手段
-|-|-|
web | weiui等(响应式mobile库) | 运行环境限制在手机浏览器中（含微信） | 一般 | 纯前端
hybrid app | ionic （angular+cordova）| 同时支持手机浏览器和独立APP | 还行 | UI用前端,本地设备用cordova plugin
native app | React native（RN），ionic2 | 仅支持独立APP | 优秀 | ui和设备全用前端语言映射，不完成支持前端特性（html，css）
native | android、ios | 仅支持独立APP | 极限方案 | 多平台独立编写


---
# 使用
- 搭建开发环境 & 安装
```
react-native init AwesomeProject
cd AwesomeProject
react-native run-android
```
- 仅启动服务(调试)
```
react-native start
```
http://reactnative.cn/docs/0.39/getting-started.html
 
## 编写Hello World (注：没有DIV)
`index.ios.js`或是`index.android.js`

```
import React, { Component } from 'react';
import { AppRegistry, Text } from 'react-native';
 
class HelloWorldApp extends Component {
  render() {
    return (
      <Text>Hello world!</Text>
    );
  }
}
 
// 注意，这里用引号括起来的'HelloWorldApp'必须和你init创建的项目名一致
AppRegistry.registerComponent('HelloWorldApp', () => HelloWorldApp);
```


---
# 详解 ` index.android.js `
- 引入依赖
```
import React, {Component} from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View
} from 'react-native';
```
- 注册组件
```
AppRegistry.registerComponent('AwesomeProject', () => AwesomeProject);

```
- 类定义
```
export default class AwesomeProject extends Component {
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.hello}>
                    hello world
                </Text>
                <Text style={styles.welcome}>
                    Welcome to React Native!
                </Text>
                <Text style={styles.instructions}>
                    To get started, edit index.android.js
                </Text>
                <Text style={styles.instructions}>
                    Double tap R on your keyboard to reload,{'\n'}
                    Shake or press menu button for dev menu
                </Text>
            </View>
        );
    }
}
```
```
export default class AwesomeProject extends Component

export的带default关键字的组件，即默认组件,将其命名为 AwesomeProject (可以自定义命名)
```
- 样式(命名方式的变化如` fontSize` )
```
const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
    },
    hello:{
        color:'red',
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
    },
    welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
    },
    instructions: {
        textAlign: 'center',
        color: '#333333',
        marginBottom: 5,
    },
});
```


---
# 初始页模块化
- `index.android.js`
```
import {AppRegistry} from 'react-native';
import setup from './src/setup';
 
// AppRegistry.registerComponent('AwesomeProject', setup);// function (问题:Hot不同步)
AppRegistry.registerComponent('AwesomeProject', () => setup);// module
```
- `/src/setup.js`
```
'use strict';
 
import React, {Component} from 'react';
import {StyleSheet, Text, View, Navigator} from 'react-native';


export default class extends Component {
    render() {
        return (
           ...
        );
    }
}
```


---
`RN----导入组件，import from 'xxxx'的用法详解`
http://blog.csdn.net/chevins/article/details/51523770


---
# 问题
- 依赖sdk`23.0.1 `,安装即可
```
Starting JS server...                                                                    
Building and installing the app on the device (cd android && gradlew.bat installDebug)...
 
FAILURE: Build failed with an exception.                                                 
 
* What went wrong:                                                                       
A problem occurred configuring project ':app'.                                           
> failed to find Build Tools revision 23.0.1   
```
http://stackoverflow.com/questions/33155087/react-native-on-android-failed-to-find-build-tools

 
- react-native Fetching JS Bundle
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/66720886-file_1484816673101_12b00.png)

处理办法：删除node_modules,重新`npm install` (建议使用国内镜像)
 
- 启动后，无法正常打开，是因为`调试工具`依赖`悬浮窗`权限，需要手动赋予。
`react-native在Anroid真机运行时可能会遇到白屏的情况解决办法`
http://blog.csdn.net/itpinpai/article/details/50845625

![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/80604699-file_1484818079208_893f.png)
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/65728024-file_1484818103275_f82a.png)

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/50668486-file_1484818111205_13b48.png)





 
 
 