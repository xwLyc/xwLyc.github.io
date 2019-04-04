---
layout: post
title: "优雅的进行vue-router文件管理"
date: 2018-05-22
description: "高效管理自己的vue-router文件"
tag: Vue
---   
也可以看`segmentDefault`，在这里提问的问题， [传送门](https://segmentfault.com/q/1010000014959830)

一直都习惯在一个router.js 去编写自己的路由功能，但是发现当项目菜单增多(模块增多)，功能变多，router.js 代码也会越来越长，就会变得不是那么容易管理，多人合作也会不是特别一目了然的感觉。
像这种

    const router = new Router({
        routes: [
            {
                path: '/login',
                name: 'login',
                component: resolve => require(['@/components/login'], resolve)
            },
            {
                path: '/index',
                name: 'index',
                component: resolve => require(['@/components/index'], resolve),
                children: [
                    // 课程管理
                    {
                        path: '/lesson',
                        name: 'lesson',
                        component: resolve => require(['@/components/lesson'], resolve),
                    },
                    {
                        path: '/lesson/editLesson',
                        name: 'editLesson',
                        component: resolve => require(['@/components/lesson/editLesson'], resolve)
                    },
                    {
                        path: '/lesson/excelLesson',
                        name: 'excelLesson',
                        component: resolve => require(['@/components/lesson/excelLesson'], resolve)
                    },
                    {
                        path: '/lesson/excelLesson/excelEdit',
                        name: 'excelEdit',
                        component: resolve => require(['@/components/lesson/excelEdit'], resolve)
                    },
                    // 资源管理
                    {
                        path: '/resources',
                        name: 'resources',
                        component: resolve => require(['@/components/resources'], resolve)
                    },
                    // 数据统计
                    {
                        path: '/statistics',
                        name: 'statistics',
                        component: resolve => require(['@/components/statistics'], resolve)
                    },
                    // 活动管理
                    {
                        path: '/activity',
                        name: 'activity',
                        component: resolve => require(['@/components/activity'], resolve)
                    },
                    {
                        path: '/activity/editActivity',
                        name: 'editActivity',
                        component: resolve => require(['@/components/activity/editActivity'], resolve)
                    },
                    {
                        path: '/activity/lockConfig',
                        name: 'lockConfig',
                        component: resolve => require(['@/components/activity/lockConfig'], resolve)
                    },
                    {
                        path: '/activity/msgConfig',
                        name: 'msgConfig',
                        component: resolve => require(['@/components/activity/msgConfig'], resolve),
                        children: [
                            {
                                path: '/activity/liebian/msgUnLock',
                                name: 'lb_msgUnLock',
                                component: resolve => require(['@/components/activity/liebian/msgUnLock'], resolve)
                            },
                            {
                                path: '/activity/liebian/msgStudy',
                                name: 'lb_msgStudy',
                                component: resolve => require(['@/components/activity/liebian/msgStudy'], resolve)
                            },
                            {
                                path: '/activity/xuqi/msgUnLock',
                                name: 'xq_msgUnLock',
                                component: resolve => require(['@/components/activity/xuqi/msgUnLock'], resolve)
                            },
                            {
                                path: '/activity/xuqi/msgStudy',
                                name: 'xq_msgStudy',
                                component: resolve => require(['@/components/activity/xuqi/msgStudy'], resolve)
                            },
                            {
                                path: '/activity/xuqi/msgRenewal',
                                name: 'xq_msgRenewal',
                                component: resolve => require(['@/components/activity/xuqi/msgRenewal'], resolve)
                            },
                        ]
                    },
                    // 消息模板管理
                    {
                        path: '/template',
                        name: 'template',
                        component: resolve => require(['@/components/template'], resolve)
                    },
                    {
                        path: '/template/editTemplate',
                        name: 'editTemplate',
                        component: resolve => require(['@/components/template/editTemplate'], resolve)
                    },
                    // 集合页管理
                    {
                        path: '/collection',
                        name: 'collection',
                        component: resolve => require(['@/components/collection'], resolve)
                    },
                    {
                        path: '/collection/editCollection',
                        name: 'editCollection',
                        component: resolve => require(['@/components/collection/editCollection'], resolve)
                    },

                    /* ------- */
                    /* 更多代码 */
                    /* ------- */
                ]
            }, 
            ,{
                path: '*',
                redirect: '/login'
            }
        ]
    })

其实router文件就是把所有的路由对象，都放在routes数组里面。

可以用ES6语法的 展开运算符，按模块分类然后拼接到routes数组中。

可以看下展开运算符的基本操作。。

![img6](../../../images/2018/05/6.png)

所以刚刚的问题的路由文件代码可以分割如下

![img7](../../../images/2018/05/7.jpeg)

展示其中一个模块的路由代码

![img8](../../../images/2018/05/8.jpeg)

是不是这样感觉就清爽很多啦~~~ 太帅！！！😝😝😝