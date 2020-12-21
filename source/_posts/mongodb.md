---
title: window系统下载安装配置mongodb
date: 2020-12-21 17:59:03
categories: 数据库
tags:
- mongodb
---

## 前提
* win10

## 简介
> MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。
在高负载的情况下，添加更多的节点，可以保证服务器性能。
MongoDB 旨在为WEB应用提供可扩展的高性能数据存储解决方案。
MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

* [官网地址](https://www.mongodb.com/):https://www.mongodb.com/

## 下载
* [mongodb下载地址](https://www.mongodb.com/try/download/community):https://www.mongodb.com/try/download/community
* [mongodb Compass 图形管理工具](https://www.mongodb.com/try/download/compass?jmp=docs):https://www.mongodb.com/try/download/compass?jmp=docs

## 安装
1、选好对应的版本、平台选windows，package选msi格式的，然后直接点击“Download”
2、直接“next”到安装完成。（或者不想用官方的图形编辑器，则安装时不勾选install mongoDB compass）
3、默认**安装目录**：C:\Program Files\MongoDB\
4、mongodb默认连接端口27017

## 配置
1、创建数据目录，主要是磁盘根目录，eg:d:/mongodb/db 或d:/mongodb/logs
```sh
d:
mkdir mongodb
cd mongodb\
mkdir db
mkdir logs
```
2、mongodb的bin目录下，执行命令告诉mongodb自己要把数据存放到哪里，在命令行输入：
```sh
mongod -dbpath d:\mongodb\db
```
3、在浏览器访问：http://localhost:27017，会看看到：“It looks like you are trying to access MongoDB over HTTP on the native driver port.”
4、在mongodb的bin目录下的同级新建一个文件 mongo.conf
```sh
# 创建文件
touch mongo.conf

# 创建后目录结构

├─bin
├─data
│  ├─diagnostic.data
│  └─journal
├─log
│ 
└─mongo.conf
```
5、mongo.cong内容如下：
```config
dbpath = d:\mongodb\db
logpath = d:\mongodb\logs\mongodb.log
```

6、 在新的命令行cd到mongodb的bin目录下，继而输入命令：
```sh
# 告诉mongodb，配置文件的方法，并将mongodb作为系统服务启动
mongod --config "C:\Program Files\MongoDB\Server\4.4\mongo.conf" --install -serviceName "MongoDB"
```

7、配置系统环境变量，打开系统环境变量把mongodb到bin目录的路径放到path变量对应的值里面。然后重新打开命令行，输入：mongo 命令即可连接




