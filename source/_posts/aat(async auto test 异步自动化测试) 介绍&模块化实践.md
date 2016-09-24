title: aat(async auto test 异步自动化测试) 介绍&模块化实践
date: 2016-09-16 00:00:00
tags: [自动化测试]
 
---
# 关键描述
- async 步执行函数,目的:页面同步刷新(柯里化)
- sycFuncPromise 返回Promise的方法,处理连续的异步func调用.(可完全替代async)
- currying 柯里化,单行定义函数且执行函数
```
function _Each(index, length) {
  sycFuncPromise(function () {
    el('.nav-bar-title.ng-binding').triggerHandler('click');
  })()
    .then(function () {
      return sycFuncPromise(function () {
        el('.bookname_content>.bookname', index).triggerHandler('click');
        ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
      })()
    })
    .then(function () {
      // ...
    })
}
_Each(0, $('.bookname_content>.bookname').length);
```
```
currying(function _Each(index, length) {
 
  sycFuncPromise(function () {
    el('.nav-bar-title.ng-binding').triggerHandler('click');
  })()
    .then(function () {
      return sycFuncPromise(function () {
        el('.bookname_content>.bookname', index).triggerHandler('click');
        ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
      })()
    })
    .then(function () {
      // ...
    })
 
})(0, $('.bookname_content>.bookname').length);
```
 
- el 简化选择器(支持angular选择器,jquery,querySelector)
```
/**
* 简化选择器
* @param selector
* @param index
* @returns {*}
*/
function el(selector, index) {
  var index = arguments.length <= 1 || arguments[1] === undefined ? 0 : arguments[1];
  return window.angular ? angular.element(document.querySelectorAll(selector)[index]) : $(selector);
}
 
exports.el = el;
```


--- 
# 使用
## 方式一
- index.html
```
  <script src="autoTest/core.aat.js"></script>
  <script src="autoTest/task.aat.js"></script>
```

- core.aat.js (直接引用情况下会exports会使用window)

```
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports'], factory) : // AMD
      (factory((global.aat = global.aat || {}))); // window
}(this, (function (exports) {
  'use strict';
 
  exports.xxx = '';
 
})));


// ---
或者,直接使用window进行赋值.
```
- task.aat.js   (直接引用情况下会exports会使用window)
```
 
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports'], factory) : // AMD
      (factory((global.aat = global.aat || {}))); // window
}(this, (function (exports) {
  'use strict';
 
  exports.sss = '';
 
})));
// ---
或者,直接使用window进行赋值.
```


## 方式二



---
#  require.js 模块化方式
- index.html
```
<script data-main="js/index" src="lib/require.js"></script><!--  模块化入口 -->

```
- js/index.js 配置初始化
- requirejs(['aat_core', 'aat'], function (aat_core, task) 对象初始化 ```
requirejs.config({
  baseUrl: '',
  paths: {
    aat_core: 'autoTest/core.aat',
    aat: 'autoTest/task.aat'
  }
});
 
// Start the main app
requirejs(['aat_core', 'aat'], function (aat_core,  aat ) {
  console.log( aat );// 初始化验证
  window.aat =  aat ;
});
```
- core.aat.js (模块化引用情况下会exports会使用 AMD )
```
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports'], factory) : // AMD
      (factory((global.aat = global.aat || {}))); // window
}(this, (function (exports) {
  'use strict';
 
  exports.xxx = '';
 
})));
```
- task.aat.js   ( 模块化 引用情况下会exports会使用 AMD )
> require('aat_core') 注意需要 requirejs.config初始化`aat_core`且 ` requirejs(['aat_core', 'aat'], function (aat_core,  aat )`完成初始化
```
 
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports'], factory) : // AMD
      (factory((global.aat = global.aat || {}))); // window
}(this, (function (exports) {
  'use strict';
 
    /** 引入模块接口(规范模块化,兼容AMD) */
  var core = (typeof require === 'function') ? require('aat_core') : exports;
  var sycFuncPromise = core.sycFuncPromise;
  var async = core.async;
  var currying = core.currying;
  var el = core.el;


  exports.sss = '';
 
})));
```


--- 
# 使用时初始化(使用文件名)
例: ` task.aat.js`  
>define(function (require, exports, module) {  require('path/filename');  })


- js/index.js 配置初始化
- requirejs(['aat'], function ( aat ) 对象初始化(仅初始化主函数) ```
requirejs.config({
  baseUrl: '',
  paths: {
    aat: 'autoTest/task.aat'
  }
});
 
// Start the main app
requirejs(['aat'], function ( aat ) {
  console.log( aat );// 初始化验证
  window.aat =  aat ;
});
```


- task.aat.js   ( 模块化 引用情况下会exports会使用 AMD )
> require(' autoTest/aat_core' ') 可不需要 requirejs.config初始化`aat_core`及 ` requirejs(['aat_core', 'aat'], function (aat_core,  aat )`
```
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(factory) : // AMD
      (factory(null,(global.aat = global.aat || {}))); // window
}(this, (function ( require, exports) {
  'use strict';
 
    /** 引入模块接口(规范模块化,兼容AMD) */
  var core = require('autoTest/aat_core')
  var sycFuncPromise = core.sycFuncPromise;
  var async = core.async;
  var currying = core.currying;
  var el = core.el;


  exports.sss = '';
 
})));
```
- 简易参考
```
//这里按照commonjs的形式让requirejs自己去搜一遍，再引入third.js
define(function (require, exports, module) {
    var third =require('third');
    third();
    module.exports.action = function () {console.log(22)};
});
```


---
# 可能遇到问题 & 处理办法
## 启动异常:Uncaught Error: Mismatched anonymous define() module: function

### 问题原因
引入` require.js`后,所有的` define()`定义模块都应该使用` requirejs(['aat_core', 'aat'], `初始化,不应该<Script>直接引入
- 分析:
```
引用的requirejs跟引用的其他js引起了冲突,其他js中也定义的define这个变量.
比如你引用了这样的js:
<script src="services/require.js"></script>
<script src="services/Utility.js"></script>
出现了Mismatched anonymous define() module这种错误.那么意味着Utility.js中也定义了define这样的变量.所以在使用中你只引用require.js
然后 require(['Utility'],function(){}) 使用Utility.js 就不会出现错误了.
该错误归根结底就是 define已经被定义了.或者在之后的js中被重写了.
```
`Mismatched anonymous define() module:这个怎么解决requirejs中的_百度知道`
http://zhidao.baidu.com/link?url=g9Lh6tofr3U7-nPUDqbqk6ifHxLH8PLwkDQ039biy3bhwwCGRyfDkonLaDUFMgoDj-UyiajnmSjO0MvtcCjdjXKvTlNOoMDMk_YpGlyODi7


### 处理办法
方法一: 调整`<script src="require.js"></script>`引入时机


方法二: 依据规范,通过 ` requirejs(['aat_core', 'aat'], `初始化
` javascript - Uncaught Error: Mismatched anonymous define() module: function definition(name, global) - Stack Overflow `
http://stackoverflow.com/questions/23796019/uncaught-error-mismatched-anonymous-define-module-function-definitionname



方法三: 修改`<Script>`引入方法
```
<script>
  var ace = require("./path/to/ace");
</script>
```
`javascript - loading Ace causes Uncaught Error: Mismatched anonymous define() module: - Stack Overflow`
http://stackoverflow.com/questions/23224341/loading-ace-causes-uncaught-error-mismatched-anonymous-define-module



---
## 启动异常:Uncaught Error: Module name "aat_core" has not been loaded yet for context: _. Use require([])
```
require.js:168 Uncaught Error: Module name "aat_core" has not been loaded yet for context: _. Use require([])
http://requirejs.org/docs/errors.html#notloadedmakeError
     @ require.js:168localRequire
     @ require.js:1433requirejs
     @ require.js:1794(anonymous function)
    @ task.aat.js:18execCb
    @ require.js:1693check
    @ require.js:881enable
    @ require.js:1173init
    @ require.js:786callGetModule
    @ require.js:1200completeLoad
    @ require.js:1587onScriptLoad
    @ require.js:1714
ng_http.factory.js:276 response: 34 0.397s 查询成功 Object {object: Object}
```
- 问题原因

偶现：资源的异步加载，当使用了未初始化的对象时异常
依赖：未明确依赖，需要维护依赖关系


- 解决办法
```
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports','aat_core'], factory) : // AMD
      (factory((global.aat = global.aat || {}))); // window
}(this, (function (exports) {
  'use strict';
 
  var core = (typeof require === 'function') ? require('aat_core') : exports;
  /** 引入模块接口(规范模块化,兼容AMD) */
  var sycFuncPromise = core.sycFuncPromise;
  var async = core.async;
  var currying = core.currying;
  var el = core.el;
 
  // ...
 
})))
```


`理解和解决requireJS的报错:MODULE NAME HAS NOT BEEN LOADED YET FOR CONTEXT`
http://blog.csdn.net/aitangyong/article/details/40708025


`javascript - Dynamic require in RequireJS, getting "Module name has not been loaded yet for context" error? - Stack Overflow`
http://stackoverflow.com/questions/17446844/dynamic-require-in-requirejs-getting-module-name-has-not-been-loaded-yet-for-c






---
# 类似技术,开源技术
`搭建基于Karma和Jasmine的前端单元测试 - 简书`
www.jianshu.com/p/a7ffb564734e


更多:`  简书-搜索: karma`
http://www.jianshu.com/search?q=karma&page=1&type=notes


`webpro/Automated-SPA-Testing: Automated unit & functional testing for web applications`
https://github.com/webpro/Automated-SPA-Testing

