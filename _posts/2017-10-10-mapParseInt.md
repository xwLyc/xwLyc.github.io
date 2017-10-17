---
layout: post
title: "小五解密之['1', '2', '3'].map(parseInt) "
date: 2017-10-10
description: "为什么 ['1', '2', '3'].map(parseInt) 返回 [1, NaN, NaN]？"
tag: JavaScript
---   
**[javascript-puzzlers](http://javascript-puzzlers.herokuapp.com?_blank/)** 被称为 javascript 界的专业八级测验，感兴趣的 jser 可以去试试。

自己亲测了下，只对了一半多一点... 路漫漫其修远兮，吾将上下而求索~ js的路还有好远~~~

**进入正题：**

在 javascript 中 `['1', '2', '3'].map(parseInt)` 为何返回不是 [1, 2, 3] 却是 [1, NaN, NaN]？

我们首先回顾一下 `parseInt() `个 `map()` 两个函数的用法：

### parseInt() 函数 

#### **定义**
`parseInt()` 函数可解析一个字符串，并返回一个整数。

#### **语法**
parseInt(string, radix)

| 参数     | 描述           | 
| ------------- | :-------------| 
| string         | 必需。要被解析的字符串。 | 
| radix         | 可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间。 <br>  如果省略该参数或其值为 `0`，则数字将以 10 为基础来解析。 <br> 如果它以 `"0x"` 或 `"0X"` 开头，将以 16 为基数。如果该参数小于 2 或者大于 36，则 `parseInt()` 将返回 `NaN`。 | 

#### **注意**
如果`parseInt(string,radix)`中的, 小于10的`string`数字 大于等于 非零的`radix`也会返回`NaN`，还有其它 NaN情况就控制台打印试试~~

### map() 函数

#### **定义**

The map() method creates a new array with the results of calling a provided function on every element in the calling array.

对数组的每个元素调用定义的回调函数并返回包含结果的新数组。

#### **语法**

arr.map(callback[,thisArg]);

| 参数     | 描述           | 
| ------------- | :-------------| 
| arr         | 必需。一个数组对象。 | 
| callback    | 必需。必需。一个接受**最多**三个参数的函数。对于数组中的每个元素，`map` 方法都会调用 `callback` 函数一次。 <br>这三个参数分别为：<br> `currentValue`  callback的第一个参数，数组中当前被传递的元素。<br> `index`   callback的第二个参数，数组中当前被传递的元素的索引。<br> `array`   callback的第三个参数，调用 map 方法的数组。| 
| thisArg     | 可选。callback 函数里的`this`值 默认是`window`对象 | 

### 下面我们将parseInt重新定义，看一下会返回什么？

    var parseInt = function (string, radix, others) {
        return string + '-' + radix + '-' + others;
    };
    ['1','2','3'].map(parseInt);

返回结果如下： ![a](../../../images/2017/10/a.png)

为什么会得到这个数组?

以数组第一个字符串为例，`1-0-1,2,3`是因为`map`的callback返回参数分别是`currentValue：1`，`index：0`，`array：[1,2,3]`（此处被转义）。其他同上。

因为`parseInt`函数接收2个参数，string，radix，如果`map`函数返回了这两个参数，不管意义是否是所期望的，都会将其作为参数执行。

以`1-0-1,2,3`为例，`map(parseInt)`中，parseInt作为callback函数执行的将是：`parseInt('1',0)`（前面那个数字1自动被转换为字符串1），其他同上。

看下面的执行函数结果:

    parseInt('1', 0);
    parseInt('2', 1);
    parseInt('3', 2);
    注意：这里的 parseInt 是没有被重新定义过的。

所以得到1，NaN，NaN;

### 解决方案

那我们如何正常获取到map返回的整数的值呢？？

**方法一：使 parseInt基数 默认为10。**

    function returnInt(element) {
        return parseInt(element, 10);
    }

    ['1', '2', '3'].map(returnInt); // [1, 2, 3]

**方法二：使用Number函数**

    ['1', '2', '3'].map(Number); // [1, 2, 3]

**方法三：使用箭头函数**

    ['1', '2', '3'].map(str => parseInt(str)); // [1, 2, 3]
