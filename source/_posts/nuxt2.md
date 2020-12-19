---
title: nuxt+swiper
date: 2020-12-15 14:28:21
categories:
  - nuxt
tags:
  - nuxt
  - swiper
---

## nuxt项目使用swiper，主要考虑ssr场景[Directive]
1、以插件vue-awesome-swiper为例
```sh
# 安装
npm install swiper vue-awesome-swiper --save
```

2、在nuxt.config.js中添加以下代码
```js
// 全局添加css引入
css: [
  {src: "swiper/dist/css/swiper.css"},
]

// 全局添加js引入
plugins: [
  {src: "@/plugins/vue-swiper.js", ssr: false},
]
```

3、插件文件vue-swiper.js内容
```js
import Vue from 'vue'
if (process.browser) {
  const VueAwesomeSwiper = require('vue-awesome-swiper/dist/ssr')
  Vue.use(VueAwesomeSwiper)
}
```

4、Directive基础结构
```vue
<template>
  <div v-swiper:mySwiper="swiperOption" ref="mySwiper">
    <div class="swiper-wrapper">
      <div class="swiper-slide">
        // code
      </div>
    </div>
    <div class="swiper-pagination"></div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        swiperOption: {
          speed: 1500,
          autoplay: {
            delay: 5000
          },
          lazy: {
            loadPrevNext: true
          },
          loop: true,
          // slidesPerView: 4,  // 同时展示几个
          // slidesPerGroup: 4, // 左右切换时，根据数据长度变化
          // navigation: {
          //   nextEl: ".swiper-button-next",
          //   prevEl: ".swiper-button-prev"
          // },
          pagination: {
            el: '.swiper-pagination'
          },
          // ...
        }
      }
    },
    mounted() {
     if (process.isBrowser()) {
      let postSwiper = this.$refs.mySwiper;
      postSwiper.onmouseover = () => {
        this.mySwiper.autoplay.stop();
      };
      postSwiper.onmouseout = () => {
        this.mySwiper.autoplay.start();
      };
    }
   }
  }
</script>
```

## 参考
* [swiper官网](https://www.swiper.com.cn/)
* [vue-awesome-swiper插件GitHub地址](https://github.com/surmon-china/vue-awesome-swiper)
* [Directive Hook Arguments](https://vuejs.org/v2/guide/custom-directive.html#Dynamic-Directive-Arguments)
* [swiper api](https://www.swiper.com.cn/api/index.html)
