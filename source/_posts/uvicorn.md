---
title: uvicorn入门
date: 2020-06-24 10:47:50
categories: python
tags: uvicorn
---

## 简介
uvicorn 是一个基于 asyncio 开发的一个轻量级高效的 web 服务器框架。
uvicorn 设计的初衷是想要实现两个目标：
* 使用 uvloop和 httptools实现一个极速的 asyncio 服务器。
* 实现一个基于 ASGI(异步服务器网关接口)的最小的应用程序接口。
它目前支持 HTTP/1.1，websockets，Pub/Sub 广播，并且可以扩展到其他协议和消息类型。

## 快速入门
python版本要求：3.5+
安装命令：
```sh
pip install uvicorn
```

## 简单实例1
1、 新建demo.py,内容如下：
```python
async def app(scope, receive, send):
    assert scope['type'] == 'http'
    await send({
        'type': 'http.response.start',
        'status': 200,
        'headers': [
            [b'content-type', b'text/plain'],
        ]
    })
    await send({
        'type': 'http.response.body',
        'body': b'this is uvicon demo!',
    })
```
2、执行下面命令,启动服务：
```sh
uvicorn demo:app
```
3、打开浏览器访问：http:127.0.0.1:8000,可查看效果

## 简单实例2
1、 新建demo.py,内容如下：
```python
import uvicorn

async def app(scope, receive, send):
    assert scope['type'] == 'http'
    await send({
        'type': 'http.response.start',
        'status': 200,
        'headers': [
            [b'content-type', b'text/plain'],
        ]
    })
    await send({
        'type': 'http.response.body',
        'body': b'this is uvicon demo!',
    })

if __name__ == "__main__":
    # 这个要根据实际的文件名动态修改，可以指定ip、端口、日志级别
    uvicorn.run("demo:app", host="127.0.0.1", port=5000, log_level="info")
```
2、执行下面命令,启动服务：
```sh
python demo.py
```
3、打开浏览器访问：http:127.0.0.1:5000,可查看效果




## 参考
* [uvicorn](http://www.uvicorn.org)
* [gunicorn](https://gunicorn.org/)