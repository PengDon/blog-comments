---
title: nuxt 全局样式变量
date: 2020-09-14 16:03:32
categories:
  - nuxt
tags:
  - less
  - scss
---

## 前提

- node v12.18.3
- npm v6.14.6
- nuxt ^2.11.0

## less 样式全局变量

1. less 全局变量文件，variables.less ,文件路径：assets/css/variables.less

```css
@body-bg: blue;
@font-size-base: 12px;
@font-color-white: #ffffff;
```

2. 安装相关依赖

```sh
npm i -D less less-loader @nuxtjs/style-resources
```

3. 配置 nuxt.config.js 文件

```js
// 主要配置两个地方
// 1. 配置modules属性
  modules: [
    '@nuxtjs/style-resources',
  ],

// 2. 配置styleResources属性，与modules属性同级
  styleResources: {
    less: ['./assets/css/variables.less'],
  },
```

4. 页面使用

```vue
<template>
  <div class="container">
    <h1 class="title">vue-nuxt</h1>
  </div>
</template>

<script>
export default {};
</script>

<style lang="less">
body {
  background: @body-bg;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: @font-size-base;
  color: @font-color-white;
  letter-spacing: 1px;
}
</style>
```

# scss 样式全局变量

1. scss 全局变量文件，variables.scss ,文件路径：assets/css/variables.scss

```css
$body-bg: blue;
$font-size-base: 12px;
$font-color-white: #ffffff;
```

2. 安装相关依赖

```sh
npm i -D node-sass sass-loader scss-loader @nuxtjs/style-resources
```

3. 配置 nuxt.config.js 文件

```js
// 主要配置两个地方
// 1. 配置modules属性
  modules: [
    '@nuxtjs/style-resources',
  ],

// 2. 配置styleResources属性，与modules属性同级
  styleResources: {
    scss: ['./assets/css/variables.scss'],
  },
```

4. 页面使用

```vue
<template>
  <div class="container">
    <h1 class="title">vue-nuxt</h1>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
body {
  background: $body-bg;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: $font-size-base;
  color: $font-color-white;
  letter-spacing: 1px;
}
```
