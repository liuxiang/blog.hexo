title: npm 常用命令&cnpm镜像
date: 2015-12-26 00:00:02 #发表日期，一般不改动
categories: npm  #文章文类
tags: [ npm,cnpm ]



---
# npm常用命令


`npm install <name>` 安装nodejs的依赖包
 
例如npm install express 就会默认安装express的最新版本，也可以通过在后面加版本号的方式安装指定版本，如npm install express@3.0.6
 
`npm install <name> -g`  将包安装到全局环境中
 
但是代码中，直接通过require()的方式是没有办法调用全局安装的包的。全局的安装是供命令行使用的，就好像全局安装了vmarket后，就可以在命令行中直接运行vm命令
 
`npm install <name> --save`  安装的同时，将信息写入package.json中
 
项目路径中如果有package.json文件时，直接使用npm install方法就可以根据dependencies配置安装所有的依赖包
 
这样代码提交到github时，就不用提交node_modules这个文件夹了。
 
`npm init`  会引导你创建一个package.json文件，包括名称、版本、作者这些信息等
 
`npm remove <name>` 或`cnpm uninstall <name>` 移除
 
`npm update <name>`更新
 
`npm ls` 列出当前安装的了所有包
 
`npm root` 查看当前包的安装路径
 
`npm root -g`  查看全局的包的安装路径
`cnpm root -g`  查看全局的包的安装路径

 
`npm help`  `npm help init` 帮助，如果要单独查看install命令的帮助，可以使用的npm help install
 
帮助文档： https://npmjs.org/doc/  - `CLI Commands`


** 参考 **
npm常用命令

http://blog.csdn.net/haidaochen/article/details/8546796


---


` npm config set registry  https://registry.npm.taobao.org `  设置代理



`npm config get registry ` 获取npm当前镜像地址
`cnpm config get registry ` 获取c npm当前镜像地址
`npm info underscore` 配置查看



`npm config delete <key>` 删除配置


`npm config edit` 在编辑器中打开npm配置文件



`npm cache clean` 清理缓存



`npm rebuild node-sass` 重新编译模块



`npm ls ionic` 当前目录是否安装ionic此node模块

`npm ls ionic -g` 全局目录是否安装ionic此node模块


**参考**

`npm-node模块管理工具 命令概述`
http://www.gowhich.com/blog/665


`npm镜像使用方法 - 这个人很懒 - 开源中国社区`
http://my.oschina.net/mustang/blog/263435?fromerr=BuKV7UnY

---


# 安装cnpm,淘宝npm镜像
 
你可以使用我们定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
 ```
$ npm install -g cnpm --registry= https://registry.npm.taobao.org
```
或者你直接通过添加 npm 参数 alias 一个新命令:
```
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"
 
# Or alias it in .bashrc or .zshrc
$ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
```


## 安装模块

```

$ cnpm install [name]

```


## 使用同步模块
当cnpm镜像中没有某模块或版本过旧时,可以通过`sync`同步.后再使用`cnpm install`安装
```
$ cnpm sync connect

或
$ open https://npm.taobao.org/sync/connect

```


<!-- more -->

## 其它命令  
支持 npm 除了 publish 之外的所有命令, 如:
```
$ cnpm info connect
```


---



**参考**
`淘宝  NPM 镜像`
http://npm.taobao.org/



`快速搭建 Node.js 开发环境以及加速 npm`

http://segmentfault.com/a/1190000000454456


`官方npm完整文档`
https://npmjs.org/doc/ - `CLI Commands`



<!-- more -->
