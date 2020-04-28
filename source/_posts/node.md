---
title: node开发环境配置
date: 2020-04-28 14:57:46
categories: 
- Node
tags: 
- Node
- Npm
---

## 前置条件
1、开发软件安装目录 d:\dev

## node下载与安装
1、node官方下载地址：https://nodejs.org/en/download/
2、安装node时选择自定义目录，例如：d:\dev\node

## 系统环境变量配置(win10)
1、修改当前用户下的系统变量path，添加node目录：d:\dev\node
2、在命令窗口执行node -v、npm -v,查看安装是否成功

## node配置
1、在d:\dev\node目录下新建两个文件夹，分别是node_global(全局包下载存放目录)和node_cache(node缓存)
2、执行命令，更改配置
```sh
npm config set prefix "d:\dev\node\node_global"
npm config set cache "d:\dev\node\node_cache"
```
3、查看配置文件内容
```sh
# 用命令查看
npm config ls
```
4、修改当前用户下的系统变量path，添加d:\dev\node\node_global
5、在当前用户下新增系统变量，变量名：NODE_PATH 变量值：d:\dev\node\node_global\node_modules

## 切换到淘宝镜像
```sh
npm config set registry https://registry.npm.taobao.org
```
