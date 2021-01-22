---
title: vuepress 使用记录
date: 2020-12-29 13:35:47
categories: 静态网站生成器
tags: 
-- vuepress
---

## 快速开始
```sh
# 创建项目目录
mkdir front-doc && cd front-doc
# 初始化包管理
npm init
# 安装vuepress依赖
npm i -g vuepress
# 创建首页并输入内容    
mkdir docs && echo '# home context' > docs/README.md
```
修改package.json文件：
```js
{
  "scripts": {
    "dev": "vuepress dev docs",
    "build": "vuepress build docs"
  }
}
```
启动服务
```sh
npm run dev
```



## 参考
* [vuepress](https://vuepress.vuejs.org/zh/)
* [awesome-vuepress](https://github.com/vuepress/awesome-vuepress)
* [vuepress-theme-hope](https://vuepress-theme.mrhope.site/zh/)
* [vuepress npm](https://www.npmjs.com/package/vuepress)
* []()
