title: 现代浏览器中内置的几个可以等效替代jQuery的功能
date: 2016-08-30 00:00:00
tags: [替代jQuery]


---
# 获取元素选择器是一样的[selector选择器] ```
$('.contant>h1');
[<h1>现代浏览器中内置的几个可以等效替代jQuery的功能</h1>]


document.querySelector('.contant>h1');
<h1>现代浏览器中内置的几个可以等效替代jQuery的功能</h1>

```


# API依然存在差异
```
// jQuery

$('.contant>h1').html();
"现代浏览器中内置的几个可以等效替代jQuery的功能"


// querySelector
document.querySelector('.contant>h1').html();
VM1257:1 Uncaught TypeError: document.querySelector(...).html is not a function(…)
```
```
// jQuery

$(".contant>h1").css('background-color',"salmon");



// querySelector
document.querySelector('.contant>h1').style.backgroundColor="salmon";

```
```
// jQuery方式
$("#picture").attr("src", "http://placekitten.com/200/200");
 
//使用将querySelector映射为$的原生js方式
$("#picture").src = "http://placekitten.com/200/200";
```
# 表达式 `$` 统一  
>并不建议,因为除选择器以外的API都不一样.而使用统样的表达式`$`程序员容易混淆,犯错
```
<script>
  // 创建全局的 '$' 变量
  window.$ = function(selector) {
    return document.querySelector(selector);
  };
 
  (function() {
    // 通过id查找item1，将它的背景颜色修改为浅红
    var item = $("#salmon").style.backgroundColor="salmon";
    console.log(item);
  }()); 
</script>
```


# document.queryselectorall 返回的是一个节点列表
一个很巧妙的做法是将querySelector映射为\$，而将querySelectorAll函数映射为$$。但它返回的是一个节点列表，不如jQuery里返回的Array格式好用，我们可以使用Array.prototype.slice.call(nodeList)方法加工一下，让它更方便。



`现代浏览器中内置的几个可以等效替代jQuery的功能 - 推酷`
http://www.tuicool.com/articles/2y2Qvy2


---


# angular环境使用` angular.element()`
```
angular.element()
JQLite {}
```
```
方法一：

var test = angular.element(document.querySelector('#testId'));
test.addClass(‘testClass’);


方法二：

var test = angular.element(document.getElementById('test');
test.addClass(‘testClass’);


方法三：

angular.forEach(angular.element(document).find('div'),function(node){
            if(node.id == 'testId'){
                     node.addClass('testClass');
            }
if(node.className == ‘testClass’){
node.removeClass(‘testClass’)
}
})


方法四：使用$documen.

$document就和angular.element(document)是一样的

angular.forEach($document.find('div'),function(node){
            if(node.id == 'testId'){
                     node.addClass('testClass');
            }
if(node.className == 'testClass'){
node.removeClass(‘testClass’)
}
           })
```


---
`Angular.element和$document的使用方法分析，代替jquery - Simon_ITer的个人空间 - 开源中国社区`
http://my.oschina.net/keysITer/blog/626746


`Angular's jqLite`
http://devdocs.io/angularjs~1.5/api/ng/function/angular.element

http://www.cnblogs.com/ys-ys/p/4918985.html


`angular源码分析：angular的整个加载流程 - 王大鹏 - 博客园`
http://www.cnblogs.com/web2-developer/p/angular-7.html


`angular源码分析：angular中jqLite的实现——你可以丢掉jQuery了 - 王大鹏 - 博客园`
http://www.cnblogs.com/web2-developer/archive/2015/11/09/angular-5.html
