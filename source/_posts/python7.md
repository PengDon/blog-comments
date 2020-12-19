---
title: python request
date: 2020-12-07 09:58:45
categories: python
tags: python
---

## request库

> requests是Python中一个第三方库，基于 urllib，采用 Apache2 Licensed 开源协议的 HTTP 库。它比 urllib 更加方便。[requests库的github地址](https://github.com/requests/requests)

1、 安装

```sh
pip install requests
```

2、创建请求

```py
# 导入requests库
import requests
# 创建一个请求，用来获取百度的网页信息
result = requests.get('http://www.baidu.com/')
# POST请求
result = requests.post('http://www.baidu.com/',data={key:value})
# PUT、DELETE、HEAD和OPTIONS
result = requests.put('http://www.baidu.com/',data={key,value})
result = requests.head('http://www.baidu.com/')
result = requests.delete('http://www.baidu.com/')
result = requests.options('http://www.baidu.com/')
```

3、在URL中传递参数

```py
# get请求并传递参数
data = {"name":"zhangsan","age":18}
result = requests.get("https://www.baidu.com/s",params=data)
# 打印一下URL，发现该URL已经被正确编码
print result.url 
# 输出：https://www.baidu.com/s?name=zhangsan&age=18

# 也可以传递一个列表进去
data = {"name":"zhangsan","favorite":["football","basketball"]}
result = requests.get("https://www.baidu.com/s",params=data)
print result.url 
# 输出：https://www.baidu.com/s?name=zhangsan&favorite=football&favorite=basketball

```

3、响应内容

```py
# 每次请求之后都会返回一个对象，我们可以从此对象中获取响应内容
result = requests.get("https://api.github.com/events")
print result.text  
# 输出：[{"id":"6924608641","type":"PushEvent",...}]

# 二进制响应内容
print result.content  
# 输出：b'[{"id":"6924656608","type":"CreateEvent",...}]'

# JSON格式的响应内容，如果解码失败，result.json()将会引发异常
print result.json()     
# 输出：[{"id":"6924608641","type":"PushEvent",...}]

# 获取当前的编码
print result.encoding

# 要检查一个请求是否成功，使用result.raise_for_status()或者result.status_code来检查
```

4、套接字响应

```py
# 在极少数情况下，你希望从服务器中获得是原始套接字响应，你可以通过result.raw来获取。如果你想这样做，确保你设置stream=True在你的初始请求。一旦你这样操作了，你可以这样
result = requests.get("https://api.github.com/events",stream=True)
print result.raw        
# 输出： <urllib3.response.HTTPResponse object at 0x10ce52dd8>
print result.raw.read(10)    
# 输出： b'\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x03'

# 通常情况下，我们使用如下这种模式来保存正在流式传输的内容：
with open("info.txt","wb") as f:
  for item in result.iter_content(chunk_size=128):
    f.write(item)
```

5、自定义头部

```py
#  将自定义请求头添加到请求当中，只需要传递一个字典到headers参数。注意，请求头的值必须是一个字符串，byte类型的字符串或者unicode。虽然允许unicode，但还是避免使用unicode
header = {"user-agent":'my_test/0001'}
result = requests.get("https://api.github.com/events",headers=header)
```

6、复杂的post请求

```py
# 发送一些表单编码数据,只需要将字典传递给data参数
infoDict = {"name":"张三"}
result = requests.post('http://127.0.0.1:5000/test/post',data=infoDict)

# 传递元组数据
tupleInfo = ("name","张三")
result = requests.post('http://127.0.0.1:5000/test/post',data=tupleInfo)

# 发送一些非编码格式的数据，即你发送的是一个string而不是dict，那么数据将会直接发送
import json
infoDict = {"name":"张三"}
result = requests.post('http://127.0.0.1:5000/test/post',data=json.dumps(infoDict))

# 发送一个字典数据，你可以通过它使用json参数，它会自动编码
infoDict = {"name":"张三"}
result = requests.post('http://127.0.0.1:5000/test/post',json=infoDict)

# 注意，如果你传递了data参数或者files，那么json将会被忽略
```

7、post上传文件

```py
with open('info.txt','rb') as f:
  result = requests.post('http://localhost:5000/post',files={"files":f})
```

8、响应状态码

```py
result = requests.get('http://localhost:5000/get')
print result.status_code  # 200

# 当返回200，表示请求执行成功，我们还可以使用如下方法判断请求是否成功，True为成功，False不成功
print result.staatus_code == requests.codes.ok    # True

# 执行一个错误的请求(4XX客户端错误，5XX服务器错误)时，我们可以以下方法来抛出异常进行检测
result = requests.get('http://localhost:5000/get')
print result.status_code      # 404
print result.raise_for_status()   # Traceback (most recent call last): ...

# 但是如果我们的请求是执行成功的，即状态码为200，此时raise_for_status()的值将会是None
```

9、响应头

```py
# 可以使用Python字典来查看服务器的响应头文件
print result.headers    # {'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '2', 'Server': 'Werkzeug/0.12.2 Python/2.7.10', 'Date': 'Sun, 03 Dec 2017 14:15:32 GMT'}
```

10、Cookies

```py
# 如果响应包含了Cookie，你可以这样快速的访问它
result = requests.get('http://localhost:5000/get')
print result.cookies['userName']

# 如果需要将自己的Cookie发送给服务器，你可以使用cookies参数
cookie = {'userName':'zhangsan'}
result = requests.get('http://localhost:5000/get',cookies=cookie)

# RequestCookieJar提供了一个完整的接口，适合在多个域和路径中使用，它将返回一个Cookie，所以它也可以被传入到cookies参数中
c = requests.cookies.RequestsCookieJar()
c.set('userName','zhangsan',domain='http://localhost:5000',path='/get')
result = requests.get('http://localhost:5000/get',cookies=c)

```

11、Session对象

```py
# Session对象允许你在请求中保存某些参数，它将在所有由会话实例创建的请求中保存Cookie，
# 并将使用urllib3连接池。如果你想同一主机发出多个请求，则会重新使用底层的TCP连接，
# 这将使性能显著提高。Session具有主API的所有请求方法
s = requests.Session()
s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
r = s.get('http://httpbin.org/cookies')
print(r.text)
# '{"cookies": {"sessioncookie": "123456789"}}'

# 但是请注意，方法级参数不会保存在请求，即使使用一个session。这个栗子只会发送第一个请求的Cookie，不会发送第二个
result = s.get('http://httpbin.org/cookies', cookies={'from-my': 'browser'})
print(result.text)
# '{"cookies": {"from-my": "browser"}}'
result = s.get('http://httpbin.org/cookies')
print(result.text)
# '{"cookies": {}}'

```

12、请求和响应对象

```py
# 每当你发起一个GET请求，你都在做两件事。首先，构造一个Request将被发送到服务器的对象来请求或查询某个资源。
# 其次，Response一旦从服务器中获得响应，就会生成一个对象。该Response对象包含服务器锁返回的所有信息，
# 并且还包含Request你最初创建的对象。这是一个简单的请求，从维基百科的服务器获取一些非常重要的信息
result = requests.get('http://en.wikipedia.org/wiki/Monty_Python')
# 现在我们需要获取服务器发送给我们的头文件信息
print result.headers
# 如果我们需要获取发送给服务器的头文件信息，我们可以这样：
print result.request.headers

```

13、SSL证书验证

```py
# 请求将验证HTTPS请求的SSL证书，就像Web浏览器一样。默认情况下，启用SSL验证，如果无法验证SSL证书，将会引发SSLError
result = reqests.get('https://kyfw.12306.cn/otn/login/init')
# requests.exceptions.SSLError: ("bad handshake: Error([('SSL routines', 'tls_process_server_certificate', 'certificate verify failed')],)",)

# 为了避免出现这个错误，我们可以将CA证书的CA_BUNDLE文件或目录传递给verify参数里面：
result = reqests.get('https://kyfw.12306.cn/otn/login/init',verify='/path/...')

# 或者使用Session方式存储起来：
s = Session()
s.verify='/path/...'
result = s.get('https://kyfw.12306.cn/otn/login/init')

# 如果将verify参数设置为False，请求也可以忽略SSL证书
result = requests.get('https://kyfw.12306.cn/otn/login/init',verify=False)

```
