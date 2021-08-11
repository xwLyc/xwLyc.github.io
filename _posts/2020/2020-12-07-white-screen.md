---
layout: post
title: "Vue.js 使用 Swiper 在 iOS < 11 时出现错误"
date: 2020-12-07
description: "Vue.js 使用 Swiper 在 iOS < 11 时出现错误"
tag: h5
---

### 背景

花呗一期上线后，一个ios极低系统版本用户（6p ios10系统）反馈点击购买时，没有反应，无法跳转到付款页面，重启手机也不行。

### 事件起因
花呗分期使用了`swiper`插件实现滑动选择卡片效果，而`swiper`引入包（`dom7,ssr-widow`）没有去做`babel`处理，就会造成 c`onst , let `等变量在低版本浏览器不支持的情况，直接js异常。

### 解决方案：

对dom7 ssr-window 进行babel处理.

`Vue CLI 2.x` 下，在 `build/webpack.base.config.js` 文件中修改

    // ...
    modules: {
        rules: [
        // ...
        {
            test: /\.js$/,
            loader: 'babel-loader',
            include: [
                resolve('src'), 
                resolve('test'),
                resolve('node_modules/swiper/dist/js/'),
                resolve('node_modules/webpack-dev-server/client'),
                // 新增
                resolve('node_modules/swiper'),
                resolve('node_modules/dom7'),
                resolve('node_modules/ssr-window')
            ]
          },
        // ...
        ]
    }
// ...

Vue CLI 3.x 下

在 `vue.config.js `中增加 `transpileDependencies` 配置

    module.exports = {
        transpileDependencies: [
            "swiper",
            "dom7",
            "ssr-window"
        ]
    }


### 如何避免：

1. **技术评审阶段把控：**加强移动端兼容性警惕意识，在使用插件或者新属性时，一定要先看下兼容性支持问题。

2. **CR阶段复检：**cr还是要严谨一些，多留意插件引入，条件判断等关键地方。

3. **构建阶段：**对构建的产物进行关键字const,let等字段检查，避免再次出现同类问题。

4. **线上监控报警细化和用户提示：**支付涉及到关键利益，不能正常支付时，需要给用户反馈提示以及进行arms上报处理。

5. **测试尽量兼顾低版本手机：**整体设备list，最低最高版本























