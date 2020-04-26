---
title: 1 vue-ssr基本用法 
date: 2019-11-14 20:11:27
categories: 
- Vue-SSR
tags: 
- vue
---

### 基础

#### 1. 先创建一个最基础的Nextjs项目
```bash
    mkdir vue-srr-demo
    cd vue-srr-demo
    npm init -y
    npm install vue vue-server-renderer --save
    
```
#### 2. 创建一个app.js文件,内容如下：
```bash
    const Vue = require('vue')
    module.exports = function createApp (context) {
        return new Vue({
            data: {
              url: context.url
            },
            template: `<div>Vue SSR URL: {{ url }}</div>`
        })
    }
```
#### 2. 与服务器集成
```bash
    npm install express --save
```
#### 3. 创建一个server.js文件,内容如下：
```bash
    const server = require('express')()

    server.get('/ssr', (request, response) => {
       response.send("当前访问URL "+request.url)
    })

    server.listen(8000)
```
#### 4. 创建一个页面模板index.html,内容如下：
```bash
    <!doctype html>
    <html lang="en">
    <head><title></title></head>
    <body>
    <!--vue-ssr-outlet-->
    </body>
    </html>
```
#### 5. 修改server.js文件，内容如下：
```bash
    const server = require('express')()
    const createApp = require('./app')
    const renderer = require('vue-server-renderer').createRenderer()

    server.get('/ssr', (request, response) => {
        const context = { url: request.url }
        const app = createApp(context)
        renderer.renderToString(app, (err, doc) => {
        if (err) throw err
        response.send(doc)
        })
    })

    server.listen(8000)
```
#### 6. 启动服务查看效果
```bash
    node server.js
    # 或者安装nodemon自动重启插件[npm install -g  nodemon]
    nodemon server.js
```
#### 7. 在浏览器访问：http://localhost:8000/ssr 
```bash
    Vue SSR URL: /ssr
```
#### 8. 模板插值，修改index.html
```bash
    <html>
    <head>
        <!-- 使用双花括号(double-mustache)进行 HTML 转义插值(HTML-escaped interpolation) -->
        <title>{{ title }}</title>

        <!-- 使用三花括号(triple-mustache)进行 HTML 不转义插值(non-HTML-escaped interpolation) -->
        {{{ meta }}}
    </head>
    <body>
        <!--vue-ssr-outlet-->
    </body>
    </html>
```
#### 9. 修改server.js文件，内容如下：
```bash
    const server = require('express')()
    const createApp = require('./app')
    const renderer = require('vue-server-renderer').createRenderer({
        template: require('fs').readFileSync('./index.html', 'utf-8')
    })

    server.get('/ssr', (request, response) => {
        const context = {
            url: request.url, 
            title: 'vue-ssr', 
            meta: 
                `
                <meta ...>
                <meta ...>
                `
            }
        const app = createApp(context)
        renderer.renderToString(app, context, (err, doc) => {
            if (err) throw err
            response.send(doc)
        })
    })

    server.listen(8000)
```
#### 10. 启动服务，查看网页源代码，发现title和meta标签成功插入
```bash
    node server.js
    # 或者安装nodemon自动重启插件[npm install -g  nodemon]
    nodemon server.js
```















