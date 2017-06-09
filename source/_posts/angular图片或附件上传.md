title: angular图片或附件上传
date: 2017-4-8 00:00:00
tags: [ 上传 ]


---


# 工具
`AngularJS 文件上传控件 ng-file-upload - 岳小昊 - 博客园`
http://www.cnblogs.com/yuexiaohao/p/5556055.html


- `ng-file-upload`  https://github.com/danialfarid/ng-file-upload
- `angular-file-upload`  https://github.com/nervgh/angular-file-upload


`[Angularjs]ng-file-upload上传文件 - 推酷`
http://www.tuicool.com/articles/eeAb6r6


---
# angular $http
`AngularJS下$http上传文件(AngularJS file upload/post file) - Jannie_Kung的博客 - 博客频道 - CSDN.NET`
http://blog.csdn.net/jannie_kung/article/details/52057288



`angular 采用$http 上传file spring 提示找不到boundary`

http://blog.csdn.net/wendy260310/article/details/49493461


- 遗留问题
```
The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).



服务器由于被认为是客户端错误（例如，格式错误的请求语法，无效请求消息成帧或欺骗性请求路由）的某些东西而无法或不会处理该请求。

```


![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/99127824.jpg)


 - 后台异常：`“iconSrc”所需的类型[java.lang.String]：找不到匹配的编辑者或转换策略`
```
Field error in object 'layerIconlib' on field 'iconSrc': rejected value [org.springframework.web.multipart.commons.CommonsMultipartFile@78cfdcd6]; codes [typeMismatch.layerIconlib.iconSrc,typeMismatch.iconSrc,typeMismatch.java.lang.String,typeMismatch]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [layerIconlib.iconSrc,iconSrc]; arguments []; default message [iconSrc]]; default message [Failed to convert property value of type [org.springframework.web.multipart.commons.CommonsMultipartFile] to required type [java.lang.String] for property 'iconSrc'; nested exception is java.lang.IllegalStateException: Cannot convert value of type [org.springframework.web.multipart.commons.CommonsMultipartFile] to required type [java.lang.String] for property 'iconSrc': no matching editors or conversion strategy found]
at org.springframework.web.method.annotation.ModelAttributeMethodProcessor.resolveArgument(ModelAttributeMethodProcessor.java:113)
at org.springframework.web.method.support.HandlerMethodArgumentResolverComposite.resolveArgument(HandlerMethodArgumentResolverComposite.java:78)
at org.springframework.web.method.support.InvocableHandlerMethod.getMethodArgumentValues(InvocableHandlerMethod.java:162)
at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:129)
at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:110)
at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:775)
at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:705)
at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85)
at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:959)
at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:893)
at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:965)
at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:867)
```
- 问题原因：`File`文件（图片）对象不能使Model中的file_url值存储，因为数据类型不一致，在spring的自动装载时，会出现此错误
- 解决办法：换个名字载入对象



---
# 完整示例
## 方案一：` $ http`轻量支持
- 业务调用

```
// 上传文件
$httpFactory.layer.iconlib_save(iconlib.id, iconlib.classId, iconlib.iconSrc, iconlib.icon)
  .success(function (response) {
    $log.debug(response);
    toastr.success('操作成功');
  });
```


- 一贯统一的接口定义方式

```
iconlib_save: function (id, classId, iconSrc, file) {
  var url = domain + 'layer/iconlib/save';
  var data = {id: id, classId: classId, iconSrc: iconSrc, file: file};
  return sendForm(url, data);
},
```


- `formData`公共处理
```
function sendForm(url, data) {
  angular.forEach(data, function (value, key, data) {
    value == null && delete data[key];// 清理空字段,避免后台数据自动组装引发异常
  });


  var req = {
    url: url,
    method: 'POST',
    data: data,
    headers: {'Content-Type': undefined},
    transformRequest: function (data, headersGetter) {
      let formData = new FormData();
      angular.forEach(data, function (value, key) {
        formData.append(key, value);
      });
      return formData;
    }
  };
  // 参见：http://blog.csdn.net/jannie_kung/article/details/52057288


  return $http(req)
    .success(function (response) {
      // alert(response);
    }).error(function (err) {
      $log.error(error);
    });
}
```


## 方案二：插件支持，优点：支持进度展示
```
// 文件上传示例
false && Upload.upload({
  url: _config.domain_name + 'layer/iconlib/save',
  data: {classId: iconlib.classId},
  file: iconlib.icon
}).then(function (resp) {
  console.log('Success ' + resp.config.data.file.name + 'uploaded. Response: ' + resp.data);
}, function (resp) {
  console.log('Error status: ' + resp.status);
}, function (evt) {
  var progressPercentage = parseInt(100.0 * evt.loaded / evt.total);
  console.log('progress: ' + progressPercentage + '% ' + evt.config.data.file.name);
});
```


## 后端
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


    if (layerIconlib.getId() != null) {
        layerIconlibMapper.updateByPrimaryKey(layerIconlib);
    } else {
        layerIconlibMapper.insert(layerIconlib);
    }
    return new HashedMap() {{
        put("object", null);
        put("result", Constants.JSON_RESULT_SUCCESS);
        put("message", "success");
    }};
}
```


---
# http or ajax ($http boundary)
`HTTP上传文件的boundary - Hello World! - 博客频道 - CSDN.NET`
http://blog.csdn.net/cxb_phoenix/article/details/51063147
