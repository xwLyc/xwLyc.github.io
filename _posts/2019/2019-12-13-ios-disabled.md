---
layout: post
title: "ios input disabled 字体颜色修改无效"
date: 2019-12-13
description: "ios input disabled 字体颜色修改无效"
tag: html5 css3 
---   

#### 问题描述

设置 `input:disabled` 的字体颜色，在 ios下无效，颜色很暗。pc端显示正常。

#### 问题原因

`iPhone` `Safari/webview` 环境下 input disabled 的默认样式会有默认样式`opacity`以及隐藏样式`-webkit-text-fill-color`

#### 解决方案

    input:disabled{  
        color:@disabledColor;
        opacity: 1;
        -webkit-text-fill-color: @disabledColor;
    }