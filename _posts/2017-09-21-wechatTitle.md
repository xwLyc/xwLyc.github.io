---
layout: post
title: "在部分ios微信中使用vue单页面设置标题兼容问题"
date: 2017-09-21
description: "在部分ios微信中使用vue单页面设置标题兼容问题"
tag: vue
---   
因为 vue生成的都是单页面应用，也就是SPA(single-page-application)，所以，只能通过js `document.title = xxx` 去修改标题。

但是问题是，

### 部分ios手机微信中直接使用`document.title = xxx`修改 title 无效


然后在知乎上偶然间看到，微信有两种webView模式，一种是`WKWebView`,另一种是`UIWebView`,测试了下在 UIWebView 下，苹果的标题并没有修改成功。

所以可能原因大致就是因为在微信中`UIWebView`只加载网页标题一次 动态改变是无效的。

这个一般人是不会开启切换 webview 的, 你在微信在中添加好友输入`:switchweb`切换成 WKVWebView 应该就好了。

但是用户肯定不会去调整的，所以我们还是要考虑解决方案。

既然js动态改变不能生效 那为什么不尝试在路由切换完成后 静默加载一个空iframe动态设置title呢?

这也是加载页面的一种方式不是吗?

### 解决方案

效果预览：[效果](../../../demo/wechatTitle/index.html)

utils/setWechatTitle.js 文件

    let setWechatTitle = (title) => {
        document.title = title; 
        //ios系统下通过iframe设置title
        var mobile = navigator.userAgent.toLowerCase();
        if (/iphone|ipad|ipod/.test(mobile)) {
            let iframe = document.createElement('iframe');
            iframe.style.display = "none";
            iframe.src = "//m.baidu.com/favicon.ico";  //图片可以换成任意较小的图片

            let iframeCallback = () => {
                setTimeout(() => {
                    iframe.removeEventListener('load', iframeCallback);
                    document.removeChild(iframe)
                }, 0);
            }

            iframe.addEventListener('load', iframeCallback);
            document.body.appendChild(iframe);
        }
    }

    module.exports = {
        setWechatTitle: setWechatTitle
    }

router/index.js 文件

    import Vue from 'vue'
    import Router from 'vue-router'
    import setTitle from '@/utils/setWechatTitle.js'

    Vue.use(Router)

    let router = new Router({
    routes: [
        {
            path: '/home',
            name: 'home',
            meta:{
                title: '首页'
            },
            component: resolve => require(['@/components/home'], resolve)
        },
        {
            path: '/detail',
            name: 'detail',
            meta:{
                title: '详情页'
            },
            component: resolve => require(['@/components/detail'], resolve)
            },
        {
            path: '/list',
            name: 'list',
            meta:{
                title: '列表页'
            },
            component: resolve => require(['@/components/list'], resolve)
        },
        {
            path: '*',
            redirect: '/home'
        }
    ]
    })
    router.afterEach((route) => {
        console.log(route)
        let title = route.meta.title;
        setTitle.setWechatTitle(title)
    })
    export default router;