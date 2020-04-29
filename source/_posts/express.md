---
title: express基础
date: 2020-04-28 15:35:11
categories: 
- Node
tags: 
- Express
---

## Express 是什么？
> 官方概念：基于 Node.js 平台，快速、开放、极简的 Web 开发框架

## 优势
* 小巧灵活：Express 是一个保持最小规模的灵活的 Node.js Web 应用程序开发框架，为 Web 和移动应用程序提供一组强大的功能
* 易上手：express对web开发相关的模块进行了适度的封装，屏蔽了大量复杂繁琐的技术细节，让开发者只需要专注于业务逻辑的开发，极大的降低了入门和学习的成本
* 高性能：express仅在web应用相关的nodejs模块上进行了适度的封装和扩展，较大程度避免了过度封装导致的性能损耗
* 扩展性强：基于中间件的开发模式，使得express应用的扩展、模块拆分非常简单，既灵活，扩展性又强


## 怎么使用Express？
1、安装express
```sh
npm install express --save
```
2、代码最基本结构
```js
// 1、引入express模块
const express = require('express')
// 2、创建 express 实例
const app = express()
// 3、自定义路由， 响应HTTP的GET方法
app.get('/', (req, res) => res.send('Hello World!'))
// 4、监听3000端口请求
app.listen(3000, () => console.log(`Example app listening on port ${port}!`))
```

3、一个典型的使用express的app.js，主要做了以下几件事：
* 引入express模块
* 创建 express 实例
* 使用app.set设置express内部的一些参数(options)
* 使用app.use来注册函数
* 通过http.createServer用app来处理请求


## 使用场景
1、作为资源服务器
2、开发web项目
3、作为中间件提供服务

## 配置设置
* express可以使用 set(setting,value)、 enable(setting)、disable(setting)方法来配置
* 下面是可以配置的变量：

名称|作用|默认值
----|----|-----
env |定义环境模式字符串，如development,testing,production|process.env.NODE_ENV
trust proxy| 启用/禁用反向代理的支持|false
jsonp callback name| 定义JSONP请求的默认回调名称|?callback=
json replacer|定义JSON replacer回调函数|null
json spaces|指定当格式化JSON响应时使用的空格数量|开发中为2，在生产为0
case sensitive routing |启用/禁用区分大小写，如/home与/Home是不一样的|disabled
strict routing | 启用/禁用严格的路由，如/home和/home/是不一样的|disabled
view cache |启用/禁用视图模板编译缓存|enabled
view engine|指定呈现模板时，如果从视图中省略了文件扩展名，应该使用的默认模板引擎扩展|
views | 指定模板引擎用来查找视图模板的路径|./views


## 配置路由
* 语法
```js
app.get(path,[middleware],callback)
app.post(path,[middleware],callback)
// middleware是回调函数执行前要应用的中间件函数
// callback是应该处理该请求并把响应发回给客户端的请求处理程序
// callback以Request对象作为第一个参数，以Response对象作为第二个参数
```
* 全路径匹配
```js
// 全部路径的全局处理程序
app.all('*',function(req,res){
   
});
```
* 无参数场景
```js
app.get('/',function(req,res){
    // 发送各种类型的响应
    res.send("get");
});
app.post('/',function(req,res){
    // 发送各种类型的响应
    res.send("post");
});
```
* 有参数场景
```js
app.get('/getUserInfo',function(req,res){
  let params = req.query; // get传递过来的参数
  res.send("get params"+params.id);
});
// eg: get 请求 /getUserInfo?id=u123
// params.id -> u123
```
* 正则匹配参数
```js
app.get(/^\/money\/(\w+)\:(\w+)?$/.function(req,res){
	res.send('get money ' + req.params[0] + req.params[1]);
});
// eg: get 请求  /money/10:66
// req.params[0]->10  req.params[1]->66
```
* 已定义的参数
```js
app.get('/user/:userid',function(req,res){
	res.send("Get User: " + req.param("userid"));
});
// eg: get 请求  /user/u123
// req.param("userid") -> u123
```
* 已定义的参数应用回调函数
```js
// 这里的next是一个用于已注册的下一个app.param()回调的回调函数，必须要在回调函数中的某处调用next()，否则回调链会被破坏。value是从URL路径解析的参数的值。
app.param(param,function(req,res,next,value){})
```

## Request对象

### 属性
 名称| 作用
---- | ----
originalUrl | 请求的原始URL字符串
protocol |  协议的字符串，如http或https
ip  |  请求的ip地址
path  |  请求的路径部分
host  | 请求的主机名
method |   HTTP方法
query  |   请求的URL的查询字符串部分
fresh  |  一个布尔值，当最后修改与当前匹配时为true
stale  | 一个布尔值，当最后修改与当前匹配时为false
secure  | 一个布尔值，当建立TLS连接时为true
acceptsCharset(charset) |  一个方法，如果由charset指定的字符集受支持，返回true
get(header)   |   返回header的值的方法
headers   |  请求标头的对象形式


## Response对象

### 设置标头
 名称| 作用
---- | ----
get(header)   | 返回指定的header参数的值
set(header,value)  |  设置header参数的值
set(headerObj) |   接受一个对象，包括多个'header':'value'属性
locatio(path)  |  把location标头设置为指定的path参数
type(type_string) |   根据type_string参数设置Content-Type标头
attachment([filepath]) |   把Content-Disposition标头设置为attachment，并且如果指定filepath，则Content-Type头是基于文件扩展名设置的

### 设置状态
 名称| 作用
---- | ----
200 |  正确
300  |  Rediction重定向
400  |   Bad Request错误的请求
401 | Unauthorized未经许可
403 |  Forbidden禁止
500 | Server Error服务器错误

### 发送响应
res.send(status,[body])
body是一个String或者Buffer对象，如果指定Buffer对象，内容类型就被自动设置为application/octet-stream(应用程序/八位字节流)

### 发送JSON响应
res.json(status,[object])
```js
app.get('/json',function(req,res){
    app.set('json spaces',4);
    res.json({name:'bob',built:'1223',centers:['art','maths']});
});
// jsonp callback name被设置为cb,意味着客户端需要在URL中传递的不是?callback=<function>，而是?cb=<function>
app.get('jsonp',function(res,req){
    app.set('jsonp callback name','cb');
    res.jsonp({name:'bob',built:'1223',centers:['art','maths']});
});
```
### 发送文件
res.sendFile(path,[options],[callback])
```js
// path指向你要发送给客户端的文件，options参数是一个对象，
// 包含了一个maxAge属性定义的最长期限和root属性(用来支持path参数相对路径的根路径)
// 当文件传输完成时，回调函数被调用，并接受一个错误作为唯一的参数
app.get('/image',function(req,res){
    res.sendFile('arch.jpg',{maxAge:1,root:'./views'},
        function(err){
            if(err){
                console.log('Error');
            }else{
                console.log('Success');
            }
        });
});
```

### 发送下载响应
res.download(path,[filename],[callback])

### 重定向响应
res.redirect(path);

## 中间件

1. Express提供的大部分功能是通过中间件函数完成的，这些中间件函数在nodejs收到请求的时点和发送响应的时点之间执行。Express的connect模块提供了中间件框架，让你方便的在全局或路径级别或为单个路由插入中间件功能。通过Express支持的中间件可以让你快速提供静态文件，实现cookie，支持会话，处理post数据等等，你甚至可以创建自己的自定义中间件函数，并利用它们来预处理请求和提供自己的功能。
2. 有哪些中间件

名称 | 作用
---- | ----
static |  允许Express服务器以流式处理静态文件的GET请求，这个中间件是Express内置的，它可以通过express.static()访问
express-logger |  实现一个格式化的请求记录器来在跟踪对服务器的请求
basic-auth-connect  |  提供对基本的HTTP身份验证的支持
cookie-parser  | 可以从请求读取cookie并在响应中设置cookie
cookie-session |  提供基于cookie的会话支持
express-session |  提供了一个强大的会话支持
body-parser |  把POST请求正文中的JSON数据解析为req.body属性
compression | 对发给客户端的大响应提供Gzip压缩支持
caurf   |  提供跨站点请求伪造保护

3. 安装中间件
```sh
npm install 中间名称 --save
```
4. 分配中间件
  1> 在全局范围内把中间件分配给某个路径
  ```js
  // 要对所有路由指定中间件，可以在Express app对象上实现Use()方法：
  // use([path],middleware)
  // 其中，path是可选的，默认为/，意味着所有的路径，middleware是一个函数，
  // 即function(req,res,next){}，每个中间件函数都有一个构造函数，它返回相应的中间件功能。
  // next是要执行的下一个中间件函数。
  // 例如，把body-parser中间件应用于所有路径：
  var express=require('express');
  var bodyParser=require('body-parser');
  var app=express();
  app.use('/',bodyParser());
  ```
  2> 把中间件分配到单个路由
  ```js
  var express=require('express');
  var bodyParser=require('body-parser');
  var app=express();
  app.get('/parseRoute',bodyParser(),function(req,res){
          // coding
  });
  ```
  3> 添加多个中间件函数
  ```js
  // 可以根据需要在全局范围和路由上分配任意多的中间件函数
  var express=require('express');
  var bodyParser=require('body-parser');
  var cookieParser=require)('cookie-parser');
  var session=require('express-session');
  var app=express();
  // 注意，你分配函数的顺序就是它们在请求中被应用的顺序。一些中间件需要被添加在别的中间件函数前面。
  app.use('/',bodyParser()).use('/',cookieParser()).use('/',session());
  ```

## 常见问题

**如何处理 404 响应？**
```js
app.use(function (req, res, next) {
  res.status(404).send("Sorry can't find that!")
})
```

**如何设置一个错误处理器？**
```js
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

**如何渲染纯 HTML 文件？**
```js
// 可以通过 res.sendFile() 直接对外输出 HTML 文件
res.readFile('index.html')
// 如果你需要对外提供的资源文件很多，可以使用 express.static() 中间件
app.use(express.static('htmls'));
```


## 参考
[Express](https://www.expressjs.com.cn/)