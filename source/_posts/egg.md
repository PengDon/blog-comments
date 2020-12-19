---
title: egg 环境配置
date: 2020-12-07 11:21:58
categories: node后端框架
tags: 
- egg
---

## **基础环境准备**
* **node &gt;= 12**

```
   [官网地址去下载node](https://nodejs.org/en/)
```

* **npm &gt;= 6**

```
   [官网地址](https://www.npmjs.com/%29%29%29%29\)
```

* **npm切换淘宝源**    npm config set registry [http://registry.npm.taobao.org](http://registry.npm.taobao.org)

* **node环境配置**

`以安装在D盘根目录为例：D:\node，在此目录新建文件夹mkdir node_global 、mkdir node_cache`

`设置npm安装的全局模块所在的路径`

`npm config set prefix “D:\node\node_global”`

`设置npm缓存cache的路径`

`npm config set cache "D:\node\node_cache"`

`设置系统环境变量,“我的电脑”-右键-“属性”-“高级系统设置”-“高级”-“环境变量”`

`进入环境变量对话框，在【系统变量】下新建【NODE_PATH】，输入【D:\node\node_modules】`

`将【用户变量】下的【Path】修改为【D:\node\node_global】`

`查看node版本`

`node -v`

`查看npm版本`

`npm -v`

* #### 换源工具

`用 npm 全局安装 nrm`

`npm install -g nrm`

`查看所有的可用的源`

`nrm ls`

```
     npm -------- https://registry.npmjs.org/ 
     yarn ------- https://registry.yarnpkg.com/
     cnpm ------- http://r.cnpmjs.org/
     taobao ----- https://registry.npm.taobao.org/
     nj --------- https://registry.nodejitsu.com/
     npmMirror -- https://skimdb.npmjs.com/registry/
     edunpm ----- http://registry.enpmjs.org/
```

`添加源`

`nrm add 源的名称  https:// 地址`

`删除某个源`

`nrm del 源的名字`

`切换到某个源`

`nrm use 源的名字`

```
测试源的速度

nrm test
```