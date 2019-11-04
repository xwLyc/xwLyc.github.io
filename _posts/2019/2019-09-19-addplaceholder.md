---
layout: post
title: "使用 CSS 伪类为 select 元素添加 placeholder"
date: 2019-09-19
description: "使用 CSS 伪类为 select 元素添加 placeholder"
tag: css
---   

 参考stackoverflow: https://stackoverflow.com/questions/5805059/how-do-i-make-a-placeholder-for-a-select-box

    <style>
    	select:required:invalid {
        	color: gray;
    	}
    	option[value=""][disabled] {
        	display: none;
    	}
    	option {
        	color: black;
    	}
    </style>

    <select required>
    	<option value="" disabled selected>Select something...</option>
        <option value="1">One</option>
        <option value="2">Two</option>
    </select>

总结codepen地址 ：[https://codepen.io/xwLyc/pen/WNeYNgP](https://codepen.io/xwLyc/pen/WNeYNgP?_blank)

#### 原理

根据上面的例子做下基本的解释：

- HTML5 中，针对客户端验证（client-side validation）提供了 required 属性，同时在 CSS 里可以使用 :required (MDN：链接) 配合基本的选择器（selector）来对样式进行声明： `select:required；`
- HTML5 中表单的 required 属性施加在表单[、控件]上，表示这个表单[、控件]的值是必须有的；
- 反之，没有的时候就是非法，此时 CSS 提供了 :invalid 表示非法的伪类，一个 `<select/>` 是必选（:required）的但是没有选择的时候、或者值为空（value=""）属于非法，可以这么来选择：select:required:invalid;
- 上文给出的例子里，第一个 `<option/>` 的 value="", 默认它是 selected （选中）的，配合有 required 属性的 parent `<select/>` 的时，value="" 的当前 `<select/>`，是一个 :invalid （非法）的，配合 CSS 声明，就可以对 `<select/>` 制作一个 faked 的 placeholder 样式；
- 第一个 `<option/>` 中还有一个 disabled 属性，在这里的作用是点开 `<select/>` 的下拉菜单之后也不能选择当这个 `<option/>`；