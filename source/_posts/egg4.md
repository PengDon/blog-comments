---
title: egg 简易例子3
date: 2020-12-07 14:14:23
categories: node后端框架
tags: 
- egg
---

## service

1、在app根目录新建service文件夹

```sh
  cd app
  mkdir service
  cd service
  touch users.js
```

2、app/service/users.js
```js
const Service = require("egg").Service;

class UserService extends Service {
  async findAll() {
    const user = await this.app.mysql.select("users");
    return { user };
  }

  async findOne(id) {
    const user = await this.app.mysql.get("users", { id: id });
    return { user };
  }

  async add(params) {
    const date = new Date();
    const result = await this.app.mysql.insert("users", {
      name: params.name,
      password: params.pwd,
      createDate: params.createDate,
      status: params.status
    });
    return { result };
  }

  async delete(id) {
    const result = await this.app.mysql.delete("users", {
      id: id
    });
    return { result };
  }

  async updated(params) {
    // // 如果主键是自定义的 ID 名称，如 self_id，则需要在 `where` 里面配置
    // const options = {
    //   where:{
    //     self_id:params.id
    //   }
    // }
    const row = {
      name: params.name,
      password: params.pwd,
      status: params.status
    };
    // const result = await this.app.mysql.update('users',row,options);
    const result = await this.app.mysql.update("users", row);
    return { result };
  }
}

module.exports = UserService;
```

## Controller
1、在controller目录新增users.js文件
```js
const Controller = require("egg").Controller;

class UserController extends Controller {
  async findAll() {
    const { ctx } = this;
    const user = await ctx.service.users.findAll();
    ctx.body = user;
  }

  async findOne() {
    const { ctx } = this;
    const id = ctx.params.id;
    const user = await ctx.service.users.findOne(id);
    ctx.body = user;
  }

  async add() {
    const { ctx } = this;
    const params = ctx.request.body;
    const result = await ctx.service.users.add(params);
    ctx.body = result;
  }

  async delete() {
    const { ctx } = this;
    const id = ctx.params.id;
    const result = await ctx.service.users.delete(id);
    ctx.boyd = result;
  }

  async update() {
    const { ctx } = this;
    const params = ctx.request.body;
    const result = await ctx.service.users.update(params);
  }
}

module.exports = UserController;
```

## 路由配置

1、在app目录新建routers文件夹
```sh
mkdir routers
cd routers
touch home.js
touch users.js
```

2、app/routers/home.js
```js
module.exports = app => {
  const { router, controller } = app;
  router.get("/", controller.home.index);
};
```

3、app/ruters/users.js
```js
module.exports = app => {
  const { router, controller } = app;
  router.prefix("/api/v1");
  router.get("/users", controller.users.findAll);
  router.get("/users/:id", controller.users.findOne);
  router.post("/users", controller.users.add);
  router.put("/users", controller.users.update);
  router.del("/users/:id", controller.users.delete);
};
```

4、修改route.js文件
```js
module.exports = app => {
  require("./routers/home")(app);
  require("./routers/users")(app);
};
```

## eggjs中extend框架扩展和调用
参考:[https://eggjs.org/en/basics/extend.html](https://eggjs.org/en/basics/extend.html%29)

application.js —— this指向：app对象

调用：this.app

context.js —— this指向：ctx对象

调用：this.ctx

request.js —— this指向：ctx.request对象

调用：this.ctx.request

response.js —— this指向：ctx.response对象

调用：this.ctx.response

helper.js —— this指向：ctx.helper对象

调用：this.ctx.helper

**以helper.js为例\(app/extend/helper.js\)**

```
const moment = require('moment')

/**
 * @description: 格式化时间
 * @param {time} time 时间戳
 * @return: 
 * @example: ctx.helper.formatTime(new Date().getTime()) => 2020-01-16 12:48:43
 */
exports.formatTime = time => moment(time).format('YYYY-MM-DD HH:mm:ss')

/**
 * @description: 处理成功响应
 * @param {Object} ctx 上下文
 * @param {any} res 接口返回的结果对象
 * @param {string} msg 消息内容
 * @return: 
 * @example: ctx.helper.success(ctx,res,'描述') => {"code": 200,"data": res,"msg":"描述"}
 */
exports.success = (ctx, data = null, msg = '请求成功')=> {
  ctx.body = {
    code: 1,
    data,
    msg 
  }
  ctx.status = 200
}
```

**在view视图模板中调用helper对象下的**formatTime**\(\)方法**

```
<span> {{ helper.formatTime(date) }} </span>
```

**在controller中调用helper对象下的success\(\)方法**

```
# app/controller/users.js

async findOne() {
    const { ctx, service } = this;
    const id = ctx.params.id;
    const res = await service.users.findOne(id);
    ctx.helper.success(ctx, res);
}
```

## 中间件
参考:[https://eggjs.org/en/basics/middleware.html](https://eggjs.org/en/basics/middleware.html%29%29)

* 基本写法结构

```js
// 基本结构

// app/middleware/file_name.js
module.exports = options => {
  return async function function_name(ctx, next) {
    // coding
  };
};

// 在config/config.default.js文件里面进行配置
config.middleware = ['function_name']; // 数组的顺序为中间件执行的顺序
```

* 例如：添加404错误处理中间件

```js
// app/middleware/notfound_handler.js

module.exports = () => {
    return async function notFoundHandler(ctx, next) {
      await next();
      if (ctx.status === 404 && !ctx.body) {
        if (ctx.acceptJSON) {
          ctx.body = { error: 'Not Found' };
        } else {
          ctx.body = '<h1>Page Not Found</h1>';
        }
      }
    };
  };

// 在config/config.default.js文件里面进行配置
config.middleware = ["auth",'notfoundHandler']; // 数组的顺序为中间件执行的顺序
```

* router 中使用中间件

```
// 以 app/routers/home.js为例

// 方法一
module.exports = app => {
  const { router, controller } = app;
  const auth = app.middleware.auth(); // 引入auth中间件,括号内可以传递参数给中间件{key: value}
  router.get("/home", controller.home.index);
};

// 方法二
module.exports = app => {
  const { router, controller } = app;
  router.get("/home", auth, controller.home.index); // 只在/home路由使用，放到第二个参数
};
```

