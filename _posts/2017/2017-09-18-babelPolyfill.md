---
layout: post
title: "babel-polyfill"
date: 2017-09-18
description: "babel-polyfill 介绍"
tag: JavaScript
---   


### Polyfill是什么， 为什么他拿给你解决兼容问题 ？

举个例子，有些旧浏览器不支持Number.isNaN方法，Polyfill就可以是这样的：

    if(!Number.isNaN) {
        Number.isNaN = function(num) {
            return(num !== num);
        }
    }

啥意思呢，就是假如浏览器没有`Number.isNaN`方法，那咱们就给它添加上去，所谓Polyfill就是这样解决API的兼容问题的。


有些人就写对应的Polyfill（Polyfill有很多），帮你把这些差异化抹平，不支持的变得支持了（简单来讲，写些代码判断当前浏览器有没有这个功能，没有的话，就写一些支持的补丁代码）。

你只需要把需要的Polyfill引入到你的程序里，就可以了。

比如下面就是对html5各个特性支持的Polyfill,你需要哪个，就引入哪个。

[HTML5 Cross Browser Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills?_blank)

### babel-polyfill 使用方式


[babel-polyfill 官网](http://babeljs.io/docs/usage/polyfill?_blank),

#### 安装

    npm install --save babel-polyfill

因为这是一个polyfill（它将在你的源代码之前运行），我们需要它是一个`dependency`，而不是一个`devDependency` 

(官方建议是需要 --save 安装的，但是我在项目里貌似用 --save-dev 也可以~~ 尽量还是按照官网的来~)

#### Browserify / Node / Webpack中的用法

    1.  require("babel-polyfill");

    2.  import "babel-polyfill";

    3.  module.exports = {

        　　entry: ["babel-polyfill", "./app/js"]

        };

注：第三种方法适用于使用webpack构建的同学，加入到webpack配置文件(webpack.config.js)entry项中