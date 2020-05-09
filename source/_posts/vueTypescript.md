---
title: vue typescript 简单实现
date: 2020-05-09 14:46:32
categories: 
- Vue-Typescript
tags: 
- vue
- typescript
---

> **注意** 
接着“vue webpack 从零开始搭建项目框架1”的基础上添加typescript支持
--------------

## 回顾vue webpack 从零开始搭建项目框架1 目录结构
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

## 新增说明
名称  | 描述
----  |----  
ts-loader | TypeScript 为 Webpack 提供了 ts-loader，其实就是为了让webpack识别 .ts .tsx文件
tslint、tslint-loader | 约束代码格式
vue-class-component | 强化 Vue 组件，使用 TypeScript/装饰器 增强 Vue 组件
vue-property-decorator | 在 vue-class-component 上增强更多的结合 Vue 特性的装饰器

## 安装依赖
```sh
npm install typescript ts-loader tslint tslint-loader -D
npm install vue-property-decorator vue-class-component -S
```

## 代码片段
1、修改webpack.config.js文件
```js
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: path.join(__dirname, './app.ts'),
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
      },
      {
        test: /\.ts$/,
        exclude: /node_modules/,
        enforce: 'pre',
        loader: 'tslint-loader'
      },
      {
        test: /\.tsx?$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
          appendTsSuffixTo: [/\.vue$/],
        }
      }
    ]
  },
  resolve: {
    extensions: ['.js','.ts', '.json', '.css', '.vue'],
    modules: ["node_modules", path.resolve(__dirname, 'app')]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'index.html', // 模版路径
      filename: 'index.html', // 生成的文件名称
      inject: 'body' // 指定插入的<script>标签在body底部
    }),
    new VueLoaderPlugin()
  ],
  devServer: {
    host: '127.0.0.1',
    port: '8899',
    // contentBase:__dirname,
    compress: true, // enable gzip compression
    hot: true, // hot module replacement. Depends on HotModuleReplacementPlugin
    disableHostCheck: true
  }
}
```
2、添加tsconfig.json文件
```json
{
  "exclude": [
    "node_modules"
  ],
  "compilerOptions": {
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "allowJs": true,
    "module": "esnext",
    "target": "es5",
    "moduleResolution": "node",
    "isolatedModules": true,
    "lib": [
      "dom",
      "es5",
      "es2015.promise"
    ],
    "sourceMap": true,
    "pretty": true
  }
}
```
3、让ts识别*.vue文件,在根目录新建vue.d.ts
```js
declare module "*.vue" {
  import Vue from "vue";
  export default Vue;
}
```
4、修改App.vue文件
```js
<template>
    <div> {{msg}} </div>
</template>
<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class App extends Vue {
    msg:string = 'vue typescript'
}
</script>
```
5、执行命令会自动打开浏览器，查看是否正常显示出App.vue里面的内容
```sh
# 开发环境
npm run dev
# 生产环境
npm run build
```
6、完整的目录结构
```bash
-xxx
 |--.gitignore  # git忽略提交
 |--app.ts      # 入口文件
 |--App.vue     # 需要渲染展示的内容
 |--index.html  # 模板插槽
 |--pageage-lock.json # 锁定所有模块的版本号
 |--pageage.json  # 记录你项目中所需要的所有模块
 |--vue.d.ts      # 类型定义文件
 |--tsconfig.json # typescript配置文件
 |--webpack.config.js # webpack配置文件
```

7、tsconfg.json配置文件说明
```js
{
  // 编译选项
  "compilerOptions": {
    // 输出目录
    "outDir": "./output",
    // 是否包含可以用于 debug 的 sourceMap
    "sourceMap": true,
    // 以严格模式解析
    "strict": true,
    // 采用的模块系统
    "module": "esnext",
    // 如何处理模块
    "moduleResolution": "node",
    // 编译输出目标 ES 版本
    "target": "es5",
    // 允许从没有设置默认导出的模块中默认导入
    "allowSyntheticDefaultImports": true,
    // 将每个文件作为单独的模块
    "isolatedModules": false,
    // 启用装饰器
    "experimentalDecorators": true,
    // 启用设计类型元数据（用于反射）
    "emitDecoratorMetadata": true,
    // 在表达式和声明上有隐含的any类型时报错
    "noImplicitAny": false,
    // 不是函数的所有返回路径都有返回值时报错。
    "noImplicitReturns": true,
    // 从 tslib 导入外部帮助库: 比如__extends，__rest等
    "importHelpers": true,
    // 编译过程中打印文件名
    "listFiles": true,
    // 移除注释
    "removeComments": true,
    "suppressImplicitAnyIndexErrors": true,
    // 允许编译javascript文件
    "allowJs": true,
    // 解析非相对模块名的基准目录
    "baseUrl": "./",
    // 指定特殊模块的路径
    "paths": {
      "jquery": [
        "node_modules/jquery/dist/jquery"
      ]
    },
    // 编译过程中需要引入的库文件的列表
    "lib": [
      "dom",
      "es2015",
      "es2015.promise"
    ]
  }
}
```


## 参考
* [Vue Typescript](https://cn.vuejs.org/v2/guide/typescript.html)
* [Typescript](https://www.tslang.cn/index.html)
