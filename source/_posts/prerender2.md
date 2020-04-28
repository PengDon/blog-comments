---
title: vue-cli2.0版本 prerender 预渲染
date: 2020-04-28 11:26:07
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

### 预渲染
1、先用手脚架创建个项目
```sh
vue init webpack vue2-prerender
cd vue2-prerender
npm install
npm run dev
```
2、项目大致结构如下：
```
-vue2-prerender
--build
--config
--node_modules
--src
--static
--test
--.babelrc
--.editorconfig
--.eslintignore
--.eslintrc.js
--.gitignore
--.postcssrc.js
--index.html
--package.json
--package-lock.json
--readme.md
```
3、预渲染插件安装
```sh
# 设置镜像下载可以加速下载
npm config set PUPPETEER_DOWNLOAD_HOST=https://npm.taobao.org/mirrors && npm install prerender-spa-plugin -D
```

4、修改build目录下的webpack.prod.conf.js文件
```js
// 1、引入依赖
const PrerenderSpaPlugin = require('prerender-spa-plugin')
const Renderer = PrerenderSpaPlugin.PuppeteerRenderer
// 2、配置插件,在plugins数组里面添加预渲染插件
new PrerenderSpaPlugin({
  staticDir: path.join(__dirname, '../dist'),
  routes: ['/', '/about'],
  renderer: new Renderer({
    inject: {},
    headless: false,
    renderAfterDocumentEvent: 'render-event'
  })
})
```

5、修改mian.js文件，内容如下：
```js
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>',
  mounted () {
    // 这句非常重要，否则预渲染将不会启动
    document.dispatchEvent(new Event('render-event'))
  }
})
```

6、修改router目录的index.js文件
```js
export default new Router({
  mode: 'history', // 预渲染一定要模式改成history
  routes,
  scrollBehavior (to, from, savedPosition) {
    return { x: 0, y: 0 }
  }
})
```
7、 执行构建打包命令
```sh
npm run build
```

8、 可以看到根目录多了个dist文件夹，目录结构如下：
```
-dist
--about
--static
--favicon.ico
--index.html
```