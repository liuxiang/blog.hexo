title: userScript——推酷,展示全部收藏. 便于搜索
date: 2016-04-21 00:00:00
categories:  userScript
tags: [ userScript ]


---

```
// 推酷-展示全部收藏。便于搜索
for (var i = 1; i <= 100; i++) {
  $('.span3.small-span-margin.container-body').before('<div class="newPage_' + i + '">newPage</div>');
  $('.newPage_' + i + '').load("http://www.tuicool.com/articles/my?pn=" + i + " .span9.container-body",
    function (response, status, xhr) {
      if (response.indexOf('还没有收藏的文章哦') > 0) return;
      // alert(response.indexOf('还没有收藏的文章哦')); TODO 因为异步,不能及时断开，开始会请求多次
    });
}
```


```
// 推酷-展示全部收藏。便于搜索
for (var i = 1; i <= 50; i++) {
  $('.span3.small-span-margin.container-body').before('<div class="newPage_' + i + '">newPage</div>');
  $('.newPage_' + i + '').load("http://www.tuicool.com/articles/my?pn=" + i + " .span9.container-body" );
}
```


# 使用
1.打开推酷收藏页面
2.F12进行调试器
3.控制台执行以上代码即可


# 书签实现(错误)
```
javascript:(
    for (var i = 1; i <= 100; i++) {
      $('.span3.small-span-margin.container-body').before('<div class="newPage_' + i + '">newPage</div>');
      $('.newPage_' + i + '').load("http://www.tuicool.com/articles/my?pn=" + i + " .span9.container-body",
        function (response, status, xhr) {
          if (response.indexOf('还没有收藏的文章哦') > 0) return;
        });
    }
)

```
`Error:` `ncaught SyntaxError: Unexpected token for`


# userScript(调试中...)
`tuicool collection fullPage.user.js` 文件命名
```
// ==UserScript==
// @name      tuicool collection fullPage
// @namespace 
// @version    0.1
// @description  enter something useful
// @match      http://www.tuicool.com/articles/my
// @copyright  2016, You
// @require        http://ajax.googleapis.com/ajax/libs/jquery/1.4/jquery.min.js
// ==/UserScript==
 

function userFunction(){
    alert($('.active:eq(1)').html())
    $('.active:eq(1)').html($('.active:eq(1)').html()+'(所有)');
    $('.active:eq(1)').click(function(){
      for (var i = 1; i <= 50; i++) {
        $('.span3.small-span-margin.container-body').before('<div class="newPage_' + i + '">newPage</div>');
        $('.newPage_' + i + '').load("http://www.tuicool.com/articles/my?pn=" + i + " .span9.container-body");
      }
    })
}
 
setTimeout(userFunction,3000);
window.onload = userFunction;
document.ready = function(){
alert($('.active:eq(1)').html())
}
```


<!-- more -->

