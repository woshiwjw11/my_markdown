# vue 项目开发 整体流程

[TOC]

## 项目目录

![webpack配置相关](./vue/webpack_1.png)

> 这两个文件夹主要是webpack的配置文件相关内容



![node安装的模块依赖](./vue/yilai.png)

> 通过node安装的项目依赖

![源码](./vue/src.png)

> 存放开发的源码

![静态资源](./vue/st.png)

> 存放第三方静态资源文件，现在其中的文件是，在改文件夹为空时，依旧可以提交到github上面。

![babel](./vue/babel.png)

> 配置将es6文件，转化为es5

## 项目运行的过程

* 小知识点，在使用组件之前，一定要在script代码中注册：

```javascript
 components: {
    Hello
  }
```

* webpack是如何进行打包

  * webpack.base.conf.js文件详述

  ```javascript
    entry: {
      app: './src/main.js'//入口文件
    },
    output: {
      path: config.build.assetsRoot,
      publicPath: process.env.NODE_ENV === 'production' ? config.build.assetsPublicPath : config.dev.assetsPublicPath,
      filename: '[name].js'
    },
    resolve: {
      extensions: ['', '.js', '.vue'],//自动获取后缀名
      fallback: [path.join(__dirname, '../node_modules')],
      alias: {
        'src': path.resolve(__dirname, '../src'),
        'assets': path.resolve(__dirname, '../src/assets'),
        'components': path.resolve(__dirname, '../src/components')
      }//引入文件相关配置
  ```

> 配置基本文件，入口出口，和基本处理方法

* webpack.dev.conf.js

```javascript
// add hot-reload related code to entry chunks
Object.keys(baseWebpackConfig.entry).forEach(function (name) {
  baseWebpackConfig.entry[name] = ['./build/dev-client'].concat(baseWebpackConfig.entry[name])
})//启动hot-reload的方法
```

![配置](./vue/webpack2.png)

