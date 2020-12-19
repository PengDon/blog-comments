<!--
 * @Author: don
 * @Date: 2020-08-17 11:22:35
 * @LastEditors: don
 * @LastEditTime: 2020-12-07 10:34:36
 * @Description: 
-->
---
title: python知识点
date: 2020-06-18 10:35:45
categories: python
tags: python
---

## Python简介
* &ensp;&ensp;&ensp;&ensp;Python（英国发音：/ˈpaɪθən/美国发音：/ˈpaɪθɑːn/），是一种面向对象的解释型计算机程序设计语言，由荷兰人GuidovanRossum于1989年发明，第一个公开发行版发行于1991年。

* &ensp;&ensp;&ensp;&ensp;Python是纯粹的自由软件，源代码和解释器CPython遵循GPL（GNUGeneralPublicLicense）协议。Python语法简洁清晰，特色之一是强制用空白符（whitespace）作为语句缩进。

## Python特点
1. Python使用C语言开发，但是Python不再有C语言中的指针等复杂的数据类型。
1. Python具有很强的面向对象特性，而且简化了面向对象的实现。它消除了保护类型、抽象类、接口等面向对象的元素。
1. Python代码块使用空格或制表符缩进的方式分隔代码。
1. Python仅有31个保留字，而且没有分号、begin、end等标记。
1. Python是强类型语言，变量创建后会对应一种数据类型，出现在统一表达式中的不同类型的变量需要做类型转换。

## python开发环境搭建
1. python官网：https://www.python.org
1. 到官网下载对应系统配置的安装包，以windows为例：https://www.python.org/downloads/windows/
1. 检查python版本：python -V
1. 检查pip版本： pip -V

## Python应用以及场景
1. 系统编程：提供API（ApplicationProgrammingInterface应用程序编程接口），能方便进行系统维护和管理，Linux下标志性语言之一，是很多系统管理员理想的编程工具。

1. 图形处理：有PIL、Tkinter等图形库支持，能方便进行图形处理。

1. 数学处理：NumPy扩展提供大量与许多标准数学库的接口。

1. 文本处理：python提供的re模块能支持正则表达式，还提供SGML，XML分析模块，许多程序员利用python进行XML程序的开发。

1. 数据库编程：程序员可通过遵循PythonDB-API（数据库应用程序编程接口）规范的模块与MicrosoftSQLServer，Oracle，Sybase，DB2，MySQL、SQLite等数据库通信。python自带有一个Gadfly模块，提供了一个完整的SQL环境。

1. 网络编程：提供丰富的模块支持sockets编程，能方便快速地开发分布式应用程序。很多大规模软件开发计划例如Zope，Mnet及BitTorrent.Google都在广泛地使用它。

1. 多媒体应用：Python的PyOpenGL模块封装了“OpenGL应用程序编程接口”，能进行二维和三维图像处理。PyGame模块可用于编写游戏软件。

1. pymo引擎：PYMO全称为pythonmemoriesoff，是一款运行于SymbianS60V3，Symbian3，S60V5，Symbian3，Android系统上的AVG游戏引擎。因其基于python2.0平台开发，并且适用于创建秋之回忆（memoriesoff）风格的AVG游戏，故命名为PYMO。

1. 黑客编程：python有一个hack的库，内置了你熟悉的或不熟悉的函数，但是缺少成就感。

1. 网络爬虫：也称网络蜘蛛，是大数据行业获取数据的核心工具。没有网络爬虫自动地、不分昼夜地、高智能地在互联网上爬取免费的数据，那些大数据相关的公司恐怕要少四分之三。能够编写网络爬虫的编程语言有不少，但Python绝对是其中的主流之一，其Scripy爬虫框架应用非常广泛。

1. 科学计算：NumPy，SciPy，Matplotlib可以让Python程序员编写科学计算程序。

1. 桌面软件：PyQt、PySide、wxPython、PyGTK是Python快速开发桌面应用程序的利器。

1. 服务器软件（网络软件）：　Python对于各种网络协议的支持很完善，因此经常被用于编写服务器软件、网络爬虫。第三方库Twisted支持异步网络编程和多数标准的网络协议（包含客户端和服务器），并且提供了多种工具，被广泛用于编写高性能的服务器软件。

1. 游戏：很多游戏使用C++编写图形显示等高性能模块，而使用Python或者Lua编写游戏的逻辑、服务器。相较于Python，Lua的功能更简单、体积更小；而Python则支持更多的特性和数据类型。

1. 自动化运维：这几乎是Python应用的自留地，作为运维工程师首选的编程语言，Python在自动化运维方面已经深入人心，比如Saltstack和Ansible都是大名鼎鼎的自动化平台。

1. 云计算：开源云计算解决方案OpenStack就是基于Python开发的，搞云计算的同学都懂的。

1. WEB开发：基于Python的Web开发框架不要太多，比如耳熟能详的Django，还有Tornado，Flask。其中的Python+Django架构，应用范围非常广，开发速度非常快，学习门槛也很低，能够帮助你快速的搭建起可用的WEB服务。

1. 数据分析：　在大量数据的基础上，结合科学计算、机器学习等技术，对数据进行清洗、去重、规格化和针对性的分析是大数据行业的基石。Python是数据分析的主流语言之一。

1. 人工智能：Python在人工智能大范畴领域内的机器学习、神经网络、深度学习等方面都是主流的编程语言，得到广泛的支持和应用。


## Python3.8安装Scrapy爬虫框架

步骤概览：

1、网络安装wheel；**pip install wheel**

2、本地安装lxml；lxml下载地址  
：[https://www.lfd.uci.edu/~gohlke/pythonlibs/\#lxml](https://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml)

找到下载目录输入：pip install lxml-4.5.0-cp38-cp38-win\_amd64.whl

3、本地安装pyWin32；pyWin32下载地址  
：[https://www.lfd.uci.edu/~gohlke/pythonlibs/\#pywin32](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pywin32)

找到下载目录输入：pip install pywin32-227-cp38-cp38-win\_amd64.whl

4、本地安装Twisted；Twisted下载地址  
：[https://www.lfd.uci.edu/~gohlke/pythonlibs/\#twisted](https://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted)

找到下载目录输入：pip install Twisted-19.10.0-cp38-cp38-win\_amd64.whl

5、网络安装Scrapy；pip install scrapy

6、检查Scrap是否安装成功: scrapy


## 参考资料
* [python文档](https://docs.python.org/zh-cn/3/)
* [Python语言规范](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)

## 1.Scrapy {#item-1}

Scrapy是一个为了爬取网站数据，提取结构性数据而编写的应用框架。 可以应用在包括数据挖掘，信息处理或存储历史数据等一系列的程序中。。用这个框架可以轻松爬下来如亚马逊商品信息之类的数据。

![](https://segmentfault.com/img/bVbbEmN?w=2016&h=672 "clipboard.png")

> 项目地址：[https://scrapy.org/](https://scrapy.org/)

## 2.PySpider {#item-2}

pyspider 是一个用python实现的功能强大的网络爬虫系统，能在浏览器界面上进行脚本的编写，功能的调度和爬取结果的实时查看，后端使用常用的数据库进行爬取结果的存储，还能定时设置任务与任务优先级等。

![](https://segmentfault.com/img/bVbbEnu?w=640&h=228 "clipboard.png")

> 项目地址：[https://github.com/binux/pyspider](https://github.com/binux/pyspider)

## 3.Crawley {#item-3}

Crawley可以高速爬取对应网站的内容，支持关系和非关系数据库，数据可以导出为JSON、XML等。

![](https://segmentfault.com/img/bVbbEn7?w=640&h=276 "clipboard.png")

> 项目地址：[http://project.crawley-cloud.com/](http://project.crawley-cloud.com/)

## 4.Portia {#item-4}

Portia是一个开源可视化爬虫工具，可让您在不需要任何编程知识的情况下爬取网站！简单地注释您感兴趣的页面，Portia将创建一个蜘蛛来从类似的页面提取数据。

![](https://segmentfault.com/img/bVbbEof?w=640&h=339 "clipboard.png")

> 项目地址：[https://github.com/scrapinghub/portia](https://github.com/scrapinghub/portia)

## 5.Newspaper {#item-5}

Newspaper可以用来提取新闻、文章和内容分析。使用多线程，支持10多种语言等。

![](https://segmentfault.com/img/bVbbEoy?w=1882&h=614 "clipboard.png")

> 项目地址：[https://github.com/codelucas/newspaper](https://github.com/codelucas/newspaper)

## 6.Beautiful Soup {#item-6}

Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式.Beautiful Soup会帮你节省数小时甚至数天的工作时间。

![](https://segmentfault.com/img/bVbbEpg?w=2104&h=652 "clipboard.png")

> 项目地址：[https://www.crummy.com/software/BeautifulSoup/bs4/doc/](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)

## 7.Grab {#item-7}

Grab是一个用于构建Web刮板的Python框架。借助Grab，您可以构建各种复杂的网页抓取工具，从简单的5行脚本到处理数百万个网页的复杂异步网站抓取工具。Grab提供一个API用于执行网络请求和处理接收到的内容，例如与HTML文档的DOM树进行交互。

![](https://segmentfault.com/img/bVbbEpq?w=640&h=256 "clipboard.png")

> 项目地址：[https://grablab.org/docs/\#grab-spider-user-manual.](https://grablab.org/docs/#grab-spider-user-manual.)[.](http://docs.grablib.org/en/latest/#grab-spider-user-manual.)

## 8.Cola {#item-8}

Cola是一个分布式的爬虫框架，对于用户来说，只需编写几个特定的函数，而无需关注分布式运行的细节。任务会自动分配到多台机器上，整个过程对用户是透明的。

![](https://segmentfault.com/img/bVbbEpw?w=640&h=611 "clipboard.png")

项目地址：[https://github.com/qinxuye/cola](https://github.com/qinxuye/cola)

