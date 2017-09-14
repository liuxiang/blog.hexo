title: jetbrains_webstorm 保存文件,不能触发BrowserSync程序同步
date: 2016-11-29 00:00:00
tags: [ webstorm ]




---
# 热加载术语整理
```
BrowserSync
webpack-dev-server
Hot Loading Component
HMR（hot module replacement）
React Hot Replace
react hot loader
```


# 更新配置
关闭临时文件功能
```
use safe write save changes to a temporary file first

使用安全写保存更改到一个临时文件

```
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/98410013-file_1484816592625_f20c.png) 


---
# 避免文件发生变化就触发浏览器同步。更新为`Ctrl+S`
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/86170105.jpg)
- Synchronze files on frame or editor tab activation : 在框架或编辑器选项卡激活时同步文件(验证结果：`关闭IDE会保存`)
- save files on frame deactivation :  保存文件失帧(验证结果：`离开IDE工具，会触发保存`-浏览器同步)
- Save fils automatically if application is idle for `15sec`: 如果应用程序空闲“15秒”，则自动保存文件
- Use "safe write"(save changes to a temporary file first) : 使用“安全写入”（`保存对临时文件的更改`）

注：此会导致Ctrl+S无法保存到实际源文件

