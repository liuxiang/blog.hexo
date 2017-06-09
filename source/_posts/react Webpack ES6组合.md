title: react Webpack ES6组合
date: 2016-12-20 00:00:00
tags: [webpack]


---
# Webpack
`介紹 | Webpack 中文指南`
http://webpackdoc.com/


## 简介
Webpack 是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。



## 示例
`使用示例` http://webpackdoc.com/usage.html

-  index.html
```
<!-- index.html -->
<html>
<head>
  <meta charset="utf-8">
</head>
<body>
  <script src="bundle.js"></script>
</body>
</html>
```
-  entry.js
```
document.write('It works.')

```
- 编译 entry.js 并打包到 bundle.js：
```
$ webpack entry.js bundle.js

```
> 用浏览器打开 index.html 将会看到 It works.



- 接下来添加一个模块 module.js 并修改入口 entry.js

- module.js
```
module.exports = 'It works from module.js.'
```
- entry.js
```
document.write('It works.')
document.write(require('./module.js')) // 添加模块
```
> 重新打包 `webpack entry.js bundle.js`后刷新页面看到变化 It works.It works from module.js.


---
## webpack简单点来说就就是一个配置文件，所有的魔力都是在这一个文件中发生的。 这个配置文件主要分为三大块

- entry 入口文件 让webpack用哪个文件作为项目的入口
- output 出口 让webpack把处理完成的文件放在哪里
- module 模块 要用什么不同的模块来处理各种类型的文件


`Webpack傻瓜式指南（一） - 前端外刊评论 - 知乎专栏`
https://zhuanlan.zhihu.com/p/20367175?columnSlug=FrontendMagazine


`Webpack傻瓜指南（二）开发和部署技巧 - 前端外刊评论 - 知乎专栏`
https://zhuanlan.zhihu.com/p/20397902


---


`React+Webpack快速上手指南 - 简书`
http://www.jianshu.com/p/418e48e0cef1


`Webpack 入门指迷 - 题叶, JiyinYiyong - SegmentFault`
https://segmentfault.com/a/1190000002551952


`轻松入门React和Webpack - 说学逗唱 - SegmentFault`
https://segmentfault.com/a/1190000002767365

```
执行打包
如果通过npm install -g webpack方式安装webpack的话，可以通过命令行直接执行打包命令，比如这样：
 
$webpack --config webpack.config.js
这样就会读取当前目录下的webpack.config.js作为配置文件执行打包操作
 
如果是通过gulp插件gulp-webpack，则可以在gulpfile中写上gulp任务：
 
var gulp = require('gulp');
var webpack = require('gulp-webpack');
var webpackConfig = require('./webpack.config');
gulp.task("webpack", function() {
    return gulp
        .src('./')
        .pipe(webpack(webpackConfig))
        .pipe(gulp.dest('./build'));
});
```


---


# `generator-react-webpack`项目运行过程
```
npm start
```
- `package.json` - `scripts` - `start`
```
"scripts": {
    "start": "node server.js --env=dev",
    "test": "karma start",
    "serve": "node server.js --env=dev",
    "serve:dist": "node server.js --env=dist",
    "dist": "npm run copy & webpack --env=dist",
    "copy": "copyfiles -f ./src/index.html ./src/favicon.ico ./dist",
  },
```
- `server.js --env=dev`
```
require('core-js/fn/object/assign');
const webpack = require('webpack');
const WebpackDevServer = require('webpack-dev-server');
const config = require('./webpack.config');
const open = require('open');
 
const compiler = webpack(config); // 实例化webpack
new WebpackDevServer(compiler, config.devServer) // 启动服务
```
- `webpack.config.js`
```
const args = require('minimist')(process.argv.slice(2)); //  获取输入`dev`
let config = require(path.join(__dirname, 'cfg/' + validEnv));// 获取cfg模块 `cfg/dev`
 
module.exports = buildConfig(env);// 导出配置
```
- `cfg/dev.js`
```
let baseConfig = require('./base');
let defaultSettings = require('./defaults');



// 配置初始化(继承`baseConfig`)
let config = Object.assign({}, baseConfig, {
  entry: [  // 项目 入口
    'webpack-dev-server/client?http://127.0.0.1:' + defaultSettings.port,
    'webpack/hot/only-dev-server',
    './src/index'
  ],
  cache: true,
  devtool: 'eval-source-map',
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin(),
    new BowerWebpackPlugin({
      searchResolveModulesDirectories: false
    })
  ],
  module: defaultSettings.getDefaultModules() // 项目module 模块
});


// Add needed loaders to the defaults here
config.module.loaders.push({
  test: /\.(js|jsx)$/,
  loader: 'react-hot!babel-loader', // hot热加载 &  Babel能够将ES6代码转换成ES5
  include: [].concat(
    config.additionalPaths,
    [ path.join(__dirname, '/../src') ]
  )
});


module.exports = config; // 导出配置
```


- `cfg/ defaults.js `  项目module 模块
```
module.exports = {
  srcPath: srcPath,
  publicPath: '/assets/',
  port: dfltPort,
  getDefaultModules: getDefaultModules // function
};
```
 
- `cfg/base.js`
```
module.exports = {
  additionalPaths: additionalPaths,
  port: defaultSettings.port,
  debug: true, // 是否启用debug
  devtool: 'eval',
  output: { // 项目出口
    path: path.join(__dirname, '/../dist/assets'),
    filename: 'app.js',
    publicPath: defaultSettings.publicPath
  },
  devServer: {
    contentBase: './src/',
    historyApiFallback: true,
    hot: true,// 是否支持热加载
    port: defaultSettings.port,
    publicPath: defaultSettings.publicPath,
    noInfo: false
  },
  resolve: {
    extensions: ['', '.js', '.jsx'],
    alias: {
      actions: `${defaultSettings.srcPath}/actions/`,
      components: `${defaultSettings.srcPath}/components/`,
      sources: `${defaultSettings.srcPath}/sources/`,
      stores: `${defaultSettings.srcPath}/stores/`,
      styles: `${defaultSettings.srcPath}/styles/`,
      config: `${defaultSettings.srcPath}/config/` + process.env.REACT_WEBPACK_ENV,
      'react/lib/ReactMount': 'react-dom/lib/ReactMount'
    }
  },
  module: {}
};
```


---
#  Gulp和webpack的区别
```
背景： 最近收到很多童鞋的问题：gulp和webpack 什么关系，是一种东西吗？可以只用gulp，不用webpack吗 或者反过来？


基于此问： 我简单归结了一下区别和概念，让需要的同学阅读理解，从而不把时间浪费到这种模糊不清的选择问题上！


---
gulp是工具链、构建工具，可以配合各种插件做js压缩，css压缩，less编译 替代手工实现自动化工作
1.构建工具
2.自动化
3.提高效率用
webpack是文件打包工具，可以把项目的各种js文、css文件等打包合并成一个或多个文件，主要用于模块化方案，预编译模块的方案
1.打包工具
2.模块化识别
3.编译模块代码方案用
所以定义和用法上来说 都不是一种东西，无可比性 ，更不冲突！【当然，也有相似的功能，比如合并，区分，但各有各的优势】
``` 
```
基于此问： 来自知乎的一篇回答！够白话文了：
怎么解释呢？因为 Gulp 和 browserify / webpack 不是一回事
Gulp应该和Grunt比较，他们的区别我就不说了，说说用处吧。Gulp / Grunt 是一种工具，能够优化前端工作流程。比如自动刷新页面、combo、压缩css、js、编译less等等。简单来说，就是使用Gulp/Grunt，然后配置你需要的插件，就可以把以前需要手工做的事情让它帮你做了。
说到 browserify / webpack ，那还要说到 seajs / requirejs 。这四个都是JS模块化的方案。其中seajs / require 是一种类型，browserify / webpack 是另一种类型。


seajs / require : 是一种在线"编译" 模块的方案，相当于在页面上加载一个 CMD/AMD 解释器。这样浏览器就认识了 define、exports、module 这些东西。也就实现了模块化。


browserify / webpack : 是一个预编译模块的方案，相比于上面 ，这个方案更加智能。没用过browserify，这里以webpack为例。首先，它是预编译的，不需要在浏览器中加载解释器。另外，你在本地直接写JS，不管是 AMD / CMD / ES6 风格的模块化，它都能认识，并且编译成浏览器认识的JS。这样就知道，Gulp是一个工具，而webpack等等是模块化方案。Gulp也可以配置seajs、requirejs甚至webpack的插件。
```
`Gulp和webpack的区别，是一种工具吗？`

http://blog.csdn.net/xllily_11/article/details/51782005



---
# gulp-webpack-demo
`gulp+webpack构建配置 - mask_天俊 - 博客园`
http://www.cnblogs.com/maskmtj/archive/2016/07/21/5597307.html
项目github地址： https://github.com/jokermask/gulp_webpack_demo
```
基本的需求：
css：要能编译sass，要压缩，要能自己补前缀

js：要压缩，要丑化（就是把变量名换成简单的字母，即省空间又不容易被看懂），要合并

img： 要压缩

html：可以引入文件（就解决一开始说的问题，多页面下共同的组件）

gulp中的watch功能可以监听文件，通过livereload和gulp的监听来实现保存后自动构建并刷新页面十分带感

```
```
感觉gulp这些功能都蛮不错，但我其实还有一些需求没说，是关于js的
1.模块化（避免命名冲突）
2.要用es6（跟上潮流嘛）
3.js能不能按需加载？比如说我有页面A和页面B，他们都依赖一个base.js，然后又分别各自依赖a.js和b.js，这种情况我用刚才的gulpfile打包以后合并出来的一个公共的js是将三个js都包括了，但显然页面B不需要a.js，所以当用户单独访问页面B的时候加载的js就显得多余了，就多费了流量啊，速度也会慢。


上述的前两个gulp还是能解决，但是第三个问题好像并不好解决。。。在查找各方资料后我决定用webpack，所以就是其他css，html的部分还是gulp来处理，但是把js交给webpack来处理
```


---
`gulp + webpack 构建多页面前端项目`

https://github.com/fwon/blog/issues/17
github: https://github.com/fwon/gulp-webpack-demo


---
# ES6 —— gulp+Babel

- `package.json`中的devDependencies
```
"devDependencies": {
    "babel-preset-es2015": "^6.5.0",
    "gulp-load-plugins": "^1.1.0",
    "gulp-babel": "^6.1.2",
    "gulp-plumber": "^1.0.1",
    "gulp-rename": "^1.2.2",
    "gulp": "^3.9.1",
    "gulp-jshint": "^2.0.0",
    "gulp-concat": "^2.6.0",
    "gulp-uglify": "^1.4.1",
    "gulp-util": "^3.0.1"
  }
```
- gulpfile.js
```
var gulp = require('gulp'),
  concat = require('gulp-concat'),
  rename = require('gulp-rename'),
  jshint = require('gulp-jshint'),
  uglify = require('gulp-uglify');




// Load plugins
var $ = require('gulp-load-plugins')();


/* es6 */
gulp.task('es6', function() {
  
  return gulp.src('src/es6js/*.js')
    .pipe($.plumber())
    .pipe($.babel({
      presets: ['es2015']
    }))
    .pipe(gulp.dest('dist/ztimages/'));
});
```
我们有了任务，但是怎么执行任务呢？ 在命令行工程目录下输入gulp es6 就可以执行这个任务了。但是我们不能每写一行代码就手动“执行”一下。这样也太麻烦了。我们可以配置一个监听任务。
```
//监听文件修改
gulp.task('watch', ['es6'], function() {
  gulp.watch(['src/js/earth.js'], ['es6']);
});
```
> 在命令行工程目录下输入gulp watch就可以执行这个监听任务了。


`ES6 初体验 —— gulp+Babel 搭建ES6环境`
http://www.cnblogs.com/webARM/p/5305644.html


---
#  ES6 —— webpack+Babel
`webpack配置文件ES6写法？ - xxxxxxxxx的回答 - SegmentFault`

https://segmentfault.com/q/1010000004241222/a-1020000004241336


`使用webpack 进行ES6开发 - OPEN 开发经验库`
http://www.open-open.com/lib/view/open1455887871526.html


---
# 常见组合方式
- 仅使用gulp,解决代码压缩等预编译构建问题。但缺少预编译的模块化能力（webpack）
- 仅使用webpack,提供js模块化方案。但缺少代码压缩等预编译能力(gulp)
- gulp + webpack: https://github.com/fwon/gulp-webpack-demo
- npm script + webpack:  https://github.com/react-webpack-generators/generator-react-webpack


---
# 附：ES6（babel）常见组合
## 一、在线"编译" 模块的方案（ 浏览器环境 ）
1.[babel-polyfill](http://www.ruanyifeng.com/blog/2016/01/babel.html) `六、babel-polyfill`
> 举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片


2.[babel-core/browser](http://www.ruanyifeng.com/blog/2016/01/babel.html) ` 七、浏览器环境`
> browser.js是Babel提供的转换器脚本，可以在浏览器运行。用户的ES6脚本放在script标签之中，但是要注明type="text/babel"


3.[babel-standalone](https://github.com/babel/babel-standalone)
> 给那些非Node.js环境中使用最新的JavaScript而服务的。
注意，网页中实时将ES6代码转为ES5，对性能会有影响。生产环境需要加载已经转码完成的脚本。


`详见：`ES6兼容低版本浏览器.md`



## 二、预编译模块的方案
- Babel-cli
- npm script + Babel
-  gulp + Babel
-  webpack + Babel

