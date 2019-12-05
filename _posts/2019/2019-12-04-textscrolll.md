---
layout: post
title: "vue 实现文字无缝滚动效果（从下往上滚动）"
date: 2019-12-04
description: "vue 实现文字无缝滚动效果（从下往上滚动）"
tag: vue 
---   

#### 需求：对中奖用户进行滚动效果展示

![Kapture 2019-12-04 at 14.36.51.gif](https://image-static.segmentfault.com/398/380/3983800482-5de7541f3d3e9_articlex)

这种效果需求还是蛮多的，想起之前用`JQuery`实现的一个无缝滚动( [缅怀过去](https://www.jq22.com/jquery-info1237))，是通过jq的`animate`方法实现的，动画结束之后就将第一个元素`appendTo`追加到最后面，实现循环滚动特效，不得不感叹技术更新换代真的很快。

回到现在Vue的项目，本来想用插件，但是觉得这么一个小动画用插件有点太重了，还是自己写一个比较好。

#### 实现原理

实现原理也比较简单

1. 对整个列表实现上移动画
2. 将列表的第一个数据移动到最后一个

因为vue是基于数据驱动的，所以，对我们开发者来说，直接操控数组，删除第一个数组数据，然后追加到数组后面就好了。

点此查看[实现效果以及源码预览](https://codesandbox.io/s/vue-marquee-s18jm)可以略过下面的内容

**template 部分**

      <!-- 无缝滚动效果 -->
      <div class="marquee-wrap">
        <ul class="marquee-list" :class="{'animate-up': animateUp}">
          <li v-for="(item, index) in listData" :key="index">{{item}}</li>
        </ul>
      </div>
      
**script部分**

    export default {
      name: "marquee-up",
      data() {
        return {
          animateUp: false,
          listData: ['12***ve 成功邀请12人 已获奖金60元', 'l***e 成功邀请5人 已获奖金40元', 'l***e 成功邀请1人 已获奖金5元'],
          timer: null
        }
      },
      mounted() {
        this.timer = setInterval(this.scrollAnimate, 1500);
      },
      methods: {
        scrollAnimate() {
          this.animateUp = true
          setTimeout(() => {
            this.listData.push(this.listData[0])
            this.listData.shift()
            this.animateUp = false
          }, 500)
        }
      },
      destroyed() {
        this.timer = null
      }
    };
    
**style 部分**

    .marquee-wrap  {
      width: 80%;
      height: 40px;
      border-radius: 20px;
      background: rgba($color: #000000, $alpha: 0.6);
      margin: 0 auto;
      overflow: hidden;
      .marquee-list {
        li {
          width: 100%;
          height: 100%;
          text-overflow: ellipsis;
          overflow: hidden;
          white-space: nowrap;
          padding: 0 20px;
          list-style: none;
          line-height: 40px;
          text-align: center;
          color: #fff;
          font-size: 18px;
          font-weight: 400;
        }
      }
      .animate-up {
        transition: all 0.5s ease-in-out;
        transform: translateY(-40px);
      }
    }