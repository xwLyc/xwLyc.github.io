---
layout: post
title: "fastclick偶遇vue-clipboard2或clipboardJS ios无法复制"
date: 2020-03-03
description: "移动端复制失败"
tag: h5
---

#### fastclick偶遇vue-clipboard2或clipboardJS ios无法复制

### 背景

在集团app内的移动端页面，使用vue-clipboard2发现ios端无法复制，改用clipboardJS也不能复制。

### 问题排查

安卓是可以的，但是ios怎么也复制不了。

经过长时间的问题排查以及资料查询，最终发现，是由于使用了`fastclick`，而`fastclick`构造了自定义事件提前触发。

ios无法触发复制，是因为系统剪贴板命令需要原生click触发。


### 解决问题

在使用`fastclick`库的情况，需要给元素添加`classname="needsclick"`，默认让这个元素走原生click。

原理如下，fastclick库源码：

![pic](../../../images/2020/03/01.jpg)

![pic](../../../images/2020/03/02.png)

![pic](../../../images/2020/03/03.png)

后来仔细看了文档，已经明确的写明，自定义事件是无法访问系统粘贴板的。

*A synthetic paste event must not give a script access to data on the real system clipboard. Synthetic cut and copy events must not modify data on the system clipboard.
Synthetic paste events do not have any default action. Even if such an event is dispatched in an editable context, the implementation must not insert any data.*

### 相关资料

fastclick.js 其它坑也需要注意。 [iOS11.3 fastclick.js相关bug](https://www.jianshu.com/p/5b578e656966)
