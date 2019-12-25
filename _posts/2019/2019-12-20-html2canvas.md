---
layout: post
title: "html2canvas 生成海报图 开发中遇到的问题"
date: 2019-12-20
description: "html2canvas 生成海报图 开发中遇到的问题"
tag: html5 canvas 
---   

### ios里，生成的图片不显示

    canvas.toDataURL('image/jpeg', 1.0)
    
使用image/jpeg格式，在指定图片格式为 image/jpeg 或 image/webp的情况下，可以从 0 到 1 的区间内选择图片的质量。

### canvas 是弹出层时，底层有滚动条，会有部分空白

测试的时候，好奇为啥每次，生成的图片都缺一部分，而且都是从顶部开始空白，再绘制图片。

解决方案: 添加绘制坐标y起始位置 `y: window.pageYOffset,`

    html2canvas(target, {
        useCORS: true,
        y: window.pageYOffset,
    }).then(...)

### canvas 白屏终极解决方案
不考虑图片加载问题，大多是因为没找到绘制的位置的原因，会导致白屏黑屏的现象。
而且也会有改动 海报图布局的情况，经常会调位置，再遇见缩小海报图展示的需求，就要改动很多了，所以现在一般都是，把canvas定位到屏幕外面，然后绘制生成图片。对图片放大缩小就容易的多。

解决方案：

    html2canvas(target, {
        useCORS: true,
        y: (window.pageYOffset + window.innerHeight),
    }).then(...)

### ios 生成图片时显示时不显的，大图几乎不显示。

猜测是图片渲染的原因，为了确保能够生成图片，生成图片写在了所有图片加载完成之后，才进行canvas绘制图片功能，但是还是有渲染不出来的情况。这个搜了很久，跨域 `useCORS: true`，也添加了。
Android是没有问题的。

然后搜了很多看到这么一段话：


>原因跟html2canvas库的工作原理有很大的关系.html2canvas库需要我们先提供一段DOM节点，然后它再读取并解析这一段DOM节点生成canvas对象。如果DOM节点中已经使用了<img>标签的话，它也会解析这个<img>标签的src属性，然后重新创建一个Image对象，给它添加`crossOrigin="anonymous"`属性后尝试以跨域的方式重新读取图片数据。需要注意的是，一般CDN上的图片都是带有缓存响应头并且会在浏览器端缓存的，而且缓存的不仅仅是图片数据，还有HTTP响应头。所以问题的根本原因我们就找到了，当html2canvas尝试以跨域的方式去读取图片数据时，它读取到的是浏览器的缓存数据，而且因为我们没有给DOM节点中的<img>标签添加`crossorigin="anonymous"`属性，所以缓存数据是不带Access-Control-Allow-Origin响应头的，进而导致html2canvas库读取到的图片数据污染了生成的canvas对象，最终致使canvas导出数据报错

所以我们要做的事情也很简单，就是给DOM节点中的每一个<img>标签都加上`crossorigin="anonymous"`属性就可以了。

### 在iphone5不能生成海报图以及部分安卓机第一次不能正常生成海报图

被这个问题搞了很久，微信开发测试也麻烦，不停打包上线测试，最终测试出问题还是由于图片加载的问题，用的 vue 的`@load`去判断加载，但是对于某些手机这个判断似乎失效了。

然后就用原生的方法等待所有图片加载完成后，在执行绘制，奇迹发生了，居然解决了！

    function imgLoad(imgArr, ck) {
      let imgs = imgArr
      Promise.all(imgs.map((src, index) => {
        return new Promise((resolve, reject) => {
          let img = new Image()
          img.onload = () => {
            resolve(img)
          }
          img.onerror = () => {
            reject(src, index)
          }
          img.crossOrigin = 'anonymous'
          img.src = src
        })
      })).then(arr => {
        ck(arr)
      }).catch(e => {
        console.log(e)
      })
    },

绘制海报图的兼容问题真是头疼的一件事情，但是能解决掉它还是令人兴奋的事情， 开发痛并快乐着 😂😂😂

自己写了一个demo，展示地址是这个 [https://uq1uv.csb.app/](https://uq1uv.csb.app/=?_blank) 看看大家的手机都支持不，有问题的欢迎反馈！！

demo地址： [![Edit create-poster](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/create-poster-uq1uv?fontsize=14&hidenavigation=1&theme=dark)