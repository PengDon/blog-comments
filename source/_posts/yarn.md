---
title: yarn
date: 2020-08-27 16:17:02
categories:
  - 包管理器
tags:
  - yarn
---

## 简介

> 快速、可靠、安全的依赖管理工具。

- 官网地址： https://classic.yarnpkg.com/zh-Hans/

## 快速安装

windows 下有以下几种方式：

1. 下载官方安装包安装
1. npm 命令安装： npm install -g yarn
1. Chocolatey 命令安装：choco install yarn

## 常用命令

| 命令名称                                                 | 描述                                                     |
| -------------------------------------------------------- | -------------------------------------------------------- |
| yarn init                                                | 通过交互式会话创建一个 package.json                      |
| npm init -y                                              | 跳过会话，直接通过默认值生成 package.json                |
| yarn add                                                 | 添加一个依赖,会同步更新 package.json 以及 yarn.lock 文件 |
| yarn add <packageName>                                   | 依赖会记录在 package.json 的 dependencies 下 开发环境    |
| yarn add <packageName> --dev                             | 依赖会记录在 package.json 的 devDependencies 下 生产环境 |
| yarn global add <packageName>                            | 全局安装依赖                                             |
| yarn upgrade                                             | 用于更新包到基于规范范围的最新版本                       |
| yarn remove <packageName>                                | 移除一个依赖                                             |
| yarn install                                             | 安装 package.json 中的所有文件                           |
| yarn install --force                                     | 强制下载安装                                             |
| yarn run                                                 | 运行脚本                                                 |
| yarn info <packageName>                                  | 可以用来查看某个模块的最新版本信息                       |
| yarn info <packageName> --json                           | 输出 json 格式                                           |
| yarn info <packageName> readme                           | 输出 README 部分                                         |
| yarn list                                                | 列出项目的所有依赖                                       |
| yarn config list                                         | 显示当前配置                                             |
| yarn config set key value                                | 设置                                                     |
| yarn config get key                                      | 读取值                                                   |
| yarn config delete key                                   | 删除                                                     |
| yarn config set registry https://registry.npm.taobao.org | 设置淘宝镜像                                             |
| yarn cache list                                          | 列出已缓存的每个包                                       |
| yarn cache dir                                           | 返回 全局缓存位置                                        |
| yarn cache clean                                         | 清除缓存                                                 |
