title:  yeoman 之 generator-gulp-angular,介绍


date: 2017-03-23 00:00:00
tags: [yeoman ]



---



[ ** `generator-gulp-angular 首页 ` ** ]( https://github.com/Swiip/generator-gulp-angular/tree/139e2322da3ee2e4be47cbf8fd0beae047c1c41e )




[`使用`](https://github.com/Swiip/generator-gulp-angular/blob/139e2322da3ee2e4be47cbf8fd0beae047c1c41e/docs/usage.md)


- 创建你的项目 yo gulp-angular [app-name]

- 使用Gulp任务

```

gulp或gulp build构建您的应用程序的优化版本/dist

gulp serve 在您的源文件上启动浏览器同步服务器

gulp serve:dist 在您优化的应用程序上启动服务器

gulp test 用Karma启动您的单元测试

gulp test:auto 以观看模式启动您的单位测试

gulp protractor 用量角器启动e2e测试

gulp protractor:dist 使用量角器在dist文件上启动e2e测试

```

- gulpfile功能

```

useref：允许在HTML文件的注释中配置文件

ngAnnotate：将简单注入转换为完整语法以进行细化

uglify ：优化所有您的JavaScript

csso：优化所有的CSS

autoprefixer：将供应商前缀添加到CSS

rev：在文件名中添加一个哈希，以防止浏览器缓存问题

watch ：观察您的源文件并自动重新编译

eslint：可插拔的linting实用程序

imagemin：您的所有图像将在构建时进行优化

Unit test (karma) ：开箱即用的测试配置与业力

e2e test (protractor)：开箱即用e2e测试配置与量角器

browser sync：功能齐全的开发Web服务器，具有实时装载和设备同步功能

angular-templatecache：所有HTML部分将被转换为JS，以便在应用程序中捆绑

TODO lazy：不要处理尽可能没有改变的文件

```




[`用户指南`]( https://github.com/Swiip/generator-gulp-angular/blob/139e2322da3ee2e4be47cbf8fd0beae047c1c41e/docs/user-guide.md)

- Yeoman工作流程


    - serve 实时重新加载修改的服务器

    - test Karma（with gulp test）进行单元测试

    - build 优化过程

    - inject 通过发送完整的文件注入过程




[`怎么运行的`]( https://github.com/Swiip/generator-gulp-angular/blob/139e2322da3ee2e4be47cbf8fd0beae047c1c41e/docs/how-it-works.md)

- gulpfile.js

- gulp/conf.js

- gulp/inject.js

   - 注入样式

   - 注入脚本

   - Wiredep：使用线性流添加Bower依赖项的CSS和JS文件。

   - 列出和排序文件进行注入

   - 查找位置 index.html

- gulp/scripts.js

   - 使用TypeScript

   - 与ES6

- gulp/styles.js

   - Injection

   - Style stream

   - Ruby Sass

- gulp/markups.js

- gulp/watch.js

- gulp/server.js

- gulp/unit-test.js

- gulp/e2e-tests.js

- gulp/build.js




[`Gulp任务,各js作用介绍`]( https://github.com/Swiip/generator-gulp-angular/blob/139e2322da3ee2e4be47cbf8fd0beae047c1c41e/generators/app/templates/gulp/README.md)

> 因为它gulpfile.js变得巨大，因为我讨厌大文件，所以这个目录包含了所有的gulp任务。gulpfiles.js通过路径配置来通过gulp任务gulp.paths。




- build.js  包含构建任务，旨在优化所有项目并创建dist文件夹

```

部分：在一个javascript中编译html部分templateCacheHtml.js

html：大的useref，rev和uglify。

图像：使用imagemin优化图像。

字体：从bower复制字体到dist

misc：复制其他文件

clean：删除临时文件

构建：html +图像+字体+杂项

```

- e2e-tests.js  从Gulp启动e2e测试的任务。这意味着启动本地服务器，Selenium和量角器的一个实例。

- inject.js  注入任务链接项目文件index.html并写入结果.tmp/serve/index.html

```

项目CSS文件

项目JS文件

Bower css和js deps

警告将src/index.html不会被修改（这是在以前版本的情况下，仍然是在其他发电机的情况下），但注入index.html被放置在.tmp/serve。

```

- markups.js  编译标记文件（使用HTML预处理器时）。

- scripts.js  如果您有JS预处理器，请编译脚本。运行短裤 如果使用ES6，还将使用Browserify来捆绑文件。

- server.js  启动服务器进行开发或e2e测试的Gulp任务。

- styles.js  使用CSS预处理器编译样式。在指数中使用注射*

- unit-tests.js  从Gulp开始对Karma进行单元测试的任务。

- watch.js  监视任务，监视源文件以触发重新编译。




---

[`如何贡献`]( https://github.com/Swiip/generator-gulp-angular/blob/139e2322da3ee2e4be47cbf8fd0beae047c1c41e/CONTRIBUTING.md)




---

![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-6-7/8010921.jpg)

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-6-7/69221993.jpg)
   



