---
title: tornado入门
date: 2020-06-28 14:24:27
categories: python
tags: tornado
---

## 前提
python3.8
win10 64
vscode

## 简介
&ensp;&ensp;&ensp;&ensp;Tornado是一个开源的网络服务器框架，它是基于社交聚合网站FriendFeed的实时信息服务开发而来的。轻量级web框架，功能少而精，注重性能优越

## 特点与优势
1. 轻量级Web框架
1. 异步非阻塞IO处理方式。Tornado采用的单进程单线程异步IO的网络模式，其高性能源于Tornado基于Linux的Epoll（UNIX为kqueue）的异步网络IO。
1. 出色的抗负载能力
1. 优异的处理性能，不依赖多进程/多线程，一定程度上解决C10K问题
1. WSGI全栈替代产品。WSGI把应用（Application）和服务器（Server）结合起来，Tornado既可以是WSGI应用也可以是WSGI服务。
1. 既是WebServer也是WebFramework

> C10K—— Concurrently handling ten thousand connections，即并发10000个连接。对于单台服务器而言，根本无法承担，而采用多台服务器分布式又意味着高昂的成本

## 结构
* Web框架：主要包括RequestHandler用于创建Web应用程序和各种支持类的子类
* HTTP服务器与客户端：主要包括HTTPServer和AsyncHTTPClient
* 异步网络库：主要包括IOLoop和IOStream作为HTTP组件的构建块
* 协程库

## 模块
> Tornado是一个轻量级框架，它的模块不多最重要的模块是web，web模块包含了Tornado大部分主要功能的Web框架，其他模块都是工具性质的，以便让Web模块更加有用。
1. Core Web Framework 核心Web框架
* tornado.web 包括Web框架大部分主要功能，包括RequestHandler和Application类。
* tornado.httpserver一个无阻塞HTTP服务器的实现
* tornado.template模板系统
* tornado.escape HTML、JSON、URLs等编码解码和字符串操作
* tornado.locale国际化支持

2. Asynchronous Networking 异步网络底层模块
* tornado.ioloop 核心IO循环
* tornado.iostream对非阻塞的Socket的简单封装以方便常用读写操作
* tornado.httpclient无阻塞的HTTP服务器实现
* tornado.netutil网络应用的实现主要是TCPServer类

3. Integration With Other Services 系统集成服务
* tornado.auth 使用OpenId和OAuth进行第三方登录
* tornado.databaseMySQL服务端封装
* tornado.platform.twisted在Tornado上运行Twisted实现的代码
* tornado.websocket实现和浏览器的双向通信
* tornado.wsgi其他Python网络框架或服务器的相互操作

4. Utilities 应用模块
* tornado.autoload产生环境中自动检查代码更新
* tornado.gen基于生成器的接口，使用该模块 保证代码异步运行。
* tornado.httputil分析HTTP请求内容
* tornado.options解析终端参数
* tornado.process多进程实现的封装
* tornado.stack_context异步环境中对回调函数上下文保存、异常处理
* tornado.testing单元测试

> Tornado服务器的三个底层核心模块:
* httpserver 服务于web模块的一个简单的HTTP服务器的实现。Tornado的HTTPConnection类用来处理HTTP请求，包括读取HTTP请求头、读取POST传递的数据，调用用户自定义的处理方法，以及把响应数据写给客户端的socket。
* iostream 对非阻塞式的socket的封装以便于常见读写操作。为了在处理请求时实现对socket的异步读写，Tornado实现了IOStream类用来处理socket的异步读写。
* ioloop 核心的I/O循环。Tornado为了实现高并发和高性能，使用了一个IOLoop事件循环来处理socket的读写事件，IOLoop事件循环是基于Linux的epoll模型，可以高效地响应网络事件，这是Tornado高效的基础保证。

## 设计模型
* Web框架：最上层，包括处理器、模板、数据库连接、认证、本地化等Web框架所需功能
* HTTP/HTTPS层：基于HTTP协议实现了HTTP服务器和客户端
* TCP层：实现TCP服务器负责数据传输
* Event层：最底层、处理IO事件

## tornado主要文件说明
* web.py：其中定义了 Application 和 RequestHandler 类，在 app.py 里直接就用到了。Application 是个单例，总揽全局路由，创建服务器负责监听，并把服务器传回来的请求进行转发（__call__）。RequestHandler 是个功能很丰富的类，基本上 web 开发需要的它都具备了，比如redirect，flush，close，header，cookie，render（模板），xsrf，etag等等
* httpserver.py 和 tcpserver.py：这两个文件主要是实现 http 协议，解析 header 和 body， 生成request，回调给 appliaction，一个经典意义上的 http 服务器（written in python）。众所周知，这是个很考究性能的一块（IO），所以它和其它很多块都连接到了一起，比如 IOLoop，IOStream，HTTPConnection 等等。这里 HTTPConnection 是实现了 http 协议的部分，它关注 Connection 嘛，这是 http 才有的。至于监听端口，IO事件，读写缓冲区，建立连接之类都是在它的下层--tcp里需要考虑的，所以，tcpserver 才是和它们打交道的地方，到时候分析起来估计很麻烦
* iostream.py：IO事件的异步处理
* ioloop.py：一个大大的循环，循环里等待事件，然后处理事件
* epoll.py：声明了一下服务器使用 epoll。选择 select/poll/epoll/kqueue 其中的一种作为事件分发模型，是在 tornado 里自动根据操作系统的类型而做的选择

## 安装
```sh
pip install tornado
```

## 官网简单示例
* 写法一 demo.py
```python
import tornado.ioloop
import tornado.web
import tornado.options
# 定义处理类型
class MainHandler(tornado.web.RequestHandler):
    # 添加一个处理get请求方式的方法
    def get(self):
        # 添加响应数据
        self.write("简单示例")

def make_app():
    # 返回一个应用对象
    return tornado.web.Application([
        (r"/", MainHandler),
    ])

if __name__ == "__main__":
    # 开启默认日志
    tornado.options.parse_command_line()
    app = make_app()
    # 绑定一个监听端口
    app.listen(8888)
    # 启动web程序，开始监听端口的连接
    tornado.ioloop.IOLoop.current().start()
```
* 写法二 demo.py
```python
import tornado.ioloop
import tornado.web
import tornado.options
# 定义处理类型
class MainHandler(tornado.web.RequestHandler):
    # 添加一个处理get请求方式的方法
    def get(self):
        # 添加响应数据
        self.write("简单示例")

if __name__ == "__main__":
    # 开启默认日志
    tornado.options.parse_command_line()
    # 创建一个应用对象
    app = tornado.web.Application([(r"/", MainHandler)])
    # 绑定一个监听端口
    app.listen(8888)
    # 启动web程序，开始监听端口的连接
    tornado.ioloop.IOLoop.current().start()
```
* 执行
```python
# 在命令行输入命令执行
python demo.py
# 打开浏览器访问：本地ip+监听端口
# eg:http://127.0.0.1:8888
```

## 问题与解决方案
* 问题一
  *问题描述：* windows系统下安装的tornado,python版本3.8，跑官方示例执行命令后报错如下：
  ```sh
  # 错误日志片段
  raise NotImplementedError
  NotImplementedError
  ```
  *问题原因：* tornado在Windows下和linux下用的方法不同
  *解决方案：* 
     1. 打开本地python安装目录，找到\Lib\site-packages\tornado\platform目录中的asyncio.py文件并打开该文件
     2. 搜索 sys.platform == "win32" ，然后添加一行代码 asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())，具体如下：
     ```python
      # 原代码块
      if sys.platform == "win32" and hasattr(asyncio, "WindowsSelectorEventLoopPolicy"):
          # "Any thread" and "selector" should be orthogonal, but there's not a clean
          # interface for composing policies so pick the right base.
          _BasePolicy = asyncio.WindowsSelectorEventLoopPolicy  # type: ignore
      else:
          _BasePolicy = asyncio.DefaultEventLoopPolicy
     
      # 修改后的代码块
      if sys.platform == "win32" and hasattr(asyncio, "WindowsSelectorEventLoopPolicy"):
          # "Any thread" and "selector" should be orthogonal, but there's not a clean
          # interface for composing policies so pick the right base.
          asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy()) # 添加在这
          _BasePolicy = asyncio.WindowsSelectorEventLoopPolicy  # type: ignore
      else:
          _BasePolicy = asyncio.DefaultEventLoopPolicy      
     ```
     3. 再运行命令执行，没有报错了

## 参考
* [tornado官网](https://www.tornadoweb.org/en/stable/)
* [tornado github](https://github.com/tornadoweb/tornado)

