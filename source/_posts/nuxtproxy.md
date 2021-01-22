---
title: nuxt本地开发跨域场景
date: 2021-1-22 16:03:32
categories:
  - nuxt
tags:
  - proxy
---

## 场景1 单域名跨域

### **举个例子**
* 接口API：https://a.xx.com/book/23

### 修改方案
1、确定是否有安装`@nuxt/proxy`,没有则执行下面命令安装：
```sh
npm i -D @nuxt/proxy
```

2、配置`nuxt.config.js`文件
```js
// 这个根据具体场景配置
axios: {
    // Can be also an object with default options
    // baseURL: 'http://api.example.com/xx',
    // retry: true, // 请求失败重试（仅限3次）
    // debug: false,
    // proxy: 'http://api.example.com/xx', // 设置代理接口
    // prefix: '/api', // 表示给请求url加个前缀
    https: true,
    credentials: true, // 表示跨域请求时是否需要使用凭证
    proxy: true, // 表示开启代理
},

// 接口反代理
proxy: {
    '/m': {
        target: 'https://a.xx.com',
        changeOrigin: true, // 表示是否跨域
        secure: false, // 如果是https接口，需要配置这个参数，忽略证书校验
        pathRewrite: {
        '^/m': '',
        },
    },
},

```

3、还需要修改`axios.js`的封装文件，加上以下代码片段：
```js
import https from 'https'

const agent = new https.Agent({
  rejectUnauthorized: false,
})

export default function ({ $axios, redirect }) {
  // 请求处理
  $axios.onRequest((config) => {
    // 解决： Nuxtjs-Error：unable to verify the first certificate
    if (process.env.NODE_ENV === 'development') {
      // 开发环境，取消https证书校验
      config.httpsAgent = agent
    }

  })
}
```




