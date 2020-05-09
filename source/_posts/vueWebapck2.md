---
title: vue webpack 从零开始搭建项目框架2
date: 2020-04-30 10:13:10
categories: 
- Vue-Webpack
tags: 
- vue
- webpack
---

> **注意** 
接着“vue webpack 从零开始搭建项目框架1”继续扩展
1、增加了目录结构
2、增加了处理css/js/字体/图片资源的加载器
--------------

## 安装前提
node10^ npm6^ vue2.6^ webpack^4

## 简写说明
> -S --> --save
> -D --> --save-dev

## 回顾
> 上一章搭建了最基本的结构，这章会逐步完善结构

1、上一章代码目录结构
```bash
-xxx
 |--.gitignore  # git忽略提交
 |--app.js      # 入口文件
 |--App.vue     # 需要渲染展示的内容
 |--index.html  # 模板插槽
 |--pageage-lock.json # 锁定所有模块的版本号
 |--pageage.json      # 记录你项目中所需要的所有模块
 |--webpack.config.js # webpack配置文件
```

2、准备改造成以下目录
```bash
-xxx
 |--.gitignore     # git忽略提交
 |--public         # 用来存放公共静态资源
    |--index.html  # 模板插槽
 |--src            # 主要存放开发文件
    |--app.js      # 入口文件
    |--App.vue     # 需要渲染展示的内容
 |--pageage-lock.json # 锁定所有模块的版本号
 |--pageage.json      # 记录你项目中所需要的所有模块
 |--webpack.config.js # webpack配置文件
```

3、修改webpack.config.js配置文件
```js
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: path.join(__dirname, './src/app.js'),
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
        template: './public/index.html', // 模版路径
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

4、执行命令会自动打开浏览器，查看是否正常显示出App.vue里面的内容
```sh
# 开发环境
npm run dev
# 生产环境
npm run build
```

## 准备解决的问题
1、解决加载css文件 
>  style-loader、css-loader

2、解决js文件ES6转ES5代码兼容各个浏览器运行要求
>  babel-loader、@babel/core、@babel/preset-env

3、解决css浏览器兼容性，自动补全前缀
>  postcss-loader、autoprefixer

4、解决样式分离、提取css(webpack4)
>  mini-css-extract-plugin

5、解决声明了很多样式，但部分样式并没有用到，造成了css冗余，配合extract-text-webpack-plugin@next使用
>  purifycss-webpack、purify-css

6、解决加载静态文件(如：图片、字体等)
>  file-loader、url-loader


## 依赖插件安装
```sh
npm install style-loader css-loader babel-loader @babel/core @babel/preset-env postcss-loader autoprefixer -D
npm install mini-css-extract-plugin purifycss-webpack purify-css file-loader url-loader -D
```

1、变化后的webpack.config.js文件
```js
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const Glob = require('glob');
const PurifyCssWebpack = require('purifycss-webpack');
const IsProduction = process.env.NODE_ENV === 'production';

module.exports = {
  entry: path.join(__dirname, './src/app.js'),
  mode: 'development',
  // 源码映射,参考：https://webpack.docschina.org/configuration/dev-server/#devserver
  devtool: IsProduction ? 'source-map' : 'eval-source-map',
  output: {
    filename: 'index.js',
    path: path.join(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      {
        test: /\.js$/,
        use: ['babel-loader'],
        include: path.resolve(__dirname, 'src'),
        exclude: path.resolve(__dirname, 'node_modules')
      },
      {
        test: /\.css$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              // 在css-loader 之后指定1个数量的loader（即 postcss-loader）来处理import进来的资源
              importLoaders: 1
            }
          },
          'postcss-loader'
        ]
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            // url-loader封装了file-loader
            // 1.文件大小小于limit参数，url-loader将会把文件转为DataURL（Base64格式）；
            // 2.文件大小大于limit，url-loader会调用file-loader进行处理，参数也会直接传给file-loader
            loader: 'url-loader',
            options: {
              // 把小于500000B的文件打成Base64的格式。
              limit: 500000
            }
          }
        ]
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
        title: 'home',
        template: './public/index.html', // 模版路径
        filename: 'index.html', // 生成的文件名称
        inject: 'body' // 指定插入的<script>标签在body底部
      }
    ),
    new VueLoaderPlugin(),
    new PurifyCssWebpack({
      paths: Glob.sync(path.join(__dirname, '*.html'))
    })
  ],
  devServer: {
    disableHostCheck: true
  }
}
```

## 常见问题

> **问题描述** npm run dev 时看到“DeprecationWarning: Tapable.plugin is deprecated. Use new API on `.hooks` instead”
  **解决方法** 参考： https://webpack.docschina.org/plugins/extract-text-webpack-plugin/#src/components/Sidebar/Sidebar.jsx 和 https://webpack.docschina.org/plugins/mini-css-extract-plugin/

> **问题描述** npm run dev 时看到“Replace Autoprefixer browsers option to Browserslist config. ”
  **解决方法** 修改postcss.config.js文件
```js
// 源文件内容
module.exports = {
  plugins: {
    'autoprefixer': { browsers: 'last 5 version' }
  }
}
// 修改后文件内容
module.exports = {
  plugins: [
    require('autoprefixer')({
      overrideBrowserslist: ['> 0.15% in CN']
    })
  ]
}
```

## 参考
* [webapck](https://www.webpackjs.com/concepts/)
* [vue](https://cn.vuejs.org/v2/guide/)