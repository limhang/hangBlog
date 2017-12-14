---
title: react开发环境搭建
date: 2017-12-10 18:49:15
tags: [前端, react, package, webpack]
categories: 前端
---

## 缘由
这个是web-react的step by step教程，主要介绍搭建react开发环境的各种配置，包含各种sdk配置，第三方包配置

<!--more-->

## 一、需要理解的几个名词
### 1-1、npm
#### 1-1-1、npm是什么？
npm 可以理解成类似于pod的一个包管理工具，方便我们添加删除各种依赖包

#### 1-1-2、package.json是什么？
npm包中有一个package.json文件，这个文件是说明项目依赖的第三方库。在该文件中，有一个是devDependencies配置项，一个是dependencies配置项，我们知道的，dev是开发环境的意思，我们使用npm和webpack的目的是什么，是需要弄一个bundle文件出来，所以，项目中需要使用的包，我们是用开发环境模式，像css loader ，webpack之类的，是用于打包的，不是用于开发的，所以使用开发模式dev。

package.json，类似于xcode的`podfile`。这个文件，说明需要依赖的第三方库，一般将项目中需要使用的放在`dependencies`这个对象下，将解释编译项目需要用到的第三方放在`devDependencies`对象下，如`babel-core`,`webpack`,`webpack-dev-server`，这些就是解释编译项目需要用到的。此外，这个文件，还有脚本配置项，`scripts`对象，主要存放，运行的脚本，`npm run scriptName`，就可以运行。

### 1-2、webpack
#### 1-2-1、webpack是什么？
在初学的时候，一直有一个疑惑，npm和webpack到底什么关系，既然有了npm包管理工具了，也就是有了类似pod的工具了，为什么还需要webpack，不能像xcode一样，直接运行跑起来嘛？

其实，我们虽然有了npm包管理工具，可以很方便的下载第三方使用，但是我们现在写的很多es6语法，是不能被浏览器运行的，例如，谷歌浏览器，也只能做到支持50%的es6语法。而我们常用的import，export语法，其实是es6才支持的，所以我们需要将我们的es6代码转换成es5代码。webpack自带的最基础功能，就是转换es6语法为es5语法。可以理解为，webpack是一个特殊的强大的第三方库。

#### 1-2-2、webpack.config.js是什么？
webpack的配置文件，是叫做webpack.config.js。这个里面的配置项，比较多，每个配置项，可以看webpack官方文档，必须看外文的，看中文的，只会更浪费时间。主要的配置，module下的rule，可以添加各种文件(css,jsx,js,scss)的解析，devServer+HotModuleReplacementPlugin热更新。


## 二、配置文件，运行环境
### 2-1、package.json配置
在项目目录下，创建package.json，填写以下配置项

```
  "name": "wxminicode",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "./node_modules/.bin/webpack --config webpack.config.js",
    "start": "./node_modules/.bin/webpack-dev-server"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "css-loader": "^0.28.7",
    "node-sass": "^4.7.2",
    "sass-loader": "^6.0.6",
    "style-loader": "^0.19.0",
    "webpack": "^3.8.1",
    "webpack-dev-server": "^2.9.4"
  },
  "dependencies": {
    "eventemitter3": "^1.1.1",
    "lodash": "^4.17.4",
    "react": "^16.1.1",
    "react-dom": "^16.1.1",
    "react-transition-group": "^1.2.1"                                      
  }
}
```

其中`react-transition-group`是我组件化中依赖的库，版本为`1.2.1`
然后执行`npm install`

**warning:**由于防火长城的存在，所以一定要配置npm的镜像。

### 2-2、webpack.config.js配置
在项目目录下，创建webpack.config.js，填写以下配置项

```
const path = require('path');
const webpack = require('webpack');

module.exports = {
  entry: './src/index.js',
  devServer: {
    contentBase: './dist',
    hot: true
  },
  plugins: [
   new webpack.NamedModulesPlugin(),
   new webpack.HotModuleReplacementPlugin()
  ],
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  devtool: "eval-source-map",
  module: {
     rules: [
       {
         test: /\.css$/,
         use: [
           'style-loader',
           'css-loader'
         ]
       },
       {
          test: /\.scss$/,
          use: [{
              loader: "style-loader" // creates style nodes from JS strings
          }, {
              loader: "css-loader" // translates CSS into CommonJS
          }, {
              loader: "sass-loader" // compiles Sass to CSS
          }]
        },
       {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/, // 这里面的不解析
        loader: 'babel-loader',
        query:
          {
            presets:['es2015', 'react']
          }
       }
     ]
  },
};
```

### 2-3、安装上述配置文件，完善工程目录配置
仔细看上述的`webpack.config.js`文件，我们可以知道，入口文件是当前目录下的`./src/index.js`，然后生成的bundle.js位于`dist`中。

这里需要介绍下整个webpack工作流程，可自行参考webpack官网。

webpack首先进入entry配置下的文件，也就是`./src/index.js`。运行该文件后，可以得知项目所有依赖关系，然后生成一个bundle文件。上述的webpack.config.js中有配置devServer，其中指定dist为开始路径文件，故在dist文件中找index执行。

#### 2-3-1、入口文件配置
经过上述webpack运行原理阐述之后，我们需要创建一个`./src/index.js`入口文件，该文件就是我们es6，react写法的开端。示例如下：

```
import ReactDOM from 'react-dom';
import React from 'react';

let first = 'xiaozuo';

ReactDOM.render(
  <div>{first}</div>,
  document.getElementById('root')
);

```

#### 2-3-2、启动文件（index.html）
这里开始添加devservice启动文件：

```
<!DOCTYPE html>
<html>
<head>
	<title>xxx</title>
</head>
<body>
	<div id="root"></div>
	<script src="./bundle.js"></script>
</body>
</html>
```

#### 2-4、配置完成，检验效果
在项目根目录下，执行`npm run build`命令，生成bundle文件，然后执行`npm run start`命令，开始devservice服务，在没开全局网络代理下，浏览器输入`localhost:8080`，就能看到`xiaozuo`这个结果了。

如果一切顺利，您就搭建完成了基本的react开发环境了。如遇不顺，可参考官方文档，逐条排查，切记，混英文社区。





