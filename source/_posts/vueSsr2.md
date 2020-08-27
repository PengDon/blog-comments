---
title: nuxt数据请求
date: 2020-08-21 10:40:51
categories:
- SSR
tags:
- vue-ssr
---

## 前提
* win10 64
* node v12.18.3
* npm6 6.14.6


##  对比vue spa项目，axios的使用
> 仅对比vue-cli和create-nuxt-app手脚架自动生成的项目,对比axios的使用

1. vue spa中需要自己手动安装axios依赖，然后在main.js中导入全局初始化

```js
// 1.安装依赖 
npm i -S axios
// 2. 引入
import axios from 'axios'
// 3. 使用
```

2. vue nuxt中自己集成了axios模块到上下文中

```js
1. 在js中使用
export default ({$axios}){

}

2. 在asyncData方法中使用
asyncData({$axios}){

}
// 或
asyncData({app}){
  app.$axios
}
// 或
asyncData(content){
  content.app.$axios
}

3. 也可以这样,和vue spa项目一样使用
import axios from axios

```
## 设置全局拦截器

1. 写法一
```js
export default function ({$axios}) {
  let axios = $axios; 
  // 基本配置
  axios.defaults.timeout = 10000;
  axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
  // 拦截请求
  axios.onRequest(config => {})
  // 拦截响应
  axios.onResponse(res => {})
  // 错误统一处理
  axios.onError(error => {})
}
```

2. 写法二
```js
import axios from 'axios';
// 创建实例对象，避免多次刷新页面请求服务器，服务端渲染会重复添加拦截器，导致数据处理错误
const service = axios.create();
// 基本配置
service.defaults.timeout = 10000;
service.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
// 拦截请求
service.interceptors.request.use(config=>{},rejection=>{});
// 拦截响应
service.interceptors.response.use(res=>{},error=>{});

```



















## 参考
* [nuxt 异步数据](https://zh.nuxtjs.org/guide/async-data)
* [axios](http://www.axios-js.com/)
* []()
* []()



