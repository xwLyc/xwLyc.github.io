---
layout: post
title: "记一次工作中的项目效率优化"
date: 2019-10-21
description: "记一次工作中的项目效率优化"
tag: 优化
---   

事情的开始是这样子的...
产品: 我们要做一个公众号推广活动，跟渠道方合作的，巴拉巴拉...
我: 好的...
项目上线后没几天，产品又找上我说，我们要新增一个渠道合作方
我一听，心中大为警惕，以我多年的经验来判断，问了一句，除了这个后面还会不会有别的合作方
产品: 不一定，可能还会有，￥%……

so... 还是要做好那种可以配置的，稍微自动的。

访问一个地址，怎么去区分不同渠道商呢？

只有一个办法就是url里面添加 `channel` 字段区分，去匹配相应的渠道内容。

然后，现在要做的是就把之前，所有的可能会变动的内容，都提取出来... 统一文件格式，名称等...

以后就单独改这一个文件就好了。

初步看来还是不错的，但是低估了公司的影响力，渠道商越来越多，修改文件，也变得枯燥无味，也占用时间，还会担心影响到其它页面的内容，都要自测一遍。代码预览如下

    export default {
      data () {
        return {
          channel: '',
          channelName: {
            xeskids: '学而思kids',
            jiazhangbang: '家长帮',
            hangzhouxes: '杭州学而思',
            hefeixes: '合肥学而思',
            huizhouxes: '惠州学而思',
            shenzhenxhxt: '深圳小猴学堂',
            zhongshanxes: '中山学而思',
            zhongshanysx: '中山幼升小',
            jiazhangbangbc: '家长帮编程',
            nanningxes: '南宁学而思',
            shaoxingxes: '绍兴学而思',
            shenzhenxes: '深圳学而思',
            tianjinxes: '天津学而思',
          },
          guideText: {
            xeskids: '学而思kids',
            jiazhangbang: 'codemonkey',
            hangzhouxes: 'codemonkey',
            hefeixes: 'codemonkey',
            huizhouxes: 'codemonkey',
            shenzhenxhxt: 'codemonkey',
            zhongshanxes: 'codemonkey',
            zhongshanysx: 'codemonkey',
            jiazhangbangbc: 'codemonkey',
            nanningxes: 'codemonkey',
            shaoxingxes: 'codemonkey',
            shenzhenxes: 'codemonkey',
            tianjinxes: 'codemonkey',
          },
          guideImages: {
            xeskids: require('@/assets/image/fission/xeskids/guide.png'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/guide.png'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/guide.png'),
            hefeixes: require('@/assets/image/fission/hefeixes/guide.png'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/guide.png'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/guide.png'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/guide.png'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/guide.png'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/guide.png'),
            nanningxes: require('@/assets/image/fission/nanningxes/guide.png'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/guide.png'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/guide.png'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/guide.png'),
          },
          giveImages: {
            xeskids: require('@/assets/image/fission/xeskids/give.png'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/give.png'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/give.png'),
            hefeixes: require('@/assets/image/fission/hefeixes/give.png'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/give.png'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/give.png'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/give.png'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/give.png'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/give.png'),
            nanningxes: require('@/assets/image/fission/nanningxes/give.png'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/give.png'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/give.png'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/give.png'),
          },
          topPic: {
            xeskids: require('@/assets/image/fission/xeskids/toppic.png'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/toppic.png'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/toppic.png'),
            hefeixes: require('@/assets/image/fission/hefeixes/toppic.png'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/toppic.png'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/toppic.png'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/toppic.png'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/toppic.png'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/toppic.png'),
            nanningxes: require('@/assets/image/fission/nanningxes/toppic.png'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/toppic.png'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/toppic.png'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/toppic.png'),
          },
          channelEwm: {
            xeskids: require('@/assets/image/fission/xeskids/ewm.jpg'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/ewm.jpg'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/ewm.jpg'),
            hefeixes: require('@/assets/image/fission/hefeixes/ewm.jpg'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/ewm.jpg'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/ewm.jpg'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/ewm.jpg'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/ewm.jpg'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/ewm.jpg'),
            nanningxes: require('@/assets/image/fission/nanningxes/ewm.jpg'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/ewm.jpg'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/ewm.jpg'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/ewm.jpg'),
          },
          gzhImages: {
            xeskids: require('@/assets/image/fission/xeskids/gzh.png'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/gzh.png'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/gzh.png'),
            hefeixes: require('@/assets/image/fission/hefeixes/gzh.png'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/gzh.png'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/gzh.png'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/gzh.png'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/gzh.png'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/gzh.png'),
            nanningxes: require('@/assets/image/fission/nanningxes/gzh.png'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/gzh.png'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/gzh.png'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/gzh.png'),
          },
          dxlj: {
            xeskids: require('@/assets/image/fission/xeskids/dxlj.png'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/dxlj.png'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/dxlj.png'),
            hefeixes: require('@/assets/image/fission/hefeixes/dxlj.png'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/dxlj.png'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/dxlj.png'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/dxlj.png'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/dxlj.png'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/dxlj.png'),
            nanningxes: require('@/assets/image/fission/nanningxes/dxlj.png'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/dxlj.png'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/dxlj.png'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/dxlj.png'),
          },
          channelLogo: {
            xeskids: require('@/assets/image/fission/xeskids/logo.png'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/logo.png'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/logo.png'),
            hefeixes: require('@/assets/image/fission/hefeixes/logo.png'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/logo.png'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/logo.png'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/logo.png'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/logo.png'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/logo.png'),
            nanningxes: require('@/assets/image/fission/nanningxes/logo.png'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/logo.png'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/logo.png'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/logo.png'),
          },
          posterTitle: {
            xeskids: require('@/assets/image/fission/xeskids/posterTitle.png'),
            jiazhangbang: require('@/assets/image/fission/jiazhangbang/posterTitle.png'),
            hangzhouxes: require('@/assets/image/fission/hangzhouxes/posterTitle.png'),
            hefeixes: require('@/assets/image/fission/hefeixes/posterTitle.png'),
            huizhouxes: require('@/assets/image/fission/huizhouxes/posterTitle.png'),
            shenzhenxhxt: require('@/assets/image/fission/shenzhenxhxt/posterTitle.png'),
            zhongshanxes: require('@/assets/image/fission/zhongshanxes/posterTitle.png'),
            zhongshanysx: require('@/assets/image/fission/zhongshanysx/posterTitle.png'),
            jiazhangbangbc: require('@/assets/image/fission/jiazhangbangbc/posterTitle.png'),
            nanningxes: require('@/assets/image/fission/nanningxes/posterTitle.png'),
            shaoxingxes: require('@/assets/image/fission/shaoxingxes/posterTitle.png'),
            shenzhenxes: require('@/assets/image/fission/shenzhenxes/posterTitle.png'),
            tianjinxes: require('@/assets/image/fission/tianjinxes/posterTitle.png'),
          },

        }
      },
      created() {
        this.channel = sessionStorage.getItem('channel')
      },
    }


可以看到引入文件里面占用了大部分重复的东西，引入图片不同的地方就是渠道文件夹名称。

一个个新增渠道也是很觉得很麻烦的事情，复制粘贴修改，再滚动复制粘贴修改...

这种低级效率的事情，实在不想写，怎么办...

不慌，有webpack这么强大的工具当然要好好利用，通过 `require.context`去处理引入文件问题。然后通过keys()遍历文件夹下的文件。

具体功能，可以访问[`require.context`](https://webpack.docschina.org/guides/dependency-management/#require-context)了解，就不多说啦

这时候代码优化如下：


    const modulesFiles = require.context('@/assets/image/fission', true, /\guide.(png|jpg)$/) 
    const modulesFilesKeys = modulesFiles.keys().map(file => {
      return file.replace(/(^.\/)(\w+)(\/guide.(png|jpg))/, '$2')
    })
    // console.log(modulesFilesKeys)
    const guideImages = {}
    const giveImages = {}
    const topPic = {}
    const gzhImages = {}
    const dxlj = {}
    const channelLogo = {}
    const posterTitle = {}
    const channelEwm = {}
    modulesFilesKeys.forEach(name => {
      guideImages[name]   = require(`@/assets/image/fission/${name}/guide.png`)
      giveImages[name]    = require(`@/assets/image/fission/${name}/give.png`)
      topPic[name]        = require(`@/assets/image/fission/${name}/toppic.png`)
      gzhImages[name]     = require(`@/assets/image/fission/${name}/gzh.png`)
      dxlj[name]          = require(`@/assets/image/fission/${name}/dxlj.png`)
      channelLogo[name]   = require(`@/assets/image/fission/${name}/logo.png`)
      posterTitle[name]   = require(`@/assets/image/fission/${name}/posterTitle.png`)
      channelEwm[name]    = require(`@/assets/image/fission/${name}/ewm.jpg`)
    })
    export default {
      data () {
        return {
          channel: '',
          channelName: {
            xeskids: '学而思kids',
            jiazhangbang: '家长帮',
            hangzhouxes: '杭州学而思',
            hefeixes: '合肥学而思',
            huizhouxes: '惠州学而思',
            shenzhenxhxt: '深圳小猴学堂',
            zhongshanxes: '中山学而思',
            zhongshanysx: '中山幼升小',
            jiazhangbangbc: '家长帮编程',
            nanningxes: '南宁学而思',
            shaoxingxes: '绍兴学而思',
            shenzhenxes: '深圳学而思',
            tianjinxes: '天津学而思',
            nanjingxes: '南京学而思',
            xiamenjzb: '厦门家长帮',
          },
          guideText: {
            xeskids: '学而思kids',
            jiazhangbang: 'codemonkey',
            hangzhouxes: 'codemonkey',
            hefeixes: 'codemonkey',
            huizhouxes: 'codemonkey',
            shenzhenxhxt: 'codemonkey',
            zhongshanxes: 'codemonkey',
            zhongshanysx: 'codemonkey',
            jiazhangbangbc: 'codemonkey',
            nanningxes: 'codemonkey',
            shaoxingxes: 'codemonkey',
            shenzhenxes: 'codemonkey',
            tianjinxes: 'codemonkey',
            nanjingxes: 'codemonkey',
            xiamenjzb: 'codemonkey',
          },
          guideImages,
          giveImages,
          topPic,
          channelEwm,
          gzhImages,
          dxlj,
          channelLogo,
          posterTitle,
        }
      },
      created() {
        this.channel = sessionStorage.getItem('channel')
      },
    }


但是，现在并没有达到我的目的。 渠道商名字 `channelName`, 用来复制公众号回复的`guideText`，仍需要手动修改。

有没有办法不手动修改这些数据呢？

>问题一直都有的，解决问题的办法也总是会有的...(引用某个人经常说的话😂)

引入文件，可以正则匹配文件名，不就可以获取到了嘛。。。

新建一个txt文件，把渠道名称作为文件名。

![image.png](https://image-static.segmentfault.com/297/235/2972354045-5dad2b9e6110c_articlex)

最终优化代码如下：


    const channelName = {}
    const modulesFiles = require.context('@/assets/image/fission', true, /\/[\s\S]+\.txt$/) // 匹配对应渠道.txt文件
    const modulesFilesKeys = modulesFiles.keys().map(file => {
      let regResult = file.replace(/^.\/(\w+)\/([\s\S]+).txt/, '$1,$2')
      let fileName = regResult.split(',')[0] // 匹配对应渠道文件夹名字
      let txtName = regResult.split(',')[1] // 匹配对应渠道.txt文件名
      channelName[fileName] = txtName
      return fileName
    })
    // console.log(modulesFilesKeys)
    const guideText = {}
    const guideImages = {}
    const giveImages = {}
    const topPic = {}
    const gzhImages = {}
    const dxlj = {}
    const channelLogo = {}
    const posterTitle = {}
    const channelEwm = {}
    modulesFilesKeys.forEach(name => { // 根据渠道文件夹名字访问该渠道下的资源
      guideText[name]     = name === 'xeskids' ? '学而思kids' : 'codemonkey'
      guideImages[name]   = require(`@/assets/image/fission/${name}/guide.png`)
      giveImages[name]    = require(`@/assets/image/fission/${name}/give.png`)
      topPic[name]        = require(`@/assets/image/fission/${name}/toppic.png`)
      gzhImages[name]     = require(`@/assets/image/fission/${name}/gzh.png`)
      dxlj[name]          = require(`@/assets/image/fission/${name}/dxlj.png`)
      channelLogo[name]   = require(`@/assets/image/fission/${name}/logo.png`)
      posterTitle[name]   = require(`@/assets/image/fission/${name}/posterTitle.png`)
      channelEwm[name]    = require(`@/assets/image/fission/${name}/ewm.jpg`)
    })
    export default {
      data () {
        return {
          channel: '',
          channelName,
          guideText,
          guideImages,
          giveImages,
          topPic,
          channelEwm,
          gzhImages,
          dxlj,
          channelLogo,
          posterTitle,
        }
      },
      created() {
        this.channel = sessionStorage.getItem('channel')
      },
    }


以后再新增渠道，只需要渠道新建文件夹，以及该渠道下的各个资源，上去文件就可以啦，再也不用担心修改代码的问题了。✌️✌️✌️