---
title: flask 入门
date: 2020-06-23 11:31:19
categories: python
tags: python flask
---

## 前提
python3.8
win10 64
vscode

## 简介
&ensp;&ensp;&ensp;&ensp;Flask 是一个轻量级的基于 Python 的 Web 框架，适合快速开发。封装功能不及Django完善，性能不及Tornado，但是Flask的第三方开源组件比丰富（http://flask.pocoo.org/extensions/），其 WSGI工具箱采用 Werkzeug ，模板引擎则使用 Jinja2,这两个也是 Flask 框架的核心。Flask也被称为 “microframework” ，因为它使用简单的核心，用 extension 增加其他功能。Flask没有默认使用的数据库、窗体验证工具。

## 其他web框架
1. Django：比较“重”的框架，同时也是最出名的Python框架。包含了web开发中常用的功能、组件的框架（ORM、Session、Form、Admin、分页、中间件、信号、缓存、ContenType....），Django是走大而全的方向，最出名的是其全自动化的管理后台：只需要使用起ORM，做简单的对象定义，它就能自动生成数据库结构、以及全功能的管理后台。
1. Tornado：大特性就是异步非阻塞、原生支持WebSocket协议。
1. Bottle：是一个简单高效的遵循WSGI的微型python Web框架。说微型，是因为它只有一个文件，除Python标准库外，它不依赖于任何第三方模块。

## Flask常用扩展包
* Flask-SQLalchemy：操作数据库；
* Flask-script：插入脚本；
* Flask-migrate：管理迁移数据库；
* Flask-Session：Session存储方式指定；
* Flask-WTF：表单；
* Flask-Mail：邮件；
* Flask-Bable：提供国际化和本地化支持，翻译；
* Flask-Login：认证用户状态；
* Flask-OpenID：认证；
* Flask-RESTful：开发REST API的工具；
* Flask-Bootstrap：集成前端Twitter Bootstrap框架；
* Flask-Moment：本地化日期和时间；
* Flask-Admin：简单而可扩展的管理接口的框架

## 安装
```sh
pip install Flask
```

## 简单实例
新建app.py,内容如下：
```python
# 引入Flask模块
from flask import Flask
# 实例化
app = Flask(__name__)
# 设置路由
@app.route('/')
def home():
    return 'welcome home'

if __name__ == '__main__':
   app.run()
```
执行app.py
```sh
python app.py
```
打开浏览器访问：http://127.0.0.1:5000 , 浏览页面上将出现 welcome home

## 修改Flask的配置参数
Flask 程序实例在创建的时候，需要默认传入当前 Flask 程序所指定的包(模块)
```python
app = Flask(__name__)
def __init__(self,import_name,static_path=None,static_url_path=None,static_folder='static',template_folder='templates',instance_path=None,instance_relative_config=False,root_path=None):

# import_name
## Flask程序所在的包(模块)，传__name__就可以
## 其可以决定Flask在访问静态文件时查找的路径

# static_path
## 静态文件访问路径(不推荐使用，使用static_url_path代替)

# static_url_path
## 静态文件访问路径，可以不传，默认为：/ + static_folder

# static_folder
## 静态文件存储的文件夹，可以不传，默认为static

# template_folder
## 模板文件存储的文件夹，可以不传，默认为templates

# 例如：修改程序名称
app = Flask('Custom naming')
```

## 加载配置文件
在 Flask 程序运行的时候，可以给 Flask 设置相关配置，比如：配置 Debug 模式，配置数据库连接地址等等，设置 Flask 配置有以下三种方式：
```python
# 1.从配置对象中加载(常用)
app.config.from_object()
# 2.从配置文件中加载
app.config.from_pyfile()
# 3.从环境变量中加载(不常用)
app.config.from_envvar()
```
* 配置对象加载
```python
# 配置对象，里面定义需要给app添加的一系列配置
class Config(object):
      DEBUG = True
# 创建Flask类的对象，指向程序所在的包的名称
app = Flask(__name__)
# 从配置对象中加载配置
app.config.from_object(Config)
```
* 配置文件加载
创建配置文件config.ini,在配置文件中添加配置
格式eg: DEBUG=True
```python
# 创建Flask类的对象，指向程序所在的包的名称
app = Flask(__name__)
# 从配置文件中加载配置
app.config.from_pyfile('config.ini')
```
* 读取配置
```python
app.config.get()
# 在视图函数中使用
current_app.config.get()
# 注意：Flask应用程序将一些常用的配置设置成了应用程序对象属性，也可以通过属性直接设置/获取某些配置
app.debug = True
```

## 路由定义
1、指定路由地址
```python
# 指定访问路径为 home
@app.route('/home')
def demo():
    return 'this is home'
```

2、给路由传参
```python
# 路由传递参数
@app.route('/shop/<id>')
def shop_detail(id):
    return 'shop %s' % id
```

3、指定请求方式
```python
'''
在 Flask 中，定义一个路由，默认的请求方式为：
  GET
  OPTIONS(自带)
  HEAD(自带)
'''
# 支持GET和POST，并且支持自带的OPTIONS和HEAD
@app.route('/login', methods=['GET', 'POST'])  
def login():
    # 直接从请求中取到请求方式并返回
    return request.method
```

4、正则匹配路由
```python
'''
在 web 开发中，可能会出现限制用户访问规则的场景，那么这个时候就需要用到正则匹配，根据自己的规则去限定请求参数再进行访问
  具体实现步骤为：
    导入转换器基类：在 Flask 中，所有的路由的匹配规则都是使用转换器对象进行记录
    自定义转换器：自定义类继承于转换器基类
    添加转换器到默认的转换器字典中
    使用自定义转换器实现自定义匹配规则
'''
# 1、导入转换器基类
from werkzeug.routing import BaseConverter
# 2、自定义转换器
from flask import Flask, redirect, url_for
from werkzeug.routing import BaseConverter

class RegexConverter(BaseConverter):
    def __init__(self, url_name, *args):
        super().__init__(url_name)
        # 将接受的第1个参数当作匹配规则进行保存
        self.regex = args[0]

class ListConverter(BaseConverter):
    regex = '(\\d+,?)+\\d$'

    def to_python(self, value):
        return value.split(",")

    def to_url(self, value):
        return ",".join(str(i) for i in value)

# 3、添加转换器到默认的转换器字典中，并指定转换器使用时名字为: re
app = Flask(__name__)
# 将自定义转换器添加到转换器字典中，并指定转换器使用时名字为: re
app.url_map.converters["re"] = RegexConverter
# 将自定义转换器添加到转换器字典中，并指定转换器使用时名字为: list
app.url_map.converters["list"] = ListConverter

# 4、使用转换器去实现自定义匹配规则
@app.route("/demo/<re('\d{6}'):use_name>")
def demo1(use_name):
    return "用户名是 %s" % use_name

@app.route("/users/<list:user_list>")
def demo11(user_list):
    return "用户列表为 %s" % user_list


# 系统自带转换器
DEFAULT_CONVERTERS = {
    'default':          UnicodeConverter,
    'string':           UnicodeConverter,
    'any':              AnyConverter,
    'path':             PathConverter,
    'int':              IntegerConverter,
    'float':            FloatConverter,
    'uuid':             UUIDConverter,
}
# 系统自带的转换器具体使用方式在每种转换器的注释代码中有写，请留意每种转换器初始化的参数

'''
自定义转换器其他两个函数实现：
继承于自定义转换器之后，还可以实现 to_python 和 to_url 这两个函数去对匹配参数做进一步处理：
  to_python：
    该函数参数中的 value 值代表匹配到的值，可输出进行查看
    匹配完成之后，对匹配到的参数作最后一步处理再返回，比如：转成 int 类型的值再返回：
'''
class RegexConverter(BaseConverter):
    def __init__(self, url_map, *args):
        super(RegexConverter, self).__init__(url_map)
        # 将接受的第1个参数当作匹配规则进行保存
        self.regex = args[0]

    def to_python(self, value):
        return int(value)   # 在视图函数中可以查看参数的类型，由之前默认的 str 已变成 int 类型的值

'''
to_url:
　　在使用 url_for 去获取视图函数所对应的 url 的时候，会调用此方法对 url_for 后面传入的视图函数参数做进一步处理
　　具体可参见 Flask 的 app.py 中写的示例代码：ListConverter
'''
```

## 简单视图
1、返回json
```python
'''
在使用 Flask 给客户端返回 JSON 数据时，可以直接使用 jsonify 生成一个 JSON 的响应
注：不推荐使用 json.dumps 转成 JSON 字符串直接返回，因为返回的数据要符合 HTTP 协议规范，如果是 JSON 需要指定 content-type:application/json
'''
# 返回JSON
@app.route('/good')
def demo():
    json_dict = {
        "good_id": 1,
        "good_name": "笔记本"
    }
    return jsonify(json_dict)
```
2、重定向
```python
# 1、重定向到指定url地址
@app.route('/login')
def login():
    return redirect('https://xxx.com/')

# 2、重定向到自己写的视图函数
@app.route('login')
def login():
    retutn 'login page'

# 重定向，采用url_for生成login对应的url
@app.route('/buy')
def buy():
    return redirect(url_for('login'))

# 3、重定向到带有参数的视图函数
# 路由传递参数
@app.route('/user/<int:user_id>')
def user_info(user_id):
    return 'hello %d' % user_id

# 重定向，在url_for中传入参数
@app.route('/demo5')
def demo5():
    # 使用 url_for 生成指定视图函数所对应的 url
    return redirect(url_for('user_info', user_id=100))

# 自定义状态码
# 自定义不符合 http 协议的状态码
@app.route('/demo')
def demo():
    return '状态码为 500', 500

```

## 异常捕获
```python
'''
HTTP主动抛出异常
  abort 方法:抛出一个给定状态代码的 HTTPException 或者 指定响应，例如想要用一个页面未找到异常来终止请求，你可以调用 abort(404)。
  参数：code – HTTP的错误状态码
'''
# abort(404)
# 抛出状态码的话，只能抛出 HTTP 协议的错误状态码
abort(500)

'''
捕获错误
  errorhandler 装饰器:注册一个错误处理程序，当程序抛出指定错误状态码的时候，就会调用该装饰器所装饰的方法
  参数：code_or_exception – HTTP的错误状态码或指定异常
'''
# 处理所有500类型的异常
@app.errorhandler(500)
def internal_server_error(e):
    return '服务器搬家了'

# 处理特定的异常项
@app.errorhandler(ZeroDivisionError)
def zero_division_error(e):
    return '除数不能为0'
```

## 钩子函数
```python
'''
在客户端和服务器交互的过程中，有些准备工作或扫尾工作需要处理，比如：
  在请求开始时，建立数据库连接；
  在请求开始时，根据需求进行权限校验；
  在请求结束时，指定数据的交互格式；

为了让每个视图函数避免编写重复功能的代码，Flask提供了通用设施的功能，即请求钩子。
请求钩子是通过装饰器的形式实现，Flask支持如下四种请求钩子：
  before_first_request:在处理第一个请求前执行
  before_request:在每次请求前执行,如果在某修饰的函数中返回了一个响应，视图函数将不再被调用
  after_request:如果没有抛出错误，在每次请求后执行,接受一个参数：视图函数作出的响应,在此函数中可以对响应值在返回之前做最后一步修改处理,需要将参数中的响应在此参数中进行返回
  teardown_request: 在每次请求后执行,接受一个参数：错误信息，如果有相关错误抛出  
'''

from flask import Flask
from flask import abort

app = Flask(__name__)


# 在第一次请求之前调用，可以在此方法内部做一些初始化操作
@app.before_first_request
def before_first_request():
    print("before_first_request")


# 在每一次请求之前调用，这时候已经有请求了，可能在这个方法里面做请求的校验
# 如果请求的校验不成功，可以直接在此方法中进行响应，直接return之后那么就不会执行视图函数
@app.before_request
def before_request():
    print("before_request")
    # if 请求不符合条件:
    #     return "laowang"


# 在执行完视图函数之后会调用，并且会把视图函数所生成的响应传入,可以在此方法中对响应做最后一步统一的处理
@app.after_request
def after_request(response):
    print("after_request")
    response.headers["Content-Type"] = "application/json"
    return response


# 请每一次请求之后都会调用，会接受一个参数，参数是服务器出现的错误信息
@app.teardown_request
def teardown_request(e):
    print("teardown_request")


@app.route('/')
def index():
    return 'index'

if __name__ == '__main__':
    app.run(debug=True)
```

## request请求参数
request 就是flask中代表当前请求的 request 对象，作为请求上下文变量(理解成全局变量，在视图函数中直接使用可以取到当前本次请求)
常用属性：
属性 | 说明	| 类型
--- | --- | ---
data|	记录请求的数据，并转换为字符串|	*
form|	记录请求中的表单数据|	MultiDict
args|	记录请求中的查询参数|	MultiDict
cookies| 记录请求中的cookie信息|	Dict
headers|	记录请求中的报文头|	EnvironHeaders
method|	记录请求使用的HTTP方法|	GET/POST
url|	记录请求的URL地址|	string
files|	记录请求上传的文件|	*

```python
# 获取上传的图片并保存到本地
@app.route('/', methods=['POST'])
def index():
    pic = request.files.get('pic')
    pic.save('./static/aaa.png')
    return 'index'
```

## flask上下文参数
```python
'''
上下文：相当于一个容器，保存了 Flask 程序运行过程中的一些信息。
Flask中有两种上下文，请求上下文(request context)和应用上下文(application context)
  1、application 指的就是当你调用app = Flask(__name__)创建的这个对象app；
  2、request 指的是每次http请求发生时，WSGI server(比如gunicorn)调用Flask.__call__()之后，在Flask对象内部创建的Request对象；
  3、application 表示用于响应WSGI请求的应用本身，request 表示每次http请求；
  4、application的生命周期大于request，一个application存活期间，可能发生多次http请求，所以，也就会有多个request

源码了解一下 flask 如何实现这两种context：　
'''
# 代码摘选自flask 0.5 中的ctx.py文件, 进行了部分删减
class _RequestContext(object):
    
    def __init__(self, app, environ):
        self.app = app
        self.request = app.request_class(environ)
        self.session = app.open_session(self.request)
        self.g = _RequestGlobals()

# flask 使用_RequestContext的代码如下：
class Flask(object):

    def request_context(self, environ):
        return _RequestContext(self, environ)

'''
在Flask类中，每次请求都会调用这个request_context函数。这个函数则会创建一个_RequestContext对象。这个对象在创建时，将Flask实例的本身作为实参传入_RequestContext自身，因此，self.app = Flask()。
所以，虽然每次http请求都会创建一个_RequestContext对象，但是，每次创建的时候都会将同一个Flask对象传入该对象的app成员变量，使得："由同一个Flask对象响应的请求所创建的_RequestContext对象的app成员变量都共享同一个application"
通过在Flask对象中创建_RequestContext对象，并将Flask自身作为参数传入_RequestContext对象的方式，实现了多个request context对应一个application context 的目的。　　
  1、请求上下文(request context)
    请求上下文对象有：request、session
      request
          封装了HTTP请求的内容，针对的是http请求。举例：user = request.args.get('user')，获取的是get请求的参数。
      session
          用来记录请求会话中的信息，针对的是用户信息。举例：session['name'] = user.id，可以记录用户信息。还可以通过session.get('name')获取用户信息。
  2、应用上下文(application context)
  　 应用上下文对象有：current_app，g
  　 a、current_app
  　　应用程序上下文,用于存储应用程序中的变量，可以通过current_app.name打印当前app的名称，也可以在current_app中存储一些变量，例如：
      应用的启动脚本是哪个文件，启动时指定了哪些参数
      加载了哪些配置文件，导入了哪些配置
      连了哪个数据库
      有哪些public的工具类、常量
      应用跑再哪个机器上，IP多少，内存多大
'''
current_app.name
current_app.test_value='value'
'''
    b、g变量
    　　g 作为 flask 程序全局的一个临时变量,充当中间媒介的作用,我们可以通过它传递一些数据，g 保存的是当前请求的全局变量，不同的请求会有不同的全局变量，通过不同的thread id区别
'''
g.name='zhangsan'
'''
请求上下文：保存了客户端和服务器交互的数据
应用上下文：flask 应用程序运行过程中，保存的一些配置信息，比如程序名、数据库连接、应用信息等
'''
```

## cookie使用
```python
'''
Cookie：指某些网站为了辨别用户身份、进行会话跟踪而储存在用户本地的数据（通常经过加密）。
  复数形式Cookies。
  Cookie是由服务器端生成，发送给客户端浏览器，浏览器会将Cookie的key/value保存，下次请求同一网站时就发送该Cookie给服务器（前提是浏览器设置为启用cookie）。
  Cookie的key/value可以由服务器端自己定义。
'''
# 设置cookie
# 当浏览器请求某网站时，会将本网站下所有Cookie信息提交给服务器，所以在request中可以读取Cookie信息

from flask imoprt Flask, make_response
@app.route('/cookie')
def set_cookie():
    resp = make_response('this is to set cookie')
    resp.set_cookie('username', 'zhangsan')
    resp.set_cookie("pwd", "12321")    　
    return resp

# 设置过期时间
@app.route('/cookie')
def set_cookie():
    response = make_response('hello world')
    response.set_cookie('username', 'zhangsan', max_age=3600)  # 过期时间为3600秒
    return response


# 获取cookie
from flask import Flask, request

@app.route('/request')
def resp_cookie():
    resp = request.cookies.get('username')
    return resp

# 删除cookie
@app.route("/logout")
def logout():
    resp = make_response("success")
    resp.delete_cookie("username")
    resp.delete_cookie("pwd")
    return resp
```

## Session使用
```python
# session数据的获取
# session:请求上下文对象，用于处理http请求中的一些数据内容

# 设置secret_key
# 作用：设置一个secret_key值，用作各种 HASH
app.secret_key = 'python'
# 考虑到安全性, 这个密钥不建议存储在程序中. 最好的方法是存储在你的系统环境变量中, 通过 os.getenv(key, default=None) 获得.
    
#设置session，并重定向到获取session的index函数
@app.route("/login")
def login():
    session["username"] = 'zhangsan'
    session['password'] = "1234321"
    return redirect(url_for("index"))

# 获取session
@app.route("/")
def index():
    username = session.get("username")
    password = session.get("password")
    return "世界真美好%s======%s" % (username, password)

# 删除session,并重定向到获取session的index函数
@app.route("/logout")
def logout():
    session.pop("username")
    session.pop("password")
    return redirect(url_for("index"))
```

## 蓝图Blueprint
```python
'''
Blueprint 是一个存储操作方法的容器，这些操作在这个Blueprint 被注册到一个应用之后就可以被调用，Flask 可以通过Blueprint来组织URL以及处理请求。
Flask使用Blueprint让应用实现模块化，在Flask中，Blueprint具有如下属性：
  一个应用可以具有多个Blueprint
  可以将一个Blueprint注册到任何一个未使用的URL下比如 “/”、“/sample”或者子域名
  在一个应用中，一个模块可以注册多次
  Blueprint可以单独具有自己的模板、静态文件或者其它的通用操作方法，它并不是必须要实现应用的视图和函数的
  在一个应用初始化时，就应该要注册需要使用的Blueprint

但是一个Blueprint并不是一个完整的应用，它不能独立于应用运行，而必须要注册到某一个应用中。
蓝图/Blueprint对象用起来和一个应用/Flask对象差不多，最大的区别在于一个 蓝图对象没有办法独立运行，必须将它注册到一个应用对象上才能生效
'''
# 使用蓝图可以分为三个步骤
# 1、创建一个蓝图对象
admin=Blueprint('admin',__name__)
# 2、在这个蓝图对象上进行操作,注册路由,指定静态文件夹,注册模版过滤器
@admin.route('/admin')
def index():
    return 'admin_home'
# 3、在应用对象上注册这个蓝图对象
app.register_blueprint(admin,url
_prefix='/admin')
# 当这个应用启动后,通过/admin/admin/可以访问到蓝图中定义的视图函数

'''
运行机制
    蓝图是保存了一组将来可以在应用对象上执行的操作，注册路由就是一种操作
    当在应用对象上调用 route 装饰器注册路由时,这个操作将修改对象的url_map路由表
    然而，蓝图对象根本没有路由表，当我们在蓝图对象上调用route装饰器注册路由时,它只是在内部的一个延迟操作记录列表defered_functions中添加了一个项
    当执行应用对象的 register_blueprint() 方法时，应用对象将从蓝图对象的 defered_functions 列表中取出每一项，并以自身作为参数执行该匿名函数，即调用应用对象的 add_url_rule() 方法，这将真正的修改应用对象的路由表
蓝图的url前缀
    当我们在应用对象上注册一个蓝图时，可以指定一个url_prefix关键字参数（这个参数默认是/）
    在应用最终的路由表 url_map中，在蓝图上注册的路由URL自动被加上了这个前缀，这个可以保证在多个蓝图中使用相同的URL规则而不会最终引起冲突，只要在注册蓝图时将不同的蓝图挂接到不同的自路径即可
    url_for
'''
url_for('admin.index') # /admin/admin/   将admin下index函数路由注册 

'''
注册静态路由
和应用对象不同，蓝图对象创建时不会默认注册静态目录的路由。需要我们在 创建时指定 static_folder 参数。
下面的示例将蓝图所在目录下的static_admin目录设置为静态目录
'''
admin = Blueprint("admin",__name__,static_folder='static_admin')
app.register_blueprint(admin,url_prefix='/admin')
# 现在就可以使用/admin/static_admin/ 访问static_admin目录下的静态文件了 定制静态目录URL规则 ：可以在创建蓝图对象时使用 static_url_path 来改变静态目录的路由。下面的示例将为 static_admin 文件夹的路由设置为 /lib
admin = Blueprint("admin",__name__,static_folder='static_admin',static_url_path='/lib')
app.register_blueprint(admin,url_prefix='/admin')
# 设置模版目录
# 蓝图对象默认的模板目录为系统的模版目录，可以在创建蓝图对象时使用 template_folder 关键字参数设置模板目录
admin = Blueprint('admin',__name__,template_folder='my_templates')
# 注:如果在 templates 中存在和 my_templates 同名文件,则系统会优先使用 templates 中的文件
```

## Flask-Script 扩展
```python
# 通过使用Flask-Script扩展，我们可以在Flask服务器启动的时候，通过命令行的方式传入参数。而不仅仅通过app.run()方法中传参，比如我们可以通过：
python main.py runserver -host ip地址 -P 端口

# 1 、安装 Flask-Script 扩展
pip install flask-script

# 2、集成 Flask-Script
from flask import Flask
from flask_script import Manager
app = Flask(__name__)

# 把 Manager 类和应用程序实例进行关联
manager = Manager(app)

@app.route('/')
def index():
    return 'hello world'

if __name__ == "__main__":
    manager.run()
```

## 参考
* [Flask中文文档](http://docs.jinkan.org/docs/flask/)
* [Flask英文文档](https://flask-restful.readthedocs.io/en/latest/)
* [扩展列表文档](http://flask.pocoo.org/extensions/)

