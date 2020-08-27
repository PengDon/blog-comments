---
title: charles 记录
date: 2020-08-27 09:51:57
categories:
  - 抓包工具
tags:
  - charles
---

## 下载

- 官方下载地址：https://www.charlesproxy.com/download/

* 在线破解地址：https://tools.zzzmode.com/mytools/charles/

## charles.jar 破解包 使用方式[window]

> 放到 Charles 安装根目录下的 lib 目录替换已有文件。相对路径：\Charles\lib

## 手机抓包

> charles 4.2.7 为例

- 通用步骤

1. 手机 wifi 设置手动代理，输入当前电脑 ip 和 8888 端口
2. 用手机浏览器访问：chls.pro/ssl 下载证书到手机并安装

- 注意事项

1. 手机上证书后缀问题，有的手机需要.pem，有的需要.crt 格式的文件，需要根据具体场景获取对应后缀的证书文件进行安装
1. 证书安装成功后，为什么还是没办法抓包，如果是 iphone 手机，需要到：通用-关于手机-证书信任，去信任安装好的证书

## request 和 response 数据乱码

> 发现 request 和 respone 都是乱码

1. charles 安装根目录有个 Charles.ini 文件，文件内容如下：其中 vmarg.5 为新增

```js
working.directory=.
classpath.1=lib/charles.jar
main.class=com.xk72.charles.gui.MainWithClassLoader
vm.version.min=1.8
vm.location=jre\bin\server\jvm.dll
vmarg.1=-Dsun.java2d.d3d=false
vmarg.2=-Djava.net.preferIPv4Stack=false
vmarg.3=-Djava.net.preferIPv6Addresses=true
vmarg.4=-Djava.library.path=lib
vmarg.5=-Dfile.encoding=UTF-8   // 新增
dde.enabled=true
dde.class=com.xk72.charles.win32.Win32DDEManager
dde.server.name=Charles
dde.topic=System
single.instance=dde
log.level=warning

[ErrorMessages]
java.not.found=A suitable Java installation was not found. Please visit http://java.com/ to install Java.
java.failed=The Java installation is broken. Please uninstall and reinstall Java from http://java.com/

```

2. 如果还是乱码，则需要配置下 ssl 代理，打开 Proxy - SSL Proxying Settings 窗口，启用 Enable SSL Proxying,然后添加对应的代理，比如：host:\* / port:443
