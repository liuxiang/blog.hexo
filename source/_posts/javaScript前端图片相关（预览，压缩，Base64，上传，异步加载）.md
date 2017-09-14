
title: javaScript前端图片相关（预览，压缩，Base64，上传，异步加载）
date: 2017-4-1 00:00:00
tags: [ 前端图片 ]


---


# 预览
- 获取上传前,存在浏览器中的图片地址（或对象）
```
<input type="file">
input.on.('change', preview);
function preview(e) {
    var file = e.target.files[0];// 获取到input-file的文件对象
    ...
}
```


- 浏览器缓存图片的`base64`  `[ window.URL.createObjectURL(file) ]` （短）
```
<input type="file" name="imgOne" id="imgOne" onchange="preImg(this.id,'imgPre');" />
<img id="imgPre" src="" width="300px" height="300px" style="display: block;" />
```
```
window.URL.createObjectURL(document.getElementById('imgOne').files[0]);
// 结果： "blob:null/1342ae24-b5da-4ec3-b05d-f385387bd881"
```
```
document.getElementById(' imgPre ').src=' blob:null/1342ae24-b5da-4ec3-b05d-f385387bd881 ';
```
`JS预览图像将本地图片显示到浏览器上`
http://www.jb51.net/article/40864.htm
 
 
- 实际上传图片的`base64`  `[ new FileReader().readAsDataURL (file) ]` （长）
```
<input type="file" onchange="previewFile()"><br>
<img src="" height="200" width="300" alt="Image preview..."/>
```
```
var reader = new FileReader();
reader.onloadend = function () {
   preview.src = reader.result;// Base64内容
}
reader.readAsDataURL( document.querySelector('input[type=file]').files[0] );
```
`轻松实现js图片预览功能`
http://www.jb51.net/article/78215.htm
https://www.oschina.net/code/snippet_819257_22844


注意: 
在手机设备上读取临时图片数据会存在权限问题


`ionic 图片拍照；选择上传 ngcordova cordovaFileTransfer cordovaCamera - u012922981的专栏`
http://blog.csdn.net/u012922981/article/details/50924515


---
#   本地图片(前端)` 裁剪`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/31992448.jpg)
https://github.com/fengyuanchen/cropper
 
---
#   本地图片(前端)` 压缩` - 方向1： 借助 canvas 对选择的图片进行处理
`javascript - 在移动端怎样上传图片？，而且在上传前把图片压缩一定的大小？ - SegmentFault`
https://segmentfault.com/q/1010000006749744?_ea=1139187
 
 
- 方向2：借助插件 lrz.mobile.min.js
 
`[lrz.bundle.js 前端图片压缩] think2011/localResizeIMG: 前端本地客户端压缩图片，兼容IOS，Android，PC、自动按需加载文件`
https://github.com/think2011/LocalResizeIMG
 
---
#  Base64
- 利于`canvas`
 
更多见：`客户端将图片对象转Base64.md`
 
- 利用`new FileReader`
 
 
---
#  异步加载、 模糊显示
jQuery Lazy Load 图片延迟加载
 
更多见： `图片异步加载方案.md`
 
---
 
# 上传
## 方案一 ： `form`表单
- 前端`form`  (非异步，页面被刷新)
```
<form id="myForm" action="${pageContext.request.contextPath}/message/saveMessage.do" method="post">
<input type="file" name="file" capture="camera" id="inputFile">上传图片
 
<input type="submit" class="btn-submit" value="提交" id="submit">
```
后端 `MultipartFile`
 
```
@RequestMapping(method=RequestMethod.POST,value="/upload/image")
public void uploadImage(@RequestParam(required = false, value = "file")MultipartFile file
,HttpServletRequest request,HttpServletResponse response)throws Exception{
String url = getUploadUrl(file, request, response);
if(StringUtil.isEmpty(url)){
return;
}
JsonResult result = initJsonResult(url, 1, 1, Constants.JSON_RESULT_SUCCESS, null);
responseWriter(request, response, result);
}
```
例二
```
/**
 * 变更图标库图标
 * (前端请求需要注意,文件字段名必须为file,与此声明对应/如不声明则需与字段名一致-否则无法获取到文件对象)
 *
 * @param layerIconlib
 * @return
 */
@ResponseBody
@RequestMapping(method = RequestMethod.POST, value = "iconlib/save")
public Object saveIconLib(LayerIconlib layerIconlib, @RequestParam(required = false, value = "file") MultipartFile file) {
```
 
 
- 异步表单提交
```
var options = {
    url: 'async_submit_test1.aspx?action=SaveUserInfo',
    type: 'post',
    dataType: 'text',
    data: $("#form1").serialize(),
    success: function (data) {
    if (data.length > 0)
        $("#responseText").text(data);
    }
};
$.ajax(options);
```
 
 
- jQuery.form插件轻松实现表单提交 `[ajaxForm or  ajaxSubmit ]`
```
var options = {success: function (data) {$("#responseText").text(data);} };
 
 
// ajaxForm
$("#form1").ajaxForm(options);
 
 
// ajaxSubmit
$("#btnAjaxSubmit").click(function () {
    $("#form1").ajaxSubmit(options);
});
```
 
 
`使用jQuery.form插件，实现完美的表单异步提交 - 滴答的雨 - 博客园`
http://www.cnblogs.com/heyuquan/p/form-plug-async-submit.html
 
---
## 方案二： ajax上传 ` FileReader( Base64)` （ 异步）
`[new FileReader().readAsDataURL(file)]`
 
 
 
---
## 方案三：ajax上传 `FormData` （ 异步）
- [ajax]获取elment（file）值
```
$.ajax({
type: "POST",
url: "handler/UploadImageHandler.ashx",
data: { imgPath: $("#uploadFile").val() },
success: function(data) { },
});
 
```
`Jquery实现异步上传图片 - jack_Meng - 博客园`
http://www.cnblogs.com/mq0036/p/3715024.html
 
 
 
- [ajax] 序列化表单 & 进度显示
```
// 序列化表单 
var serializeData = $(this).serialize();
 
 
uploadProgress: function (event, position, total, percentComplete){
   //在这里控制进度条 
},
```
`使用Ajax异步上传图片的方法(html,javascript,php) - Helkyle的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/JOUeu/article/details/46858923
 
 
 
- [ajax] formData上传
```
var data = new FormData($('#form1')[0]);
$.ajax({
   url: 'server.php',
   type: 'POST',
   data: data,
   dataType: 'JSON',
```
`使用FormData对象提交表单及上传图片 - 傲雪星枫 - 博客频道 - CSDN.NET`
http://blog.csdn.net/fdipzone/article/details/38910553
 
 
- [ajax] formData将图片转base64上传
 
```
fromData = new FormData();
fromData.append("base64", e.target.result);
```
`JS上传图片-通过FileReader获取图片的base64 - simonbaker - 博客园`
http://www.cnblogs.com/simonbaker/p/4667849.html
 
 
-  [ajax] formData自定义(二进制)上传
```
var fd = new FormData();
var blob = dataURItoBlob(dataURL);
fd.append('file', blob);
```
base64 转 二进制文件
```
/**
* dataURL to blob, ref to https://gist.github.com/fupslot/5015897
* @param dataURI
* @returns {Blob}
*/
function dataURItoBlob(dataURI) {
    var byteString = atob(dataURI.split(',')[1]);
    var mimeString = dataURI.split(',')[0].split(':')[1].split(';')[0];
    var ab = new ArrayBuffer(byteString.length);
    var ia = new Uint8Array(ab);
    for (var i = 0; i < byteString.length; i++) {
        ia[i] = byteString.charCodeAt(i);
    }
    return new Blob([ab], {type: mimeString});
}
```
`利用`FileReader`和`FormData`实现图片预览和上传 - 推酷`
http://www.tuicool.com/articles/yYR7NnB
 
---
## 方案四 `iframe` (解决IE9及以下浏览器的异步图片上传支持)
```
这个jQuery插件实现了一个<iframe>传输，以便$ .ajax（）调用支持使用标准HTML文件输入字段上传文件。 这是通过将交换从XMLHttpRequest切换到包含提交的表单的隐藏iframe元素来完成的。
 
```
 
 
`cmlenz/jquery-iframe-transport: jQuery Ajax transport plugin that supports file uploads through a hidden iframe`
https://github.com/cmlenz/jquery-iframe-transport
 
---
## 总结
上传base64字符串其实不是一个很好的方案，base64的体积很大。
 
上传前的图片压缩逻辑之一，就是在前端把base64转成二级制数据，这个数据体积相比base64小很多，还可以塞到formdata中提交
 
`JavaScript 图片压缩问题？ - 知乎`
 
https://www.zhihu.com/question/30692677
 
 
 
---
# 多文件上传
- `multiple`
```
<input id="fileImage" type="file" size="30" name="fileselect[]" multiple />
```
`基于HTML5的可预览多图片Ajax上传 « 张鑫旭-鑫空间-鑫生活`
http://www.zhangxinxu.com/wordpress/2011/09/%E5%9F%BA%E4%BA%8Ehtml5%E7%9A%84%E5%8F%AF%E9%A2%84%E8%A7%88%E5%A4%9A%E5%9B%BE%E7%89%87ajax%E4%B8%8A%E4%BC%A0/
 
---
# 附件上传
同：图片上传
 
---
**参考**
`通过AngularJS实现图片上传及缩略图展示`
http://www.cnblogs.com/jach/p/5734920.html
 
`AngularJS图片上传功能的实现 - 推酷`
http://www.tuicool.com/articles/jQrQnmf
 
`三种上传文件不刷新页面的方法讨论：iframe/FormData/FileReader - 推酷`
http://www.tuicool.com/articles/UjYzArb
 
`详解：怎样实现前端裁剪上传图片功能 - 知乎专栏`
https://zhuanlan.zhihu.com/p/23340867
 
`通过AngularJS实现图片上传及缩略图展示 - 程序大大 - 博客园`
http://www.cnblogs.com/jach/p/5734920.html


```
<input type="file" id="one-input" accept="image/*" file-model="images" onchange="angular.element(this).scope().img_upload(this.files)"/>
```


`DataURL与File,Blob,canvas对象之间的互相转换的Javascript - 无心的专栏 - 博客频道 - CSDN.NET `
http://blog.csdn.net/cuixiping/article/details/45932793


`理解DOMString、Document、FormData、Blob、File、ArrayBuffer数据类型 « 张鑫旭-鑫空间-鑫生活`
http://www.zhangxinxu.com/wordpress/2013/10/understand-domstring-document-formdata-blob-file-arraybuffer/
