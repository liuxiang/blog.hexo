title: css自动兼容性前缀-prefixfree
date: 2016-08-18 00:00:00
tags: [css兼容性前缀]


---
# 解决困扰
```
熟悉前端开发的朋友，肯定经历过烦心的CSS属性浏览器兼容性调整过程，为了使得CSS属性支持所有的浏览器，例如， chrome，firefox，IE，opera，Safari等等，你不得不处理添加一大推的冗余CSS代码（带有浏览器特定前缀的CSS相关代码）。特别是CSS3中的相关属性，尤其需要处理。

```
> 经过测试，并不能解决所有前缀的兼容性问题
```
background-image:-*-linear-gradient(top, #beceeb, #34538b); 



http://www.cnblogs.com/lichuntian/p/prefixfree.html

```


# CDN
```
<script src="//cdn.bootcss.com/prefixfree/1.0.7/prefixfree.min.js"></script>


http://www.bootcdn.cn/prefixfree/
```


# 官网
```
https://github.com/LeaVerou/prefixfree

http://leaverou.github.io/prefixfree/

```
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/56087384.jpg)


---
# 编译器（gulp,Webpack）中兼容补偿 / CSS后处理器
`autoprefixer`  https://github.com/postcss/autoprefixer
```
gulp.task('autoprefixer', function () {
    var postcss      = require('gulp-postcss');
    var sourcemaps   = require('gulp-sourcemaps');
    var autoprefixer = require('autoprefixer');


    return gulp.src('./src/*.css')
        .pipe(sourcemaps.init())
        .pipe(postcss([ autoprefixer() ]))
        .pipe(sourcemaps.write('.'))
        .pipe(gulp.dest('./dest'));
});
```


---
`CSS3无前缀脚本prefixfree.js与Animatable使用介绍 - Li-Cheng - 推酷`
www.tuicool.com/articles/6zuI3e


`使用prefixfree来简化CSS开发 - 推酷`
http://www.tuicool.com/articles/22e2eyy


`CSS3无前缀脚本prefixfree.js及Animatable介绍 - 推酷`
http://www.tuicool.com/articles/yMjiQj





