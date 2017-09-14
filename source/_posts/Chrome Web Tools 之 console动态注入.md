title: Chrome Web Tools 之 console动态注入
date: 2016-1-8 00:00:00 #发表日期，一般不改动
categories:  Chrome Web Tools  #文章文类
tags: [ Chrome Web Tools ,console ]


---
# Chrome Web Tools 之 console动态注入
chrome浏览器-console输入:
console.inject('jquery');

console.inject('d3');

...


* 范围 `https://api.cdnjs.com/libraries`


# 源码
```
console.inject = function (library) {
 
      function getURLs() {
        var xmlhttp = new XMLHttpRequest();
 
        xmlhttp.onreadystatechange = function() {
          if (xmlhttp.readyState == XMLHttpRequest.DONE ) {
            if(xmlhttp.status == 200){
              var libraries = JSON.parse(xmlhttp.responseText).results;
              var foundLib = libraries.reduce(function (found, item) {
                if (item.name === library || item.name === library + '.js') {
                  found = item;
                }
                return found;
              }, undefined);
 
              if (foundLib) {
                var url = foundLib.latest.replace('http:', 'https:');
                var libScript =document.createElement('script');
                libScript.src = url;
                document.head.appendChild(libScript);
                return console.log('library injected from ' + url);
              } else {
                console.log('library "' + library + '" not found');
              }
            }
            else { console.log(XMLHttpRequestlhttp.status)}
          }
        }
 
        var searchString = 'https://api.cdnjs.com/libraries?search=' + library;
        xmlhttp.open("GET", searchString, true);
        xmlhttp.send();
      }
 
      getURLs();
    }


console.inject('jquery'); 

```


**参考**
chrome插件`Console Injector`
https://chrome.google.com/webstore/detail/console-injector/abdfbnapkafgcheofcijaieahcbjnpkd?utm_source=chrome-ntp-icon
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/45182493.jpg)


**类似插件**
`jquery-injector` 区别是主动注入jquery

https://chrome.google.com/webstore/detail/jquery-injector/indebdooekgjhkncmgbkeopjebofdoid/related?utm_source=chrome-ntp-icon
注入成功与否确认 `$().jquery`


<!-- more -->
