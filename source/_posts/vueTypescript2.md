---
title: vueTypescript 简单实现2
date: 2020-05-09 16:43:39
categories:
  - Vue-Typescript
tags:
  - vue
  - typescript
---

> **注意**
> 接着“vue typescript 简单实现”的基础上添加 typescript 支持

---

## 新增说明

1、添加 eslint 支持

> eslint@6.5.1、@typescript-eslint/parser、@typescript-eslint/eslint-plugin

2、用来检测和规范 Vue 代码的风格

> eslint-plugin-vue

3、用来做格式化工具配合我们的 ESLint 可以更大的发挥作用

> prettier、eslint-config-prettier、eslint-plugin-prettier

## 依赖插件安装

```sh
npm install eslint@6.5.1 @typescript-eslint/parser @typescript-eslint/eslint-plugin -D
npm install eslint-plugin-vue -D
npm install prettier eslint-config-prettier eslint-plugin-prettier -D
```
