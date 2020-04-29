---
title: koa2基础
date: 2020-04-29 11:11:59
categories:
  - Node
tags:
  - Koa
toc: true # 是否启用内容索引
---

## 前提

node10^

## 简介

Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

## 安装

```sh
npm i koa
```

## 最基本结构

```js
// 1、引入koa模块依赖
const Koa = require('koa')
// 2、实例化koa对象
const app = new Koa()
// 3、监听3000端口，端口可以自定义
app.listen(3000)
```

## 设置

- app.env 默认是 NODE_ENV 或 "development"
- app.keys 签名的 cookie 密钥数组
- app.proxy 当真正的代理头字段将被信任时
- 忽略 .subdomains 的 app.subdomainOffset 偏移量，默认为 2
- app.proxyIpHeader 代理 ip 消息头, 默认为 X-Forwarded-For
- app.maxIpsCount 从代理 ip 消息头读取的最大 ips, 默认为 0 (代表无限)

## 中间件

- 处在 HTTP Request 和 HTTP Response 中间，用来实现某种中间功能的函数，就叫做"中间件"
- 每个中间件默认接受两个参数，第一个参数是 Context 对象，第二个参数是 next 函数。只要调用 next 函数，就可以把执行权转交给下一个中间件。
- 如果中间件内部没有调用 next 函数，那么执行权就不会传递下去。
- 多个中间件会形成一个栈结构，以"先进后出"的顺序执行。

```js
// 1、最外层的中间件首先执行
// 2、调用next函数，把执行权交给下一个中间件
// 3、coding...
// 4、最内层的中间件最后执行
// 5、执行结束后，把执行权交回上一层的中间件
// 6、coding...
// 7、最外层的中间件收回执行权之后，执行next函数后面的代码
```

## 常用代码片段

1、返回其他类型的内容

```js
// 声明一个main中间件
const main = (async ctx) =>{
if (ctx.accepts('json')) {
    ctx.type = 'json';
    ctx.body = { data: 'json' };
  } else if (ctx.accepts('html')) {
    ctx.type = 'html';
    ctx.body = '<p>html</p>';
  } else if (ctx.accepts('xml')) {
    ctx.type = 'xml';
    ctx.body = '<data>xml</data>';
  } else{
    // 默认返回纯文本
    ctx.type = 'text';
    ctx.body = 'text';
  };
};
// 直接运行页面中会显示json格式，因为我们没有设置请求头，所以每一种格式都是ok的
// app.use()用来加载中间件
app.use(main)
```

2、网页模板

```js
const main = async (ctx) => {
  ctx.type = 'html'
  ctx.body = fs.createReadStream('./data/index.html')
}
app.use(main)
```

3、原生路由

```js
app.use(async (ctx) => {
  if (ctx.request.url == '/') {
    //通过ctx.request.url获取用户请求路径
    ctx.body = '<h1>首页</h1>'
  } else if (ctx.request.url == '/my') {
    ctx.body = '<h1>联系我们</h1>'
  } else {
    ctx.body = '<h1>404 not found</h1>'
  }
})
```

4、koa-router 模块路由

```sh
# 路由安装
npm install koa-router
```

代码片段

```js
app.use(router.routes()).use(router.allowedMethods())
// routes()返回路由器中间件，它调度与请求匹配的路由。
// allowedMethods()处理的业务是当所有路由中间件执行完成之后,若ctx.status为空或者404的时候,丰富response对象的header头.
router.get('/', async (ctx) => {
  ctx.body = '<h1>首页</h1>'
})
router.get('/my', async (ctx) => {
  ctx.body = '<h1>个人中心</h1>'
})
```

5、静态资源(图片、字体、样式表、脚本......)

```sh
# 路由安装
npm install koa-staic
```

代码片段

```js
const path = require('path')
const serve = require('koa-static')
const main = serve(path.join(__dirname))
app.use(main)
```

6、重定向跳转

```js
const Router = require('koa-router')
const router = new Router()

app.use(router.routes()).use(router.allowedMethods())

router.get('/commit', async (ctx) => {
  // 重定向
  ctx.redirect('/login')
})
router.get('/', async (ctx) => {
  ctx.body = '主页'
})
```

7、返回 500 状态码

```js
const main = async (ctx) => {
  // 这个时候你访问首页会报一个500的错误(内部服务器错误)服务器会报错
  ctx.throw(500)
}

app.use(main)
```

8、返回 400 状态码

```js
const main = async (ctx) => {
  // response返回的状态码就是404
  ctx.status = 404
  // 让页面中显示该内容，服务器不不报错
  ctx.body = 'Page Not Found'
}

app.use(main)
```

9、处理错误的中间件

```js
const handler = async (ctx, next) => {
  try {
    // 执行下个中间件
    await next()
  } catch (err) {
    // 如果main中间件是有问题的会走这里
    ctx.status = err.statusCode || err.status || 500
    ctx.body = {
      // 把错误信息返回到页面
      message: err.message,
    }
  }
}

const main = async (ctx) => {
  // 如果这里是没有问题的就正常执行，如果有问题会走catach
  ctx.throw(500)
}

app.use(handler)
app.use(main)
```

10、errors 事件监听

```js
const main = (ctx) => {
  ctx.throw(500)
}
app.on('error', (err, ctx) => {
  // 如果有报错的话会走这里
  console.error('server error', err) //err是错误源头
})

app.use(main)
```

11、释放 error 事件

```js
const handler = async (ctx, next) => {
  try {
    await next()
  } catch (err) {
    ctx.response.status = err.statusCode || err.status || 500
    ctx.response.type = 'html'
    ctx.response.body = '<p>有问题，请与管理员联系</p>'
    // 释放error事件
    ctx.app.emit('error', err, ctx)
  }
}

const main = (ctx) => {
  ctx.throw(500)
}

app.on('error', (err) => {
  // 释放error事件后这里的监听函数才可生效
  console.log('错误', err.message)
  console.log(err)
})

app.use(handler)
app.use(main)
```

12、简单日志中间件例子
./logger/koa-logger.js

```js
module.exports = (ctx, next) => {
  // 自定义
  console.log(`${Date.now()} ${ctx.method} ${ctx.url}`)
}
```

./logger.js

```js
const koaLogger = require('./logger/koa-logger')
app.use(koaLogger)
```

13、简单异步中间件(必须写成 async 函数)

```sh
npm install fs.promised
```

```js
const fs = require('fs.promised');

const main = async (ctx, next) {
    ctx.type = 'html';
    ctx.body = await fs.readFile('./data/index.html', 'utf8');
};

app.use(main);
```

13、中间件合成例子

```sh
npm install koa-compose
```

```js
const compose = require('koa-compose')

const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.method} ${ctx.url}`)
  next()
}

const main = (ctx) => {
  ctx.body = '主页'
}
// 合成中间件
const middlewares = compose([logger, main])

app.use(middlewares)
```

14、读写 cookie

```js
const main = (ctx) => {
  // 读取cookie,没有返回0
  const n = Number(ctx.cookies.get('view') || 0) + 1
  // 设置cookie
  ctx.cookies.set('view', n)
  // 显示cookie
  ctx.response.body = n + ' views'
}

app.use(main)
```

15、模拟表单 post 请求

```sh
npm install koa-body
```

```js
const koaBody = require('koa-body')

const main = async (ctx) => {
  const body = ctx.body
  if (!body.name) {
    ctx.throw(400, '.name required')
  }
  ctx.body = { name: body.name }
}

app.use(koaBody())
app.use(main)
```

16、文件上传

```js
const koaBody = require('koa-body')
const Router = require('koa-router')
const fs = require('fs')
const path = require('path')
const router = new Router()

// 解析多部分主体，默认false
app.use(
  koaBody({
    multipart: true,
    formidable: {
      maxFileSize: 200 * 1024 * 1024, // 设置上传文件大小最大限制，默认2M
    },
  })
)

app.use(router.routes()).use(router.allowedMethods())
// 上传单个文件
router.post('/uploadfile', (ctx, next) => {
  // 获取上传文件
  const file = ctx.request.files.file
  // 创建可读流
  const reader = fs.createReadStream(file.path)
  let filePath = path.join(__dirname, 'data/') + `/${file.name}`
  // 创建可写流
  const upStream = fs.createWriteStream(filePath)
  // 可读流通过管道写入可写流
  reader.pipe(upStream)
  return (ctx.body = '上传成功！')
})
```

17、简易连接 mysql 封装

```sh
npm install --save mysql
```

./util/mysql.js

```js
// 引入mysql依赖
const mysql = require('mysql')
// 创建数据库连接池
const pool = mysql.createPool({
  host: '127.0.0.1', // 数据库服务器地址
  user: 'root', // 数据库用户名
  password: '123456', // 数据库密码
  database: 'test', // 连接的数据库
})
// 调用封装
let query = async (sql, values) => {
  return new Promise((resolve, reject) => {
    pool.getConnection((err, connection) => {
      if (err) {
        reject(err)
      } else {
        connection.query(sql, values, (error, results, fields) => {
          if (err) {
            reject(err)
          } else {
            resolve(results)
          }
          connection.release()
        })
      }
    })
  })
}

module.exports = {
  query,
}
```

./service/mysql.js

```js
// 调用例子
const { query } = require('../util/mysql.js')

const queryUser = async (ctx) => {
  return await query('SELECT username FROM t_user')
}

module.exports = User = {
  queryUser,
}
```

## 参考

- [koa](https://koa.bootcss.com/)
