---
layout: post
title: "css3 flex布局 justify-content:space-between 最后一行左对齐"
date: 2019-12-25
description: "css3 flex布局 justify-content:space-between 最后一行左对齐"
tag: css3
---   

在使用`justify-content:space-between`布局时，针对最后一行元素使用 `justify-self: start;`没有效果，查了下css3 flexbox 还未支持。

#### 那么如何实现最后一行左对齐呢？

现有的几个方案

- 使用标签元素补全缺的item
- 使用grid
- 使用伪类

伪类的情况，如果最后一个元素是满的，会有占位，grid会有兼容问题，又不想新增标签。

#### 每一行固定列数的情况实现左对齐方案

由于每一列的数目都是固定的，因此，我们可以计算出最后一个元素的`margin-right`值保证完全左对齐。

假设每一行只有3列元素，那么当最后一个元素是第二列（即`li:last-child:nth-child(3n + 2)`）的情况，才需要进行 `margin-right`处理，距离是一个元素的宽度+空隙宽度。

假设元素宽度是`$width`，上述情况所需要的距离：`(100% -  3 * $width) / 2 + $width ` => `(100% - $width) / 2`


    .list1  li:last-child:nth-child(3n + 2) {
      margin-right: calc((100% - $width) / 2);
    }

![image.png](https://image-static.segmentfault.com/119/227/1192276822-5e031bc01602d_articlex)

同理，一行4列的情况，需要处理两种情况，最后一个元素在第二列 和 最后一个元素在第三列的情况。

    .list2  li:last-child:nth-child(4n + 2) {
      margin-right: calc((100% - $width) / 3 * 2);
    }
    .list2  li:last-child:nth-child(4n + 3) {
      margin-right: calc((100% - $width) / 3 * 1);
    }
    
![Kapture 2019-12-25 at 16.33.11.gif](https://image-static.segmentfault.com/350/478/3504784578-5e031ee82f248_articlex)

**点击这里查看demo [展示代码](https://codesandbox.io/s/flex-lastcow-align-left-omqvi?fontsize=14&hidenavigation=1&theme=dark)**

#### 每一行不固定列数的情况实现左对齐方案

这个我觉得最好的方案还是使用grid了，网上一堆，就不做讨论啦。