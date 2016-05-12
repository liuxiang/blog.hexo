title: 客户端将图片转Base64
date: 2016-05-05 00:00:00
categories: 图片转换为Base64
tags: [ 图片转换为Base64 ]


---


* 利于canvas
* canvas 在` toDataURL `不支持跨域图片，异常：` Tainted canvases may not be exported `
* 可使用` img.crossOrigin = "Anonymous"`或` img.crossOrigin = "*" `开启客户端跨域允许，解决


```
function getBase64Image(img) {
    var canvas = document.createElement("canvas");
    canvas.width = img.width;
    canvas.height = img.height;
 
    var ctx = canvas.getContext("2d");
    ctx.drawImage(img, 0, 0, img.width, img.height);
 
    var dataURL = canvas.toDataURL("image/png");
    return dataURL
 
    // return dataURL.replace("data:image/png;base64,", "");
  }
 
 
  function main() {
    var img = document.createElement('img');
    img.crossOrigin = "Anonymous";// 开启客户端允许跨域
    img.src = 'http://7xnbs3.com1.z0.glb.clouddn.com/16-5-4/24762894.jpg';
    img.crossOrigin = "Anonymous";// Tainted canvases may not be exported.
 
    img.onload = function () {
      var data = getBase64Image(img);
      console.log(data);
    }
 
    document.body.appendChild(img);
  }
 
  main()
```
或
```
<img crossOrigin="Anonymous"  onload="function(){...}" ></img>
```


---
**参考**


`如何把<img>元素里面的图片的base64编码获取？`

https://segmentfault.com/q/1010000000651225



`如何在 chrome extension 中使用 canvas 的 `toDataURL` 方法？`
https://segmentfault.com/q/1010000002459456/a-1020000002459474


`image 跨域访问代码求解释`
https://segmentfault.com/q/1010000000768672/a-1020000002436172



`javascript - Tainted canvases may not be exported - Stack Overflow`
http://stackoverflow.com/questions/22710627/tainted-canvases-may-not-be-exported


<!-- more -->
