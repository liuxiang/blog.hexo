title: package.json维护
date: 2016-1-7 00:00:03 #发表日期，一般不改动
categories: ionic  #文章文类
tags: [ ionic ,package.json ]


---

# ionic项目同步更新package or 依据package



# 同步更新package.json
**npm部分**
> 方式一: 删除`package.json`中的`dependencies`项,再`npm init`会依据`node_modules`的实际安装情况,更新`dependencies`配置项.但会多余配置部分信息,可能与原文件不一致,会出现覆盖.
 
> 方式二: 为减少误操作,可先备份(或重命名)`package.json`,再`npm init`同样会依据`node_modules`的实际安装情况,更新`dependencies`配置项
 
**cordova部分**
> `ionic state save` 更新package.json
> 会依据`plugins`的实际安装情况,更新`package.json`中的`cordovaPlugins`项
 
# 依据package.json,安装plugin或module
**npm部分**
`npm install` 或 `cnpm install`
 
**cordova部分**
`ionic state restore` 恢复及安装
> 依据package.json文件中的cordovaPlugins和cordovaPlatforms属性.来恢复应用程序的plugins 和 platforms.
可以手工填充cordovaPlugins和cordovaPlatforms属性,再使用`ionic state restore`恢复安装
 
`ionic state reset` 重置(慎用)
> <font color=red>重置方法首先将删除您的平台和插件文件夹.</font>
然后会依据你的package.json文件指定的 plugins 和 platforms重新安装


<!-- more -->
