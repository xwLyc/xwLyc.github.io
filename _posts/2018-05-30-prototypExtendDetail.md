---
layout: post
title: "原型链继承的缺点（修改原型属性的解释说明）"
date: 2018-05-30
description: "原型链继承的缺点"
tag: Javascript
---   

在重温js原型链继承的缺点的时候，发现一个疑点。

#### 原型链继承的缺点:

1. 在于对原型中引用类型值的误修改。（注意：是引用类型值）

2. 原型链不能实现子类向父类中传参。

看下例子：

    //父类：人
    function Person () {
        this.head = '脑袋瓜子';
        this.emotion = ['喜', '怒', '哀', '乐']; //人都有喜怒哀乐
    }
    //子类：学生，继承了“人”这个类
    function Student(studentID) {
        this.studentID = studentID;
    }
    Student.prototype = new Person();

    var stu1 = new Student(1001);
    console.log(stu1.head); //脑袋瓜子
    console.log(stu1.emotion); //['喜', '怒', '哀', '乐']


    stu1.head = '聪明的脑袋瓜子';
    console.log(stu1.head); //聪明的脑袋瓜子
    stu1.emotion.push('愁');
    console.log(stu1.emotion); //["喜", "怒", "哀", "乐", "愁"]
    
    var stu2 = new Student(1002);
    console.log(stu2.head); //脑袋瓜子
    console.log(stu2.emotion); //["喜", "怒", "哀", "乐", "愁"]

通过例子可以看出, stu1实例修改了`head`属性以及`emotion`属性(对象)值，stu2实例中`head`并没有发生变化，但是`emotion`的值也跟着发生了变化，之前理解的是只要stu1实例修改了属性值，stu2的属性也会发生变化，看来不是这样的~~~ 理解的还是不够透彻。

看下 stu1 和 stu2 实例打印效果

![img5](../../../images/2018/05/5.png)

可以看出当实例中存在和原型对象上同名的属性时，会自动屏蔽原型对象上的同名属性。

`stu1.head = "聪明的脑袋瓜子"` 实际上只是给 stu1 添加了一个本地属性 `head` 并设置了相关值。所以当我们打印 stu1.head 时，访问的是该实例的本地属性，而不是其原型对象上的 `head` 属性（它因和本地属性名同名已经被屏蔽了）。

但是，原型对象上引用类型的值可以通过实例进行修改，致使所有实例共享着的该引用类型的值也会随之改变。

`stu1.emotion.push('愁')` 可以看出，stu1 和 stu2 的实例都变 `愁` 了。



这就是单纯的原型链继承的缺点，如果一个实例不小心修改了原型对象上引用类型的值，会导致其它实例也跟着受影响。

因此，我们得出结论，**原型上任何类型的属性值都不会通过实例被重写，但是引用类型的属性值会受到实例的影响而修改。**

那么又有问题来了，什么样的数据类型是引用类型呢？移步这里 [[js进阶] 基本类型 引用类型](../dataStyle/)