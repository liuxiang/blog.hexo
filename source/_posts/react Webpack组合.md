title: react Webpack组合
date: 2016-12-20 00:00:00
tags: [webpack]


---
# Webpack
`介紹 | Webpack 中文指南`
http://webpackdoc.com/
`使用示例`  http://webpackdoc.com/usage.html


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