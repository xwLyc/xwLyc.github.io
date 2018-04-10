---
layout: post
title: "函数式编程以及高阶函数"
date: 2018-04-15
description: "函数式编程以及高阶函数"
tag: JavaScript
---   

最近开始每周一次技术分享会，这周轮到我，我的分享会主题是《函数式编程以及高阶函数》，看了很多文章，花了周末两天时间整理出来，主要是整理思路以及PPT确实很费时间，还是比较喜欢自己 整理博客或者自由总结东西~~~ 

现在开始简单介绍一下函数式编程，适合新手理解。下面开始进入正题：

### 函数式编程

#### 首先我们来了解一下什么是函数式编程？

在函数式编程语言中，函数是第一类的对象，也就是说，函数 不依赖于任何其他的对象而可以独立存在，而在面向对象的语言中，函数 ( 方法 ) 是依附于对象的，属于对象的一部分。这一点就 决定了函数在函数式语言中的一些特别的性质，比如作为传出 / 传入参数，作为一个普通的变量等。

￥%……&*？ 这说的是啥，没懂，没关系，我也找不到一个统一的函数式编程的定义。

然后看了很多，我总结了下，自己眼中的函数是编程思想。

    基于函数的编程方式，使用不可变值和纯粹的函数，对一个值(集合)进行处理，映射成另一个值（集合）

引用@nameoverflow的这句话：

> 函数式编程关心数据的映射，命令式编程关心解决问题的步骤

然后下面开始解析一下函数式编程的一些特性：

### 函数是一等公民

“一等”这个术语通常用来描述值。当函数被看作“一等公民”时，那它就可以去任何值可以去的地方，很少有限制。比如数字在 js里就是一等公民，作为一等公民的函数就会拥有类似数字的性质

    var abc = function(){return 42} // 函数与数字一样可以存储为变量

    var abc = [32, function(){return 42}] // 函数与数字一样可以存储为数组的一个元素

    var acb = {number: 32, fun: function(){return 42}} // 函数与数字一样可以作为对象的成员变量

    32 + (function(){return 42}) () // 函数与数字一样可以在使用时直接创建出来

    // 函数与数字一样可以被传递给另一个函数
    function weirdAdd(n, f){ return n + f()}
    weirdAdd(32, function(){return 42})

    // 函数与数字一样可以被另一个函数返回
    return 32;
    return function(){return 42}

### 不可变

> “可变和共享”是万恶之源

不可变数据其实是函数式编程相关的重要概念。相对的，函数式编程中认为可变性是万恶之源。但是，为什么会有这样的结论呢？
这个问题可能很多程序员都会有。其实，如果你的代码逻辑可变，不要惊慌，这并不是“政治错误”的。比如JS中的数组操作，很对都会对原数组进行直接改变，这当然并没有什么问题。比如：

    let arr = [1, 2, 3, 4, 5];
    arr.splice(1, 1); // 返回[2];
    console.log(arr); // [1, 3, 4, 5];

这是我们常用的“删除数组某一项”的操作。好吧，他一点问题也没有。
问题其实出现在“滥用”可变性上，这样会给你的程序带来“副作用”。先不必关心什么是“副作用”，他又是一个函数式编程的概念。
我们先看一下实例：

    const student1 = {
        school: 'XDF',
        name: 'LYC',
    }

    const changeStudent = (student, newName) => {
        const newStudent = student;
        newStudent.name = newName;
        return newStudent;
    }

    const student2 = changeStudent(student1, 'lyc');

    // both students will have the name properties
    console.log(student1, student2);
    // Object {school: "XDF", name: "lyc"} 
    // Object {school: "XDF", name: "lyc"}

我们发现，尽管创建了一个新的对象student2，但是老的对象student1也被改动了。这是因为JS对象中的赋值是“引用赋值”，即在赋值过程中，传递的是在内存中的引用(memory reference)。

> 不可变数据使代码更简单和安全。

### 纯函数

    对于相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用，也不依赖外部环境的状态

> 总结为两点：
>    1. 函数的返回结果只依赖于它的参数
>    2. 函数执行过程里面没有副作用

#### 纯函数的第一个条件：一个函数的返回结果只依赖于它的参数。

    const a = 1
    const foo = (b) => a + b
    foo(2) // => 3

foo 函数不是一个纯函数，因为它返回的结果依赖于外部变量 a，我们在不知道 a 的值的情况下，并不能保证 foo(2) 的返回值是 3。虽然 foo 函数的代码实现并没有变化，传入的参数也没有变化，但它的返回值却是不可预料的，现在 foo(2) 是 3，可能过了一会就是 4 了，因为 a 可能发生了变化变成了 2。

    const a = 1
    const foo = (x, b) => x + b
    foo(1, 2) // => 3

现在 foo 的返回结果只依赖于它的参数 x 和 b，foo(1, 2) 永远是 3。今天是 3，明天也是 3，在服务器跑是 3，在客户端跑也 3，不管你外部发生了什么变化，foo(1, 2) 永远是 3。只要 foo 代码不改变，你传入的参数是确定的，那么 foo(1, 2) 的值永远是可预料的。


#### 纯函数的第二个条件: 函数执行过程没有副作用

> 一个函数执行过程对产生了外部可观察的变化那么就说这个函数是有副作用的

我们稍微修改下foo:

    const a = 1
    const foo = (obj, b) => {
        return obj.x + b
    }
    const counter = { x: 1 }
    foo(counter, 2) // => 3
    counter.x // => 1

我们把原来的 x 换成了 obj，我现在可以往里面传一个对象进行计算，计算的过程里面并不会对传入的对象进行修改，计算前后的 counter 不会发生任何变化，计算前是 1，计算后也是 1，它现在是纯的。但是我再稍微修改一下它：

    const a = 1
    const foo = (obj, b) => {
        obj.x = 2
        return obj.x + b
    }
    const counter = { x: 1 }
    foo(counter, 2) // => 4
    counter.x // => 2

现在情况发生了变化，我在 foo 内部加了一句 obj.x = 2，计算前 counter.x 是 1，但是计算以后 counter.x 是 2。foo 函数的执行对外部的 counter 产生了影响，它产生了副作用，因为它修改了外部传进来的对象，现在它是不纯的

但是你在函数内部构建的变量，然后进行数据的修改没有副作用：

    const foo = (b) => {
        const obj = { x: 1 }
        obj.x = 2
        return obj.x + b
    }

虽然 foo 函数内部修改了 obj，但是 obj 是内部变量，外部程序根本观察不到，修改 obj 并不会产生外部可观察的变化，这个函数是没有副作用的，因此它是一个纯函数。


#### 纯函数总结

> 一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数。

为什么要煞费苦心地构建纯函数？因为纯函数非常“靠谱”，执行一个纯函数你不用担心它会干什么坏事，它不会产生不可预料的行为，也不会对外部产生影响。不管何时何地，你给它什么它就会乖乖地吐出什么。如果你的应用程序大多数函数都是由纯函数组成，那么你的程序测试、调试起来会非常方便。


### 柯里化 Curry

> 传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

比如对于加法函数，`var add = (x, y) =>　x + y `我们可以这样进行柯里化：



    //比较容易读懂的ES5写法
    var add = function(x){
        return function(y){
            return x + y
        }
    }

    //ES6写法，也是比较正统的函数式写法
    var add = x => (y => x + y);

    //试试看
    var add2 = add(2);
    var add200 = add(200);

    add2(2); // =>4
    add200(50); // =>250

事实上柯里化是一种“预加载”函数的方法，通过传递较少的参数，得到一个已经记住了这些参数的新函数，某种意义上讲，这是一种对参数的“缓存”，是一种非常高效的编写函数的方法：

    import { curry } from 'lodash';

    //首先柯里化两个纯函数
    var match = curry((reg, str) => str.match(reg));
    var filter = curry((f, arr) => arr.filter(f));

    //判断字符串里有没有空格
    var haveSpace = match(/\s+/g);

    haveSpace("ffffffff");
    //=>null

    haveSpace("a b");
    //=>[" "]

    filter(haveSpace, ["abcdefg", "Hello World"]);
    //=>["Hello world"]


### 函数组合

学会了使用纯函数以及如何把它柯里化之后，我们会很容易写出这样的“包菜式”代码：

    h(g(f(x)));

虽然这也是函数式的代码，但它依然存在某种意义上的“不优雅”。为了解决函数嵌套的问题，我们需要用到“函数组合”：

    //两个函数的组合
    var compose = function(f, g) {
        return function(x) {
            return f(g(x));
        };
    };

    //或者
    var compose = (f, g) => (x => f(g(x)));

    var add1 = x => x + 1;
    var mul5 = x => x * 5;

    compose(mul5, add1)(2);
    // =>15 


我们定义的compose就像双面胶一样，可以把任何两个纯函数结合到一起。当然你也可以扩展出组合三个函数的“三面胶”，甚至“四面胶”“N面胶”。

这种灵活的组合可以让我们像拼积木一样来组合函数式的代码：


    var first = arr => arr[0];
    var reverse = arr => arr.reverse();

    var last = compose(first, reverse);

    last([1,2,3,4,5]);
    // =>5

任何代码都是要有实际用处才有意义，对于JS来说也是如此。然而现实的编程世界显然不如范例中的函数式世界那么美好，实际应用中的JS是要接触到ajax、DOM操作，NodeJS环境中读写文件、网络操作这些对于外部环境强依赖，有明显副作用的“很脏”的工作。
这对于函数式编程来说也是很大的挑战，所以我们也需要更强大的技术去解决这些“脏问题”。


### 高阶函数

> 可以把函数作为参数，或者是可以将函数作为返回值的函数

    总结成两点：
        1.函数可以作为参数
        2.函数可以作为返回值

一个最简单的高阶函数：

    function add(x, y, f) {
        return f(x) + f(y);
    }
    add(-5, 6, Math.abs) //11

#### 1.map()

> map() 按照原始数组元素顺序根据调用的函数依次处理元素，返回一个新数组。

现在我想实现对一个数组里面的值进行平方处理，命令行代码实现如下：

    var f = x => x * x;
    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    var result = [];
    for (var i=0; i<arr.length; i++) {
        result.push(f(arr[i]));
    }

由于map()方法定义在JavaScript的Array中，我们调用Array的map()方法，传入我们自己的函数，就得到了一个新的Array作为结果：


    var pow = x => x * x;
    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];

    arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]


所以，map()作为高阶函数，事实上它把运算规则抽象了，因此，我们不但可以计算简单的f(x)=x2，还可以计算任意复杂的函数，比如，把Array的所有数字转为字符串：

    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']

请把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：['adam', 'LISA', 'barT']，输出：['Adam', 'Lisa', 'Bart']。

    var convertStr = (str) => {
        var firstLetter = str.substring(0, 1).toUpperCase(); //首字母转大写
        var otherLetters = str.substring(1).toLowerCase(); //剩余字母转小写
        return firstLetter.concat(otherLetters);
    }
    arr.map(convertStr);

#### 2.reduce()

>reduce把一个函数作用在一个序列上[x1, x2, x3, ...] ，这个函数必须接受两个参数，reduce把结果继续和序列的下一个元素 做  累积计算。

其效果是 rudece(f,[x1,x2,x3,x4]) =  f( f( f(x1,x2) x3) x4)

比方说对一个Array求和，就可以用reduce实现：

    var arr = [1, 3, 5, 7, 9];
    arr.reduce((x, y) => {
        return x + y;
    }); // 25

再比如，将序列[1,3,5,7,9]变为 13579

    var fn = (x, y) => {
        return 10*x + y;
    }
    arr.reduce(fn)

#### 3.filter()

>接收一个判断每个元素是否符合过滤条件的函数，从而过滤出符合条件的元素.

和map()类似，Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是舍弃该元素。
例如，在一个Array中，删掉偶数，只保留奇数，可以这么写：

    var arr = [1, 2, 4, 5, 6, 9, 10, 15];
    var list = arr.filter( x => {
        return x % 2 !== 0;
    });
    list; // [1, 5, 9, 15]

### 函数式编程总结

    语义更加清晰
    可复用性更高
    可维护性更好
    作用域局限，副作用少
