title: 新一代包管理工具yarn
date: 2016-12-15 00:00:00
tags: [yarn]


---
`Yarn vs npm：你需要知道的一切`
https://zhuanlan.zhihu.com/p/23493436?utm_source=tuicool&utm_medium=referral
 
`新一代包管理工具yarn`
http://imweb.io/topic/581f6c0bf2e7e042172d618a?utm_source=tuicool&utm_medium=referral


```
npm install === yarn / yarn install
npm install xxx —save === yarn add xxx
npm uninstall xxx —save === yarn remove xxx
npm install xxx —save-dev === yarn add xxx —dev
npm update === yarn upgrade
npm install xxx -g === yarn global add xxx
```


---
- 新的包管理工具,主打是速度和依赖管理(可靠性)



- 同样有淘宝的仓库:
默认仓库:`https://registry.yarnpkg.com`
```
yarn config set registry " https://registry.npm.taobao.org"
```
我刚在 专家库项目上试了下,删除node_modules再用yarn可以装回去