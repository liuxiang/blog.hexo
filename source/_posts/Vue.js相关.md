title: Vue.js相关
date: 2016-12-21 00:00:00
tags: [Vue.js]


---
# 简介

- 前端 MVVM(Model-View-ViewModel)库
-  部分功能和Jquery是差不多的，Vuejs能做的，Jquery也能做
-  Jquery多简单啊，令人发指的是Vuejs在实现相同功能的时候更简单
http://larabase.com/collection/2/post/108


# 脚手架
- [ FountainJS/generator-fountain-vue]( https://github.com/fountainjs/generator-fountain-vue#readme ) `star 24`
- [ davidhellmann/generator-dhBoilerplate]( https://github.com/davidhellmann/generator-dhBoilerplate#readme)     `star 18` 
- [更多]( http://www.yowebapp.com/generators/ )


# 使用
- 示例
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
 
<script src="./lib/vue.js"></script>
 
<div id="app">
  {{ message }}
</div>
 
</body>
</html>
 
<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue!'
    }
  })
</script>
```


- API
http://devdocs.io/vue~2/


# 组件库
[Element]( http://element.eleme.io/#/zh-CN )
Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的组件库，提供了配套设计资源，帮助你的网站快速成型。



`表格` http://element.eleme.io/#/zh-CN/component/table
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/80850400-file_1484818139040_1757d.png)  


# 示例


- [`renren-security`是一个轻量级权限管理系统]( http://git.oschina.net/babaio/renren-security )
![]( http://git.oschina.net/uploads/images/2016/1223/111503_3df60329_63154.png )


- [vue-bulma/vue-admin]( https://github.com/vue-bulma/vue-admin)  `star 2,188`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/55408542-file_1484818151580_8c3c.png)  


- [taylorchen709/vueAdmin](https://github.com/taylorchen709/vueAdmin)  `star 184`

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/40671054-file_1484818165497_12ade.png)
 
- [misterGF/CoPilot]( https://github.com/misterGF/CoPilot)   `star 470`
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-1-19/11652390-file_1484818178174_26d1.png)


# 浏览器devtools工具
- [vue-devtools]( https://github.com/vuejs/vue-devtools)
![](https://raw.githubusercontent.com/vuejs/vue-devtools/master/media/screenshot.png)



# design(设计)
- [vue-material]( https://github.com/marcosmoura/vue-material)
Material design for Vue.js https://vuematerial.github.io
![](https://vuematerial.github.io/assets/vue-material-example.png)