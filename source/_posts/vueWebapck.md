---
title: vue webpack 从零开始搭建项目框架1
date: 2020-04-29 17:27:23
categories: 
- Vue-Webpack
tags: 
- vue
- webpack
---

> **注意** 主要实现最基础最简单的vue+webpack
--------------

## 安装前提
node10^ npm6^ vue2.6^ webpack^4

## 简写说明
> -S --> --save
> -D --> --save-dev

## vue项目构成基本结构
1、引入vue库
2、html模板插槽，用于渲染数据
3、实例化Vue对象，用于处理数据(渲染、逻辑控制、展示、隐藏等)

例子：
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title></title>
  <!-- 1、引入依赖库 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
  <!-- 2、html模板插槽 -->
  <div id="app">{{ message }}</div>
  <!-- 实例化Vue对象 -->
  script>
    // 实例化vue对象并绑定数据
    var app = new Vue({
      el: '#app',
      data: {
        message: '页面加载于 ' + new Date().toLocaleString()
      }
    })
  </script>
</body>
</html>
```

## 常用插件模块

名称  | 描述
----  |----  
npm  | 包管理工具，安装node的时候自动安装
vue | vue模块
vue-loader  | vue文件加载器
vue-template-compiler | 该模块可用于将 Vue 2.0 模板预编译为渲染函数（template => ast => render），以避免运行时编译开销和 CSP 限制。大都数场景下，与 vue-loader一起使用，只有在编写具有非常特定需求的构建工具时，才需要单独使用它
webpack  | 模块打包工具
webpack-cli | 针对webpack官方提供的CLI工具
webpack-dev-server | 提供了一个简单的web服务器和使用实时重新加载的能力
html-webpack-plugin | 创建html入口文件，引入相关外部资源
extract-text-webpack-plugin@next |提取css样式,webpack4不再支持extract-text-webpack-plugin
mini-css-extract-plugin | [webpack4]将CSS提取到单独的文件中
copy-webpack-plugin | 将static插件复制到dist
optimize-css-assets-webpack-plugin | 优化/压缩css资源
clean-webpack-plugin | 自动清除生成的文件夹
style-loader | 通过注入style标记将CSS添加到DOM中
css-loader | 样式加载、解析css文件
file-loader | 指示webpack将所需的对象作为文件发出并返回其公共URL
url-loader | 将图片文件转换为base64编码，减少http请求数
less-loader | less文件加载。解析成css文件
postcss-loader | 给不同浏览器的样式加上前缀
autoprefixer | 自动补充前缀配合postcss-loader使用
postcss-url | 转换url，复制资产的PostCSS插件
script-loader |在全局上下文中执行一次javascript文件，不需要解析
babel-loader | 用来处理ES6语法，将其编译为浏览器可以执行的js语法
@babel/core | babel核心包
@babel/preset-env |ES语法分析包

## 开发与生产环境需要的webpack功能区别
1、开发环境(development模式)
* 热插拔调试
* 生成html模板

2、生产环境(production模式)
* 生成html模板
* css样式提取
* 公共模块提取
* JavaScript压缩

## 开始搭建一个vue+webpack简单例子
1、新建一个目录
```sh
mkdir xxx
cd xxx
# 自定义
npm init
或者
# 默认值
npm init -y  
```
2、创建文件
```sh
# 模板文件
touch index.html
# 存放需要渲染的内容
touch App.vue
# 入口文件
touch app.js
# webpack配置
touch webpack.config.js
# .gitignore
touch .gitignore
```
3、引入依赖
```sh
npm install  vue vue-loader webpack webpack-cli vue-template-compiler html-webpack-plugin webpack-dev-server -D 
```

4、文件目录结构
```bash
-xxx
 |--.gitignore  # git忽略提交
 |--app.js      # 入口文件
 |--App.vue     # 需要渲染展示的内容
 |--index.html  # 模板插槽
 |--pageage-lock.json # 锁定所有模块的版本号
 |--pageage.json # 记录你项目中所需要的所有模块
 |--webpack.config.js # webpack配置文件
```

5、文件内容
**index.html**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title></title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```
**App.vue**
```html
<template>
    <div> app content </div>
</template>
```
**app.js**
```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
    render: (h) => h(App)
}).$mount('#app')
```
**webpack.config.js**
```js
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: path.join(__dirname, './app.js'),
  mode: 'development',
  output: {
    filename: 'index.js',
    path: path.join(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /.vue$/,
        loader: 'vue-loader'
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.json', '.css', '.vue'],
    modules: ["node_modules", path.resolve(__dirname, 'app')]
  },
  plugins: [
    new HtmlWebpackPlugin(
      {
        template: 'index.html', // 模版路径
        filename: 'index.html', // 生成的文件名称
        inject: 'body' // 指定插入的<script>标签在body底部
      }
    ),
    new VueLoaderPlugin()
  ],
  devServer: {
    disableHostCheck: true
  }
}
```
**.gitignore**
```text
node_modules/
```
**package.json**
```json
{
  "name": "xxx",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "webpack-dev-server --open --port 3000 --hot --inline --mode=development",
    "build": "webpack --progress --colors --mode=production"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^3.5.3",
    "html-webpack-plugin": "^4.2.1",
    "vue": "^2.6.11",
    "vue-loader": "^15.9.1",
    "vue-template-compiler": "^2.6.11",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3"
  }
}
```

6、执行命令会自动打开浏览器，查看是否正常显示出App.vue里面的内容
```sh
# 开发环境
npm run dev
# 生产环境
npm run build
```


## 参考
* [webapck](https://www.webpackjs.com/concepts/)
* [vue](https://cn.vuejs.org/v2/guide/)