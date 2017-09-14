title: npm registry仓库切换
date: 2016-12-2 00:00:00
tags: [npm registry]


---
# 安装npm仓库管理 ```
npm install nrm -g
nrm ls
```
https://github.com/Pana/nrm


# 查看仓库
```
>nrm ls
 
  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
* taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```
# 切换
```
nrm use cnpm

```


---
# 手动删除registry配置
```
npm config delete registry
npm config delete disturl
 
或者
npm config edit
找到淘宝那两行,删除
```


# 手动指定registry
- 设置
```
npm config set registry https://registry.npm.taobao.org

```
- 查看
```
npm config list
```