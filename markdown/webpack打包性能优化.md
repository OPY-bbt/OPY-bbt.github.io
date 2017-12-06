# 基于webpack 3 打包性能优化

[source](https://www.codementor.io/drewpowers/high-performance-webpack-config-for-front-end-delivery-90sqic1qa#comments-90sqic1qa)

## Scope Hoisting.
---
过去 webpack 打包时的一个取舍是将 bundle 中各个模块单独打包成闭包。这些打包函数使你的 JavaScript 在浏览器中处理的更慢。相比之下，一些工具像 Closure Compiler 和 RollupJS 可以提升(hoist)或者预编译所有模块到一个闭包中，提升你的代码在浏览器中的执行速度。

打开方法: 
```javascript
module.exports = {
    plugins: [
        new webpack.optimize.ModuleConcatenationPlugin(),
    ],
};
```
--display-optimization-bailout 将显示绑定失败的原因。

## Minification and Uglification.
---

这个应该都会用吧. 去掉空格，换行，注释及一些不必要在生产环境中的东东。
```javascript
    webpack -p // -p is short for "production"
```
如果没有给node设置production环境变量的话，还需要设置
```javascript
    NODE_ENV=production PLATFORM=web webpack -p
```
还有一些第三方库在生产环境中会去掉一些在开发或测试环境中才能用到的功能，使其提升性能和减少体积.     
一般来说 ***-p*** 已经够用了， 如果还想再进一步减少体积的话可以使用 ***webpack.optimize.UglifyJsPlugin***.

## Dynamic Imports for Lazy-loaded Modules.
--- 

* step 1: Babel setup  
`yarn add babel-loader babel-core babel-preset-env`
```javascript
// update your webpack.config.js
module: {
    rules: [
        {
        test: /\.js$/,
        use: 'babel-loader',
        exclude: /node_modules/,
        },
    ],
},
```
* step 2: update babelrc       
`yarn add babel-plugin-syntax-dynamic-import`
```javascript
// update .babelrc
{
    "presets": ["env"],
    "plugins": ["syntax-dynamic-import", "transform-react-jsx"]
}
```
如果你是使用 **es2015** 作为preset，那么建议你换到 **env**.
* step 3: use import()          
```javascript
//old
import Home from './components/Home';

//now
const Home = import('./components/Home');
// 原来的require.ensure 已经被import()取代.
```
目前vue 已经可以做到开箱即用，但是react还不可以。我们可以使用 [react-code-split](https://github.com/didierfranc/react-code-splitting) 这个库，实现原理就是使用import().
```javascript
import React from 'react';
import Async from 'react-code-splitting';

const Nav = () => (<Async load={import('./components/Nav')} />);
const Home = () => (<Async load={import('./views/home')} />);
const Countdown = () => (<Async load={import('./views/countdown')} />);
```

##  Deterministic Hashes for Caching
---

增加hash方便缓存。下面这种增加hash的方式，只有当文件改变了hash值才会变。这种就避免了用户不必要的下载。      
- <font color="#FF1493">yarn add chunk-manifest-webpack-plugin webpack-chunk-hash</font>
- update config.js
```js
const webpack = require('webpack');
const ChunkManifestPlugin = require('chunk-manifest-webpack-plugin');
const WebpackChunkHash = require('webpack-chunk-hash');
const HtmlWebpackPlugin = require('html-webpack-plugin');

/* Shared Dev & Production */

const config = {
/* … our webpack config up until now */

plugins: [
    // /* other plugins here */
    // 
    // /* Uncomment to enable automatic HTML generation */
    // new HtmlWebpackPlugin({
    //   inlineManifestWebpackName: 'webpackManifest',
    //   template: require('html-webpack-template'),
    // }),
],
};

/* Production */

if (process.env.NODE_ENV === 'production') {
config.output.filename = '[name].[chunkhash].js';
config.plugins = [
    ...config.plugins, // ES6 array destructuring, available in Node 5+
    new webpack.HashedModuleIdsPlugin(),
    new WebpackChunkHash(),
    new ChunkManifestPlugin({
    filename: 'chunk-manifest.json',
    manifestVariable: 'webpackManifest',
    inlineManifest: true,
    }),
];
}

module.exports = config;
````

## CommonsChunkPlugin for Vendor Caching
---

将第三依赖打包到vendor.js里，这样你升级时，这些第三方依赖就不会重新下载.使用 **CommonsChunkPlugin**
```javascript
const webpack = require('webpack');
entry: {
    app: './app.js',
    vendor: ['react', 'react-dom', 'react-router'],
},

// 注意这里的name要和entry相同。否则这些第三方依赖会打包到所有的入口文件
plugins: [
  new webpack.optimize.CommonsChunkPlugin({
    name: 'vendor',
  }),
],
```

Tips:       
- 全局使用的modules
- 更新频率很低的modules
- 频繁使用的子modules Only load commonly-used submodules. For example, if the app uses 'rxjs/Observable' frequently, but 'rxjs/Scheduler' rarely, then only load the former. And whatever you do, don’t load all of 'rxjs'!

## Offline Plugin for webpack
---

一个离线可访问的webapp. 插件 [OfflinePlugin](https://github.com/NekR/offline-plugin) .使用也很简单，自己去看吧。`自己试了一下，ios10 微信内浏览器不行，Safari 可以离线访问`

## webpack Bundle Analyzer
---

分析bundle.js的工具🔧 
`yarn add --dev webpack-bundle-analyzer`
```javascript
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

config = { /* shared webpack config */ };

if (process.env.NODE_ENV !== 'production' && process.env.NODE_ENV !== 'test') {
  config.plugins = [
    ...config.plugins,
    new BundleAnalyzerPlugin(),
  ];
}

how to use:
node_module/.bin/webpack --profile --json > stats.json
```
### 原本提到了关于优化moment打包大小的问题，[solution](https://github.com/webpack/webpack/issues/3128#issuecomment-311418452), 这里提一下如果你使用 [roadhog](https://github.com/sorrycc/roadhog) 脚手架的话，在 `.roadhogrc.js`中加上 `"ignoreMomentLocale": true` 就可以实现(隐藏func).



