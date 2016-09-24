title: JavaScript console控制开关
date: 2016-09-06 00:00:00
tags: [console控制]


---
- console.js ```
// Avoid `console` errors in browsers that lack a console.
(function (window) {
  var method;
  var noop = function () {
  };
  var methods = [
    'assert', 'clear', 'count', 'debug', 'dir', 'dirxml', 'error',
    'exception', 'group', 'groupCollapsed', 'groupEnd', 'info', 'log',
    'markTimeline', 'profile', 'profileEnd', 'table', 'time', 'timeEnd',
    'timeline', 'timelineEnd', 'timeStamp', 'trace', 'warn'
  ];
  var length = methods.length;
  var console = (window.console = window.console || {});
 
  while (length--) {
    method = methods[length];
 
    // Only stub undefined methods.
    if (!console[method]) {
      console[method] = noop;
    }
  }
 
  /* 关闭日志输出(关闭所有副作用很大,会导致部分项目无法启动) */
  function consoleClose() {
    console['debug'] = noop;
    console['error'] = noop;
    console['info'] = noop;
    console['log'] = noop;
    console['warn'] = noop;
    console['trace'] = noop;
  }
 
  window.consoleClose = consoleClose;
}(window));
// Place any jQuery/helper plugins in here.
```
- config.js
```
window.consoleClose();/* 关闭console日志 */

```