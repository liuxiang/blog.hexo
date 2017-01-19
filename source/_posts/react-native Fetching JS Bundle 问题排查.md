title: react-native Fetching JS Bundle 问题排查
date: 2016-12-1 00:00:00
tags: [react-native]


---


# 问题现象
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/66720886-file_1484816673101_12b00.png)



# 排查`React Packager`日志
```
Scanning 1 folders for symlinks in G:\liuxiang_code_git\my-react-native\30-days-of-react-native-development\node_modules\.npminstall\react-native\0.34.1 (1ms)
┌────────────────────────────────────────────────────────── ──────────────────┐
 │  Running packager on port 8081.                                            │
 │                                                                            │
 │  Keep this packager running while developing on any JS projects. Feel      │
 │  free to close this tab and run your own packager instance if you          │
 │  prefer.                                                                   │
 │                                                                            │
 │  https://github.com/facebook/react-native                                  │
 │                                                                            │
└────────────────────────────────────────────────────────── ──────────────────┘
Looking for JS files in
   G:\liuxiang_code_git\my-react-native\30-days-of-react-native-development\node_modules\.npminstall\react-native\0.34.1\react-native
```


## 分析
可见` Looking for JS files in `项目目标不应该在`node_modules`中
正确的目标应该是
```
G:\liuxiang_code_git\my-react-native\30-days-of-react-native-development

```


## 解决办法
删除`node_modules`,重装.
```
npm install
```
- 国内加速
```
npm install -g cnpm --registry=https://registry.npm.taobao.org

```
- 查看
```
npm config get registry
```