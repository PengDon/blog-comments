---
title: flutter入门
date: 2020-06-30 16:34:13
categories: 混合开发
tags:
- flutter
---

## 前提
* win10 64

## 简介
Flutter 是 Google 开源的 UI 工具包，帮助开发者通过一套代码库高效构建多平台精美应用，支持移动、Web、桌面和嵌入式平台。Flutter 开源、免费，拥有宽松的开源协议，适合商业项目。

## 优势
* 热重载：页面每次改动,不需要手动去刷新,可自动刷新。即支持开发过程中热重载
* 统一的UI：Flutter 提供丰富的内置 UI 组件——Material Design（针对 Android）和Cupertino（适用于 iOS ），不需要担心在众多设备上看起来会有什么不同

## 架构
Flutter的架构主要分成三层:Framework，Engine和Embedder
1、Framework使用dart实现，包括Material Design风格的Widget,Cupertino(针对iOS)风格的Widgets，文本/图片/按钮等基础Widgets，渲染，动画，手势等。此部分的核心代码是:flutter仓库下的flutter package，以及sky_engine仓库下的io,async,ui(dart:ui库提供了Flutter框架和引擎之间的接口)等package
2、Engine使用C++实现，主要包括:Skia,Dart和Text。Skia是开源的二维图形库，C++ 的2D绘图引擎，调用GPU来完成渲染，提供了适用于多种软硬件平台的通用API
3、Embedder是一个嵌入层，即把Flutter嵌入到各个平台上去，这里做的主要工作包括渲染Surface设置,线程设置，以及插件等。从这里可以看出，Flutter的平台相关层很低，平台(如iOS)只是提供一个画布，剩余的所有渲染相关的逻辑都在Flutter内部，这就使得它具有了很好的跨端一致性

## 环境
1、安装flutter SDK，到官网下载最新稳定版（需要翻墙）
2、例如：下载完后得到flutter_windows_1.17.4-stable.zip，解压到D盘根目录，可以看到flutter目录，配置环境变量，在path中添加D:\flutter\bin
3、Flutter的代理设置，用来解决Dart无法正常下载更新的问题。
变量名称 | 变量值
---- | ----
FLUTTER_STORAGE_BASE_URL | https://storage.flutter-io.cn
PUB_HOSTED_URL | https://pub.flutter-io.cn



## 常见问题
* 问题1
```js
  问题描述：
    1、执行flutter doctor报错：“Android license status unknown”
    2、执行flutter doctor --android-licenses报错：“Exception in thread "main" java.lang.NoClassDefFoundError: javax/xml/bind/annotation/XmlSchema”
  解决步骤：
    1、确认当前系统jdk版本，是否是1.8，如果不是需要安装1.8版本的jdk，并且修改对应的设置和系统环境变量
    2、重新打开命令继续执行flutter doctor
```

* 问题2
```js
  问题描述：
    1、执行flutter doctor报错：“ Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses”
  解决步骤：
    1、执行flutter doctor --android-licenses，然后都选择Y,最后重新执行flutter doctor检查
```
* 问题3
```js
  问题描述：
  解决步骤：
```

* 问题4
```js
  问题描述： 执行flutter doctor报错：“Flutter plugin not installed; this adds Flutter specific functionality”
  解决步骤：
```


## 参考
* [flutter 官网-需翻墙访问](https://flutter.dev/)
* [flutter 中文教程](https://www.w3cschool.cn/evilg/)
* [androiddevtools](https://www.androiddevtools.cn/index.html)
* [Android Studio](https://developer.android.google.cn/studio)
* [软件下载](https://en.softonic.com/)
* []()
* []()
* []()


