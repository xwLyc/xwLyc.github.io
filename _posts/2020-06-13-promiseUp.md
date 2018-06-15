---
layout: post
title: "promise用法进阶以及async await用法初探"
date: 2018-06-13
description: "promise用法进阶 以及 async await用法初探"
tag: Javascript
---   

最近在写小程序，用promise封装了一些小程序内部方法的时候，然后Promise then 调用发现还是嵌套写法，感觉封装就没什么意义了，代码如下：

    login() //封装好的小程序登录方法
    getUserInfo() //封装好的小程序获取用户信息方法
    request('/user/insertUser.do', data, 'GET') //封装好的小程序接口请求方法

我需要拿到用户唯一id，就是login()的code值，然后拿到用户头像昵称，然后请求接口传给后台，写法如下

    login().then(user => {
        getUserInfo().then(res=>{
                let data = {
                    userId: user.code,
                    avatarUrl: res.userInfo.avatarUrl,
                    nickName: res.userInfo.nickName
                }
                request('/user/insertUser.do', data, 'GET').then(res => {
                    console.log(res)
                })
        });
    }).catch(err => {
        console.log(err)
    })

这样写出的结果又是三层嵌套。

#### 代码优化解决方案

### （1）prmomise.all 

    Promise.all([login(),getUserInfo()]).then(next => {
        console.log(next) //打印结果就是两个数组，所以可以通过next[0],next[1]拿到返回值
        let data = {
            userId: next[0].code,
            avatarUrl: next[1].userInfo.avatarUrl,
            nickName: next[1].userInfo.nickName
        }
        request('/user/insertUser.do', data, 'GET').then(res => {
            console.log(res)
        })

    })

### (2) async await 

不得不感叹 async await 在异步处理方面确实很强大，代码书写方式却是同步的。

    async function p() {
        let user = await login();
        let res = await getUserInfo();
        let data = {
            userId: user.code,
            avatarUrl: res.userInfo.avatarUrl,
            nickName: res.userInfo.nickName
        }
        return await request('/user/insertUser.do', data, 'GET');
    }

    p().then(result => {
        console.log(result);
    });

### （3）promise.all 和 async await 混合

    async function p() {
        let [res1, res2] = await Promise.all([login(), getUserInfo()])
        let data = {
            userId: res1.code,
            avatarUrl: res2.userInfo.avatarUrl,
            nickName: res2.userInfo.nickName
        }
        return await request('/user/insertUser.do', data, 'GET');
    }

    p().then(result => {
        console.log(result);
    });


666 操作很6~