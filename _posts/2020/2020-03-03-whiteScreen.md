---
layout: post
title: "H5安卓低版本,ios10白屏问题总结"
date: 2020-03-03
description: "移动端白屏问题"
tag: h5
---

#### H5安卓低版本,ios10白屏问题总结

### 背景

在公司App内嵌h5页面的项目中，在Android某些机型会出现h5加载不出来，也就是白屏现象，具体机型魅蓝Note6，操作系统版本5.0及以下
在某些机型的浏览器也会出现白屏问题

### 端上问题反馈

APP中会加载腾讯x5内核，但不一定成功，失败了就走系统内核，有可能是低版本的系统内核有兼容问题
端上控制台输出

```

11-23 11:31:42.822 19201-19201/? I/wvtest: 0

11-23 11:32:33.042 19201-19201/com.tal.usercenter.myapplication I/wvtest: 19

11-23 11:32:33.062 19201-19201/com.tal.usercenter.myapplication I/wvtest: Viewport argument key "viewport-fit" not recognized and ignored.

11-23 11:32:33.122 19201-19201/com.tal.usercenter.myapplication I/wvtest: Uncaught SyntaxError: Use of future reserved word in strict mode

11-23 11:32:33.122 19201-19201/com.tal.usercenter.myapplication I/wvtest: Uncaught SyntaxError: <unknown message reserved_word>

11-23 11:32:33.122 19201-19201/com.tal.usercenter.myapplication I/wvtest: 100

```

### 问题排查

通过端上控制台输出报错，排查到是文件中包含`const，let`关键字，但是低版本浏览器不支持，所以出现白屏现象

并通过打包后的js代码去搜索，果然含有const关键字

默认情况下 `babel-loader` 会忽略所有 `node_modules` 中的文件，所以可能是npm中引入的某些包有问题，没有完全编译成es5

经过排查是`swiper`这个npm包导致

### 解决问题

`vue.config.js`中有一个配置项为`transpileDependencies`，官方解释：默认情况下 babel-loader 会忽略所有 node_modules 中的文件。如
果你想要通过 Babel 显式转译一个依赖，可以在这个选项中列出来

第一版解决方案加入swiper配置   `transpileDependencies: ['swiper']`，经过打包校验，没有生效

通过查找资料，是swiper中引入了其他的npm包，是其他npm包导致的问题，最终把swiper依赖的包通过Babel转义

最终配置：`transpileDependencies: ['swiper', 'dom7', 'ssr-window']`  经过QA测试，问题解决

### 相关资料

https://segmentfault.com/a/1190000016334023
