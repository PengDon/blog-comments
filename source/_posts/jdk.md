---
title: jdk 安装配置
date: 2020-08-19 13:41:11
categories: Java
tags: 
- jdk
---

## 前提
* win10 64

## 概念简述 
* jdk(Java Development Kit)：jdk开发工具包主要用来编译调试
* jre(Java Runtime Environment)：jre是java运行环境用于解释执行Java的字节码文件
* jvm(Java Virtual Machine):Java的虚拟机，是JRE的一部分。它是整个java实现跨平台的最核心的部分，负责解释执行字节码文件

## 下载
1. 官网地址：https://www.oracle.com/java/technologies/javase-downloads.html
1. 确定需要下载的版本,比如：考虑到flutter开发，不能安装超过jdk8以上的版本，所以选择“Java SE 8u261”版本下载，点击“JDK DOWNLOAD”进入对应版本下载页面
1. 然后再根据系统环境选择对应下载包，比如：我的是win10 64位，则选择Windows x64对应的包进行下载

注意事项：
1. 安装路径不要有中文或者特殊符号如空格等

## 环境变量配置

变量名称 | 变量值 | 描述说明
------- | ------ | -------
JAVA_HOME | D:\java\jdk1.8.0_6 | jdk的安装目录
CLASSPATH |  .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar | 告诉Java执行环境，在哪些目录下可以找到所要执行的Java程序所需要的类或者包
PATH | %JAVA_HOME%\bin |
PATH | %JAVA_HOME%\jre\bin |

## 检查是否安装成功
1. 打开cmd输入
```sh
java -version
```
1. 如果可以正常显示当前安装的版本，则说明可以了

