---
title: typescript认知
date: 2020-12-17 14:14:37
categories: javascript
tags:
- typescript
---

## 背景
> TypeScript 起源于使用JavaScript开发的大型项目 。由于JavaScript语言本身的局限性，难以胜任和维护大型项目开发。因此微软开发了TypeScript ，使得其能够胜任开发大型项目。

## Typescript 是什么？
> javascript的超集，主要提供了类型系统和对 ES6 的支持，它由 Microsoft 开发，代码开源于 GitHub 上。

## Typescript 优势
1、增加了代码的可读性和可维护性，对面向对象思想进行了增强
* 添加了类型系统
* 在编译阶段就能发现大部分错误
* 增强IDE编辑器功能（代码补全、接口提示、代码重构等）
2、非常包容
* .js 文件可以直接重命名为 .ts 即可
* 即使不显式的定义类型，也能够自动做出类型推论
* 即使 TypeScript 编译报错，也可以生成 JavaScript 文件
* 兼容第三方库，即使第三方库不是用 TypeScript 写的，也可以编写单独的类型文件供 TypeScript 读取
3、拥有活跃的社区

## TypeScript 安装
1、安装命令
```sh
npm install -g typescript
```
2、单个文件编译,例如：
```sh
tsc index.ts
```

## 参考
* [typescript官网](https://www.typescriptlang.org/)
* [typescript中文](https://www.tslang.cn/)
* [typescript资讯](https://devblogs.microsoft.com/typescript/)
* [webpack typescript](https://www.webpackjs.com/guides/typescript/)
* [npm typescript](https://www.npmjs.com/package/typescript)
* [typescript github](https://github.com/Microsoft/TypeScript)
* [vue-property-decorator](https://github.com/kaorun343/vue-property-decorator)