---
title: reactNative使用全解
date: 2017-07-21 11:13:41
tags: reactNative
categories: reactNative
---

# 缘由：使用reactNative开发跨端项目（iOS,android,wechat）

<!--more-->

# 快速概览
主要使用的技术：
react es6 dva(redux,redux-sagas),ant-design reactNative reactNative-Navigation


# 一、环境搭建
默认安装：node npm react-native-cli

## 1-1、初始项目搭建
```
【创建一个reactNative项目】react-native init reactPhoto --version 0.44.3
cd reactPhoto
【创建导航插件(官方推荐)】npm install --save react-navigation【目前使用1.0.0-beta.11】
```

## 1-2、初步整理项目，运行项目
为了共用一套代码，修改index.android.js和index.ios.js内容为:
```
import './app.js';
```

新建文件app.js在里面写逻辑
**【注意：】**
```
AppRegistry.registerComponent('reactPhoto', () => reactPhoto);
```
上述代码中的注册名字必须和开始生成项目的名字一致【reactPhoto】.

## 1-3、参考资料
[react-navigation代码库](https://github.com/react-community/react-navigation)
[reactNative中文入门文档](https://reactnative.cn/docs/0.46/getting-started.html)


# 二、配置项目结构和第三方插件
## 2-1、集成dva框架
在这里推荐以下配置：
```js
{
  "name": "DvaStarter",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start",
    "test": "jest",
    "prettier": "prettier --write --single-quote --no-semi --trailing-comma es5 --print-width 80 \"app/**/*.js\"",
    "lint": "eslint app",
    "format": "yarn prettier && yarn lint -- --fix",
    "precommit": "yarn format"
  },
  "dependencies": {
    "dva": "1.3.0-beta.4",
    "react": "16.0.0-alpha.6",
    "react-native": "^0.44.3",
    "react-navigation": "^1.0.0-beta.11",
    "redux-persist": "^4.8.0"
  },
  "devDependencies": {
    "babel-eslint": "^7.2.3",
    "babel-jest": "^20.0.3",
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-preset-react-native": "^1.9.2",
    "eslint": "^3.19.0",
    "eslint-config-airbnb": "^15.0.1",
    "eslint-config-prettier": "^2.1.1",
    "eslint-plugin-babel": "^4.1.1",
    "eslint-plugin-import": "^2.3.0",
    "eslint-plugin-jsx-a11y": "^5.0.3",
    "eslint-plugin-prettier": "^2.1.1",
    "eslint-plugin-react": "^7.0.1",
    "husky": "^0.13.4",
    "jest": "^20.0.4",
    "prettier": "^1.4.4",
    "react-test-renderer": "^16.0.0-alpha.6"
  },
  "jest": {
    "preset": "react-native"
  }
}
```

## 2-2、参考资料
[dva-reactNative官方例子](https://github.com/nihgwu/react-native-dva-starter)






# 三、在iOS真机上运行
## 3-1、修改General目录左侧的targets下的reactPhoto的signing
## 3-2、修改General目录左侧的targets下的reactPhotoTests的signing
