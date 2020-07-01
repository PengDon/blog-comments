---
title: Android Studio
date: 2020-06-30 16:34:13
categories: 开发工具
tags:
- 安卓开发
---

## 常见问题
* 问题一
```js
  问题场景：打开模拟器的时候报错：“The emulator process for AVD xxxxx was killed”
  解决方案1：检查是否缺少HAXM
  具体步骤：
     1、打开Android Studio-Tools-SDK Manager-SDK Tools,找到HAXM相关项选中，然后点击下面的Apply按钮
     2、在弹出的框中选择接受，下一步继续安装
  解决方案2：检查是否自定义SDK目录
  具体步骤：
     1、打开Android Studio-Tools-AVD Manager
     2、进入到“Your Virtual Devices”,找到Actions，选中一个需要启动的设备，点击最右边的向下箭头，选择“Show on Disk”
     3、会打开系统盘sdk的默认目录，eg:C:\Users\Administrator\.android
     4、在这个根目录会看到avd文件夹，直接把这个文件夹包括内容一起拷贝到自己自定义的sdk根目录下
     5、然后再次点击启动模拟器
```
* 问题二
```js
  问题场景：需要修改默认jdk或者sdk路径
  解决步骤：
    1、打开Android Studio-file-Other Settings-Default Project Structure...
    2、根据自己需求选择要指定的路径即可
```
* 问题三
```js
  问题场景：打开Plugins无法正常显示搜索插件，
  解决步骤：
     1、打开Android Studio-file-Settings-Appearance & Behavior-System Settings-HTTP Proxy
     2、选择Auto-detect proxy settings,选择Automatic proxy configuration URL:,设置值为：https://plugins.jetbrains.com/ 
     3、保存后再去Plugins，可以看到插件了
```

