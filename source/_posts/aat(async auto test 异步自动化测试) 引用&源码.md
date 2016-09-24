title: aat(async auto test 异步自动化测试) 引用&源码
date: 2016-09-13 00:00:02
tags: [自动化测试]
 
---
# 引用
### 方式一
- index.html
```
  <script src="autoTest/core.aat.js"></script>
  <script src="autoTest/task.aat.js"></script>
```


### 方式二
# require.js 模块化方式
- index.html
```
<script data-main="js/index" src="lib/require.js"></script><!--  模块化入口 -->
```
- js/index.js 配置初始化
- requirejs(['aat_core', 'aat'], function (aat_core, task) 对象初始化
```
requirejs.config({
  baseUrl: '',
  paths: {
    aat_core: 'autoTest/core.aat',
    aat: 'autoTest/task.aat'
  }
});
 
// Start the main app
requirejs(['aat_core', 'aat'], function (aat_core, aat) {
  console.log(aat);// 初始化验证
  window.aat = aat;
});
```
>更具体见`autoTest/README.md`


---
# 源码
- core.aat.js
```
/**
* Created by Administrator on 2016/9/13 0013.
*/
 
 
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports'], factory) : // AMD
      (factory((global.aat = global.aat || {}))); // window
}(this, (function (exports) {
  'use strict';
 
  /**
   * 返回Promise的方法,处理连续的异步func调用
   * @param func
   * @returns {Function}
   */
  function sycFuncPromise(func, time) {
    return function () {
      var _arguments = arguments;
      return new Promise(function (resolve, reject) { // promise方式
        async(function () {
          func(_arguments);
          resolve();// 执行完毕,推送成功
        }, time)();
      })
    }
  }
 
  /**
   * 柯里化
   * @param func
   * @returns {Function}
   * @private
   */
  function currying(func) {
    var _arguments = arguments;
    return function () {
      // return func.call(this, ...arguments, ..._arguments);
      return func.apply(this, arguments, _arguments);
    }
  }
 
  /**
   * 异步执行函数,目的:页面同步刷新(柯里化)
   * 经过调试,在angular应用中700ms比较合适(原因是路由间动画需要时间)
   * @param func
   * @param time
   * @returns {Function}
   */
  function async(func, time) {
    var time = arguments.length <= 1 || arguments[1] === undefined ? 700 : arguments[1];
    return function () {
      var _arguments = arguments;
      setTimeout(function () {
        // return func.call(this, ..._arguments);
        return func.apply(this, _arguments);
      }, time)
    }
  }
 
  /** 出口 */
  // global.aat = {
  //   sycFuncPromise: sycFuncPromise, // 返回Promise的方法,处理连续的异步func调用
  //   async: async,                   // 异步执行函数
  //   currying: currying              // 柯里化
  // };
 
  exports.sycFuncPromise = sycFuncPromise;
  exports.async = async;
  exports.currying = currying;
  /*******************************************************/
 
  /**
   * 简化选择器
   * @param selector
   * @param index
   * @returns {*}
   */
  function el(selector, index) {
    var index = arguments.length <= 1 || arguments[1] === undefined ? 0 : arguments[1];
    return typeof angular === 'object'
      ? angular.element(document.querySelectorAll(selector)[index])
      : (window.jQuery ? $(selector) : document.querySelectorAll(selector)[index]);
  }
 
  exports.el = el;
 
  /*******************************************************/
 
  /**
   * 同步睡眠,如:sleep(3000);
   * @param delay
   */
  function sleep(delay) {
    var start = new Date().getTime();
    while (new Date().getTime() < start + delay);
  }
 
  /**
   * 强行重绘
   */
  function refresh() {
    // 对面板强行重绘
    $("ion-content").css('display', 'none');
    document.body.offsetWidth;// 在样式变化间加入计算,可强制提交之前的样式变化。否则浏览器会进行事务优化，事务前后无变化的不再重绘
    $("ion-content").css('display', '');
 
  }
 
})));
```


- task.aat.js
```
/**
* Created by Administrator on 2016/9/15 0015.
*/
 
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports', 'aat_core'], factory) : // AMD
      (factory((global.aat = global.aat || {}))); // window
}(this, (function (exports) {
  'use strict';
 
  /** 引入模块接口(待规范模块化) */
    // var sycFuncPromise = global.aat.sycFuncPromise;
    // var async = global.aat.async;
    // var currying = global.aat.currying;
 
  var core = (typeof require === 'function') ? require('aat_core') : exports;
 
  /** 引入模块接口(规范模块化,兼容AMD) */
  var sycFuncPromise = core.sycFuncPromise;
  var async = core.async;
  var currying = core.currying;
  var el = core.el;
 
  /*********************************************************************/
 
  function init() {
  }
 
  var task = {
      chioseBook: function () {
        if (!$('.choice-content').hasClass('ng-hide')) {
          async(function (prom) {
            el('.nav-bar-title.ng-binding').triggerHandler('click')
          })()
        }
 
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
 
      },
      openChapter: function () {
 
        currying(function _Each(index, length) {
          sycFuncPromise(function () {
            var selector = '.sections';
            var event = {type: 'click', currentTarget: document.querySelectorAll(selector)[index]};
            el(selector, index).triggerHandler(event);// 打开章节
          })()
            .then(function () {
              return sycFuncPromise(function () {
                $('.back-button').click();// 返回
                ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
              })()
            })
            .then(function () {
              // ...
            })
        })(0, $('.sections').length);
 
      },
      practice: function () {
        currying(function _Each(index, length) {
            sycFuncPromise(function () {
              var selector = '.sections';
              var event = {type: 'click', currentTarget: document.querySelectorAll(selector)[index]};
              el(selector, index).triggerHandler(event);// 打开章节
            })()
              .then(function () {
                return new Promise(function (resolve, reject) {
                  sycFuncPromise(function () {
                    el('.practice_inlet').triggerHandler('click');// 打开练习
 
                    new Promise(function (resolve, reject) {
                      currying(function _Each(index, length) {
                        sycFuncPromise(function () {
                          el('.option', ~~(Math.random() * 4)).triggerHandler('click');// 打开1
                          ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
                          index >= length && resolve();// 执行完毕,通知下一步环境
                        })()
                      })(0, 5);
                    }).then(function () {
                      sycFuncPromise(function () {
                        $('.back-button').click();// 返回
                        resolve();
                      })();
                    })
                  })()
                })
              })
              .then(function () {
                return sycFuncPromise(function () {
                  $('.back-button').click();// 返回
                  ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
                })()
              })
              .then(function () {
                // ...
              })
          }
        )(0, $('.sections').length);
      },
      openExp_sign: function (index) {
        currying(function _Each(index, length) {
          sycFuncPromise(function () {
            el('.pointItem', index).triggerHandler('click');// 打开知识点(实验)
          })()
            .then(function () {
              return sycFuncPromise(function () {
                el('.closeExp').triggerHandler('click');// 关闭实验
                // 此次error: Blocked a frame with origin "http://localhost:3342" from accessing a cross-origin frame.
                ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
                //index >= length && resolve();// 执行完毕,通知下一步环境
              }, 8000)();// 时间放大为3s
            })
            .then(function () {
              //...
            })
        })(0, $('.pointItem').length);
      }
      ,
      openExp: function () {
        currying(function _Each(index, length) {
          sycFuncPromise(function () {
            var selector = '.sections';
            var event = {type: 'click', currentTarget: document.querySelectorAll(selector)[index]};
            el(selector, index).triggerHandler(event);// 打开章节
          }, 1500)()
            .then(function () {
              return new Promise(function (resolve, reject) {
                async(function () {
 
                  currying(function _Each(index, length) {
                    sycFuncPromise(function () {
                      el('.pointItem', index).triggerHandler('click');// 打开知识点(实验)
                    }, 2000)().then(function () {
                      return sycFuncPromise(function () {
                        el('.closeExp').triggerHandler('click');// 关闭实验
                        ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
                        index >= length && resolve();// 执行完毕,通知下一步环境
                      }, 8000)();// 时间放大为3s
                    })
 
                  })(0, $('.pointItem').length);
 
                }, 1500)();
 
              })
            })
            .then(function () {
              return sycFuncPromise(function () {
                $('.back-button').click();// 返回
                ++index < length && _Each(index, length);// 递归调用自己,相比for没有了时间计算
              }, 1500)()
            })
            .then(function () {
              // ...
            })
        })(0, $('.sections').length);
      }
    }
    ;
 
  var taskItems = [
    {name: '切换分册', action: task.chioseBook},
    {name: '打开本册章节', action: task.openChapter},
    // {name: '单个章节实验', action: task.openExp_sign},
    {name: '本册所有实验(移动端)', action: task.openExp},
    {name: '本册练习', action: task.practice}
  ];
 
  /**
   * 出口
   * aat:Asynchronous Auto Test
   * 异步自动化测试
   */
// global.aat = {
//   init: init,
//   task: task,
//   taskf: taskf
// };
 
  exports.init = init;
  exports.task = task;
  exports.taskItems = taskItems;
 
})))
;
 
/** 简易示例 */
// define(function (require, exports, module) {
//   var core = require('autoTest/core');
//   console.log('core', core);
//   module.exports.action = function () {
//     console.log(22)
//   };
// });
```