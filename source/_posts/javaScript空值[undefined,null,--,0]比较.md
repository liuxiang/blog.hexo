title: javaScript空值[undefined,null,'',0]比较
date: 2017-4-9 00:00:00
tags: [ 空值 ]


---


![]( https://segmentfault.com/img/bVkpiW)


```
var a;// undefined
var aa = null;
var aaa = '';
var aaaa = 0;


console.log(isNaN(a)); // true
console.log(isNaN(aa));
console.log(isNaN(aaa));
console.log(isNaN(aaaa));
console.log('--------------------------------');


console.log(a === undefined);// true
console.log(a == undefined);// true


console.log(aa === undefined);
console.log(aa == undefined);// true


console.log(aaa === undefined);
console.log(aaa == undefined);


console.log(aaaa === undefined);
console.log(aaaa == undefined);
console.log('--------------------------------');


console.log(a === null);
console.log(a == null);// true


console.log(aa === null);// true
console.log(aa == null);// true


console.log(aaa === null);
console.log(aaa == null);


console.log(aaaa === null);
console.log(aaaa == null);
console.log('--------------------------------');


console.log(a === '');
console.log(a == '');


console.log(aa === '');
console.log(aa == '');// true


console.log(aaa === '');// true
console.log(aaa == '');


console.log(aaaa === '');
console.log(aaaa == '');// true
console.log('--------------------------------');




console.log(!a);// true
console.log(!aa);// true
console.log(!aaa);// true
console.log(!aaaa);// true
```


` undefined与null的区别 - 阮一峰的网络日志 `
http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html

https://segmentfault.com/a/1190000002441634


`细说 Javascript 拾遗篇（二） : undefined 和 null - StephenLee - SegmentFault`
https://segmentfault.com/a/1190000000512023


`js判断undefined类型,undefined,null, 的区别详细解析 - 博客频道 - CSDN.NET`
http://blog.csdn.net/qq_34117825/article/details/52325685


`javascript类型系统——undefined和null - 推酷`
http://www.tuicool.com/articles/b2MVJ3
