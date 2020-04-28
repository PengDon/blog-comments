---
title: vue-cli3.0版本 prerender 预渲染
date: 2020-04-28 11:01:27
categories: 
- Vue
tags: 
- vue
- prerender-spa-plugin
- seo
---

### 前提
node10^ 、 npm6^ 、vue-cli3

### 简写
-S --> --save  // 生产阶段的依赖
-D --> --save-dev  // 开发阶段的依赖

### 普通场景下的预渲染
1、利用手脚架新建个项目
```sh
vue create vue3-prerender
cd vue3-prerender
npm install
npm run serve
```

2、项目大致结构如下：
```
vue3-prerender
--node_modules
--public
--src
--tests
--.browserslistrc
--.editorconfig
--.eslintrc.js
--.gitignore
--babel.config.js
--jest.config.js
--package.json
--package-lock.json
--README.md
```

3、运行项目
```sh
npm run serve
# 可以看到有两个视图Home、About
```


4、预渲染插件安装
```sh
# 设置镜像下载可以加速下载
npm config set PUPPETEER_DOWNLOAD_HOST=https://npm.taobao.org/mirrors && npm install prerender-spa-plugin -D
```

5、新建vue.config.js文件配置预渲染,文件内容如下：
```js
// 普通场景下的预渲染
const path = require('path')
const PrerenderSPAPlugin = require('prerender-spa-plugin')
const Renderer = PrerenderSPAPlugin.PuppeteerRenderer

module.exports = {
  publicPath:'/',
  configureWebpack:config=>{
    // 生产环境
    if (process.env.NODE_ENV === 'production') {
      // 预渲染配置
      new PrerenderSPAPlugin({
        // 默认输出是dist目录
        staticDir: path.join(__dirname, 'dist'),
        // 必需，要渲染的路线，根据自己定义的路由配置
        routes: ['/', '/about'],
        // 必须，要使用的实际渲染器，没有则不能预编译
        renderer: new Renderer({
          inject: {},
          headless: false, // 渲染时显示浏览器窗口。对调试很有用。
          renderAfterDocumentEvent: 'render-event'
        })
      })
    }
  }
}
```

6、修改main.js文件,修改后内容如下：
```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App),
  mounted () {
    // 这句非常重要，否则预渲染将不会启动
    document.dispatchEvent(new Event('render-event'))
  }
}).$mount('#app')

```

7、修改router目录下的index.js文件，修改内容如下：
```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '@/views/Home.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: () => import('../views/About.vue')
  }
]

const router = new VueRouter({
  mode: 'history', // 预渲染一定要模式改成history
  routes
})

export default router

```
8、 执行构建打包命令
```sh
npm run build
```

9、 可以看到根目录多了个dist文件夹，目录结构如下：
```
-dist
--about
--static
--favicon.ico
--index.html
```
* 在vue.config.js的预渲染插件路由数组里面配置了几个路由就会生成相应的静态文件

### 特殊场景下的预渲染

#### 场景描述
1、例如公司主域名是 www.abc.com，现在开发的项目是挂载在主域名下的子目录，也就是通过https://www.abc.com/edu访问
2、由于目前路由是history模式，子目录的场景需要改路由的base属性、vue.config.js的publicPath属性，影响到预渲染的其他配置

#### 具体操作针对普通场景配置做修改
1、修改router目录下的index.js文件，内容如下：
```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '@/views/Home.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: () => import('../views/About.vue')
  }
]

const router = new VueRouter({
  mode: 'history', // 预渲染一定要模式改成history
  base: '/edu/', // 这个根据实际场景自己配置，但要与vue.config.js文件中的publicPath属性保持一致
  routes
})

export default router

```
2、修改vue.config.js文件，内容如下：
```js
// 普通场景下的预渲染
const path = require('path')
const PrerenderSPAPlugin = require('prerender-spa-plugin')
const Renderer = PrerenderSPAPlugin.PuppeteerRenderer

module.exports = {
  publicPath: process.env.NODE_ENV === 'production'?'/edu/':'/',
  outputDir: 'dist/edu', 
  configureWebpack:config=>{
    // 生产环境
    if (process.env.NODE_ENV === 'production') {
      // 预渲染配置
      new PrerenderSPAPlugin({
        staticDir: path.join(__dirname, 'dist/'),
        outputDir: path.join(__dirname, 'dist/edu'),
        indexPath: path.join(__dirname, 'dist', '/edu/index.html'),
        // 必需，要渲染的路线，根据自己定义的路由配置
        routes: ['/', '/about'],
        // 必须，要使用的实际渲染器，没有则不能预编译
        renderer: new Renderer({
          inject: {},
          headless: false, // 渲染时显示浏览器窗口。对调试很有用。
          renderAfterDocumentEvent: 'render-event'
        })
      })
    }
  }
}
```
3、 执行构建打包命令
```sh
npm run build
```

4、 可以看到根目录多了个dist文件夹，目录结构如下：
```
-dist
--edu
---about
---static
---favicon.ico
---index.html
```

### 常见问题以及解决方案
#### 问题1：项目打包发布到服务器上后，刷新页面会出现404问题
> **原因** 路由是history模式导致的
> **解决方案** 修改服务器相关nginx.conf配置
```sh
location ~* ^/edu {
  try_files $uri $uri/ /index.html; # 解决vue路由history模式打包到生产，刷新页面出现404的问题
  index index.html index.htm;
  if ( !-e $request_filename ) {
    rewrite ^(.*) /edu/index.html;
    break;
  }
}
```



### 相关参考
[Vue](https://cn.vuejs.org/v2/guide/)
[Vue CLI](https://cli.vuejs.org/zh/guide/)
[Webpack4^](https://webpack.js.org/concepts/)
[webpack-chain](https://github.com/neutrinojs/webpack-chain)
[puppeteer](https://zhaoqize.github.io/puppeteer-api-zh_CN/#/)
[prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin)
















