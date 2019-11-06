---
layout: post
title: "Vue文本换行问题"
date: 2019-11-06
description: "Vue文本换行问题"
tag: vue css
---   


### 问题背景：

后端返回的字符串带有\n换行符，但Vue将其插值渲染成div内部文本后，文本并不换行，换行符显示为一个空格。

### 目标：

让文本在换行符处换行。

 

### 解决方法：

#### 思路：实现文本换行有两种方法，一是HTML方法，即`<br>`标签；二是CSS方法，即`white-space`属性。

#### 方法1. 使用v-html

   首先，将字符串里的`\n`替换为`<br>`，然后用`v-html`指令渲染字符串为`innerHTML`。

    // JS部分
    this.text = res.data.replace(/\n/g, '<br>')

    // HTML部分
    <div v-html="text"></div>
    
  这种方法比较麻烦，而且存在安全问题，故不推荐使用。

 

#### 方法2. 设置`white-space`属性（推荐）

  将div容器的`white-space`属性设置为`pre-wrap`即可解决问题。

    // CSS部分
    .text-wrapper {
      white-space: pre-wrap;
    }

    // HTML部分
    <div class="text-wrapper">{{text}}</div>

`pre-wrap`值的意思是保留空白并且正常换行。

`white-space`各属性值详见[这里](https://www.w3school.com.cn/cssref/pr_text_white-space.asp)。其实设置为pre即可使换行符发挥作用，但这时文本在div宽度不足时不会自动换行，而是撞破边界延伸到div外部去，所以还得加上wrap。
