---
title: Android Studio
date: 2020-06-30 16:34:13
categories: 开发工具
tags:
- 安卓开发
---
## 前提
* win10 64
* Android Studio 4.0
* jdk1.8
* android-sdk_r24.4.1-windows

## 下载的android sdk目录下的主要文件说明
名称 | 描述
--   | --
AVD Manager.exe | 虚拟机管理工具
SDK Manager.exe | sdk管理工具
add-ons | 保存着附加库，比如GoogleMaps，当然你如果安装了OphoneSDK，这里也会有一些类库在里面
tools目录 | 包括测试、调试、第三方工具。模拟器、数据管理工具等
build-tools目录 | 编译工具目录，包含了转化为davlik虚拟机的编译工具
platform-tools目录 | 包含开发app的平台依赖的开发和调试工具。比如adb、和aapt、aidl、dx等文件
platforms 目录 | 每个平台的SDK真正的文件，里面会根据APILevel划分的SDK版本
system-images目录 | 编译好的系统映像。模拟器可以直接加载
sources目录 | android　sdk的源码目录




## android sdk 配置说明
1、ANDROID_SDK_HOME：安卓sdk的目录，eg:android-sdk_r24.4.1-windows.zip解压出来的文件就是sdk
```js
// eg
ANDROID_SDK_HOME // key
D:\android-sdk-windows // value
```
2、添加到path
```js
%ANDROID_SDK_HOME%\tools
%ANDROID_SDK_HOME%\emulator
%ANDROID_SDK_HOME%\platform-tools
```

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
* 问题四
```js
  问题场景：创建flutter项目时，卡着
  解决步骤：
     1、先确定自己环境没问题
     2、关掉Android Studio，然后用管理员模式打开，再创建flutter项目，发现不会卡着了
```
* 问题五
```js
  问题场景：在创建Android模拟器时间发现提示“No system images installed for this target”问题，无法创建模拟器
  解决步骤：
     1、给SDK中对应的Android版本下载对应的系统镜像，eg:xxxxxx System Image
     2、下载安装好了之后，重新启动模拟器
```
* 问题六
```
  问题场景：当android启动模拟器出现如下错误，CPU acceleration status: Unable to open HAXM device: ERROR_FILE_NOT_FOUND
  解决步骤：
     1、进入 android-sdk-windows\extras\intel\Hardware_Accelerated_Execution_Manager 目录
     2、双击intelhaxm-android.exe安装一下就可以
```


## 参考
* [androiddevtools](https://www.androiddevtools.cn/index.html)
* [Android Studio](https://developer.android.google.cn/studio)
* [sdkmanager](https://developer.android.google.cn/studio/command-line/sdkmanager.html)
