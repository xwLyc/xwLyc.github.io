---
layout: post
title: "引入swiper出现not scoped to the top-level :root element 警告"
date: 2020-11-30
description: "引入swiper出现not scoped to the top-level :root element 警告"
tag: h5
---

### 问题

项目中引入swiper运行后发现控制台出现很多类似于下面的警告

     warning  in ./node_modules/swiper/css/swiper.css

    Module Warning (from ./node_modules/postcss-loader/src/index.js):
    Warning

    (429:3) Custom property ignored: not scoped to the top-level :root element (.swiper-lazy-preloader-black { ... --swiper-preloader-color: ... })

     @ ./node_modules/swiper/css/swiper.css 4:14-135 14:3-18:5 15:22-143
     @ ./node_modules/cache-loader/dist/cjs.js??ref--12-0!./node_modules/babel-loader/lib!./node_modules/cache-loader/dist/cjs.js??ref--0-0!./node_modules/vue-loader/lib??vue-loader-options!./src/views/payment/pay.vue?vue&type=script&lang=js&
     @ ./src/views/payment/pay.vue?vue&type=script&lang=js&
     @ ./src/views/payment/pay.vue
     @ ./src/router/payment/index.js
     @ ./src/router sync index\.js$
     @ ./src/router/index.js
     @ ./src/main.js
     @ multi (webpack)-dev-server/client?http://10.26.155.86:8080/sockjs-node (webpack)/hot/dev-server.js ./src/main.js

从警告中可以看出跟项目里使用了postcss-loader有关系，在github issue最终找到一个解决方案。

### 解决方案：

找到项目 `postcss.config.js`文件，忽略`Custom property`即可。

```
module.exports = {
  plugins: {
    'postcss-cssnext': {
      features: {
        customProperties: false
      }
    },
  }
};
```
