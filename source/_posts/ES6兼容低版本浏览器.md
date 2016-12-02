title: ES6兼容低版本浏览器
date: 2016-10-11 00:00:00
tags: [ ES6]


---
#  局部引用 &  全局实现
想要不支持该方法的浏览器支持，无非两种办法
`局部引用`，引入一个相同的方法代替，其缺点则是使用起来比较麻烦，每个用到的文件都要去引入。
`全局实现`，与之相反的方法是使用 polyfill ，其优点便是使用方便，缺点则是会全局污染，特别是实例方法，涉及到修改其 prototype ，不是你的类，你去修改它原型是不推荐的。
 
针对这两种办法，提供出以下几种方案，供大家参考
 
# 方案一：引入额外的库
```
import assign from 'object-assign';
assign({}, {});
```
缺点就是需要去找到对应的库，比如 Promise 我们可以使用 lie



# 方案二：全局引入 babel-polyfill

```
import 'babel-polyfill';

```
polyfill 构建并 uglify 后的大小为 98k，gzip 后为32.6k，32k 对与移动端还是有点大的



# 方案三：手动引入 core-js
```
Object: {
      assign: "object/assign",
      create: "object/create",
      defineProperties: "object/define-properties",
      defineProperty: "object/define-property",
      entries: "object/entries",
      freeze: "object/freeze",
      ...
  }
```
```
import assign from 'core-js/library/fn/object/assign'
```

或
```

import 'core-js/fn/object/assign'
```


# 方案四：使用 `babel-plugin-transform-runtime`

https://babeljs.io/docs/plugins/transform-runtime/
```
$ npm install --save-dev babel-plugin-transform-runtime
$ npm install --save babel-runtime

```
- `.babelrc`
```
// without options
{
  "plugins": ["transform-runtime"]
}
 
// with options
{
  "plugins": [
    ["transform-runtime", {
     "helpers": false, // defaults to true; v6.12.0 (2016-07-27) 新增;
      "polyfill": true, // defaults to true
      "regenerator": true, // defaults to true
      // v6.15.0 (2016-08-31) 新增
      // defaults to "babel-runtime"
      // 可以这样配置
      // moduleName: path.dirname(require.resolve('babel-runtime/package'))
      "moduleName": "babel-runtime"
    }]
  ]
}
```
- 最后看下终极方案的通用配置
```
{
  plugins: [
    ["transform-runtime", {
      "helpers": false,
      "polyfill": true,
      "regenerator": true
    }],
    'add-module-exports',
    'transform-es3-member-expression-literals',
    'transform-es3-property-literals',
  ],
  "presets": [
    'react',
    'es2015',
    'stage-1'
  ],
}
```


---
`ES6 + Webpack + React + Babel 如何在低版本浏览器上愉快的玩耍（下） - 推酷` http://www.tuicool.com/articles/rYRBbiA


`开始使用es6的module:import+export - 推酷`
http://www.tuicool.com/articles/zABbYbv


`Polyfill.io - 自动化的 JavaScript Polyfill 服务 | 小影志`
http://c7sky.com/polyfill-io.html
