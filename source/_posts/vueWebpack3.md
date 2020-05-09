---
title: vue webpack 从零开始搭建项目框架3
date: 2020-05-07 15:01:24
categories: 
- Vue-Webpack
tags: 
- vue
- webpack
---
> **注意** 
接着“vue webpack 从零开始搭建项目框架2”继续扩展
1、添加路由支持(vue-router)
2、添加状态管理支持(vuex)
--------------

## 安装前提
node10^ npm6^ vue2.6^ webpack^4

## 简写说明
> -S --> --save
> -D --> --save-dev

## 新增功能
1. 添加路由支持
```sh
npm install vue-router -S
```
1. 添加vuex支持
```sh
npm install vuex -S
```

## 添加路由代码片段
1、修改public目录下的index.html文件
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title></title>
</head>
<body>
  <div id="app"><router-view/></div>
</body>
</html>
```
2、删除App.vue
3、在src目录新建router文件夹，router文件夹下新建index.js文件
```js
// src/router/index.js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const A = {template: '<div>aaa</div>'}
const B = {template: '<div>bbb</div>'}

const routes = [
  {
    path: '/',
    component: A 
  },
  {
    path: '/b',
    component: B
  }
]

export default new VueRouter({
  routes
})
```
4、修改app.js
```js
import Vue from 'vue'
import router from './router'
import './assets/css/index.css'

new Vue({
    router,
}).$mount('#app')
```
5、执行代码就就可以看到效果了

## 添加状态管理代码片段
1、在src目录下新建store文件夹，在store目录新建index.js文件
```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```
2、修改app.js文件
```js
import Vue from 'vue'
import store from './store'
import router from './router'
import './assets/css/index.css'

new Vue({
    store,
    router,
}).$mount('#app')
```
3、在src目录下新建views文件夹，在views目录下新建Count.vue
```html
<template>
  <div>
    <button @click="increment">添加</button>
    {{this.$store.state.count}}
  </div>
</template>
<script>

export default {
  methods: {
    increment() {
      this.$store.commit('increment')
      console.log(this.$store.state.count)
    }
  }
}
</script>
```
4、修改路由配置
```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const A = {template: '<div>aaa</div>'}
const B = {template: '<div>bbb</div>'}

const routes = [
  {
    path: '/',
    component: A 
  },
  {
    path: '/b',
    component: B
  },
  {
    path: '/count',
    component: () => import('../views/Count.vue')
  }
]

export default new VueRouter({
  routes
})
```
5、执行代码就就可以看到效果了

## 参考
* [Vue Router](https://router.vuejs.org/zh/)
* [Vue Vuex](https://vuex.vuejs.org/zh/)

