---
title: egg 简易例子1
date: 2020-12-07 11:27:17
categories: node后端框架
tags: 
- egg
---

## 闲话，如果熟悉以下知识点上手更易
* 熟悉node、express、koa等了解中间件、网络协议、数据传输、数据库（mysql、mongodb）等
* 有其他后台开发经验为佳，比如：java、php、c\#等
* 熟悉MVC开发模式会很快上手

## 构建简易例子

1、初始化项目结构,在命令行输入以下命令：
```sh
mkdir 项目名
cd 项目名
npm init
npm i egg --save
npm i egg-bin --save-dev
mkdir config
mkdir app
```

2、添加 npm scripts 到 package.json：
```json
{
  "name": "项目名称",
  "scripts": {
    "dev": "egg-bin dev"
  }
}
```

3、编写 Controller,在命令行输入以下命令：
```sh
 cd app
 mkdir controller
 cd controller
 touch home.js
```
> app/controller/home.js
```js
const Controller = require('egg').Controller;
class HomeController extends Controller {
  async index() {
    this.ctx.body = 'egg';
  }
}
module.exports = HomeController;
```

4、配置路由映射,在命令行输入以下命令：
```sh
cd app
touch router.js
```
> app/router.js
```js
module.exports = app => {
  const { router, controller } = app;
  router.get('/', controller.home.index);
};
```

5、添加一个配置文件
```sh
cd config
touch config.default.js
```
> config/config.default.js
```js
exports.keys = <此处改为你自己的 Cookie 安全字符串>;
```

6、此时目录结构如下
```
项目名
├── app
│   ├── controller        -- 用于解析用户的输入，处理后返回相应的结果
│   │   └── home.js
│   └── router.js         -- 路由
|   ├── service           -- 用于编写业务逻辑层
├── config
│   └── config.default.js -- 用于编写配置文件
|   └── plugin.js         -- 用于配置需要加载的插件
└── package.json
```

7、检测效果

```sh
 npm run dev
 open http://localhost:7001
```



## 参考
* egg     官网地址：[https://eggjs.org/zh-cn/](https://eggjs.org/zh-cn/)
* koa     官网地址：[https://koa.bootcss.com/](https://koa.bootcss.com/)
* express 官网地址：[https://www.expressjs.com.cn/](https://www.expressjs.com.cn/)
* node   文档地址：[http://nodejs.cn/api/](http://nodejs.cn/api/)

