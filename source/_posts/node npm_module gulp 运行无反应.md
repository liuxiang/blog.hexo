title: node npm_module gulp 运行无反应
date: 2016-3-3 00:00:02
categories:   node  

tags: [ node ]


---
# node npm_module 运行无反应(空白返回)
```
C:\Users\Administrator>gulp


C:\Users\Administrator>
```


# 排查
```
npm remove gulp
npm install gulp （观察安装过程中的错误）
```
提示：`node-gyp rebuild`




# 原因
如果排除gulp插件问题,那就是gulp运行脚本内命令无法执行,可能是node问题


# 解决办法
1.尝试重装npm_module插件gulp
2.尝试重装nodejs


<!-- more -->
