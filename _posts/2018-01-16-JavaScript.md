---
layout: post
title: "JavaScript日常笔记"
date: 2018-01-16
description: "JavaScript笔记"
tag: JavaScript
---   

### javascript的数据类型

`undefined`, `null`, `Boolean`, `String`, `Object`, `Number`,ES6新增 `Symbol`

ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。

### 判断一个对象是否为空对象{}


1. 最常见的思路，for...in...遍历属性，为真则为“非空数组”；否则为“空数组”

        function isEmptyObject(obj){
            for(var attr in ojb){
                return alert('非空对象)
            }
            return alert('空对象')
        }


2. 通过JSON自带的stringify方法来判断

        var obj = {}
        if(JSON.stringify(obj)=='{}'){
            console.log('空')
        }

3. ES6新增方法 Object.keys()

        let obj = {}
        if(Object.keys(obj).length==0){
            alert('空对象')
        }else{
            alert('非空对象')
        }