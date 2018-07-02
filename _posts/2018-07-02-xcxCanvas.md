---
layout: post
title: "微信小程序 canvas 绘制分享卡片图片"
date: 2018-07-02
description: "微信小程序 canvas 绘制分享卡片图片"
tag: 小程序
---   

直接贴代码

    drawDivToImage(){
        let drawBG = (avatarUrl) => {
            let ctx = wx.createCanvasContext('myCanvas')
            let transitionPX = (s) => {
                return wx.getSystemInfoSync().windowWidth / 750 * s
            }
            let canvasW = transitionPX(750)
            let canvasH = transitionPX(620)
            let headSize = transitionPX(126)
            console.log(this.cardUrl)
            //引入背景图
            ctx.drawImage(this.cardUrl, 0, 0, canvasW, canvasH)
            ctx.setFontSize(transitionPX(26))
            ctx.setFillStyle('white')
            ctx.textAlign="left";
            ctx.fillText(this.stdEndData.course.replace(/\r|\n/g," "), transitionPX(42), transitionPX(396))


            // 设置矩形内文本
            ctx.setFillStyle('#603913');
            ctx.setFontSize(transitionPX(36))
            ctx.textAlign="center";
            ctx.fillText(this.stdEndData.wordDayCount, transitionPX(294), transitionPX(506));
            ctx.fillText(this.stdEndData.sentenceDayCount, transitionPX(451), transitionPX(506));
            ctx.fillText(this.stdEndData.totalScore, transitionPX(600), transitionPX(506));
            
            
            //设置头像
            ctx.save();         //保存之前的绘制环境
            ctx.arc(transitionPX(133),transitionPX(511), headSize/2, 0, 2*Math.PI);     //参数一定要写全，不然会导致手机显示问题
            ctx.clip();     //剪切某个区域， 使得后面绘制的都在 arc里面
            ctx.drawImage(avatarUrl, transitionPX(70), transitionPX(448), headSize, headSize);
            ctx.restore()   //恢复之前的绘图环境，不然以后绘制的都不显示，类似 overflow:hidden效果


            //非常重要 canvasToTempFilePath 需要放在draw的回调函数里面，保证能在 draw 绘制完成后，生成图片地址
            ctx.draw(false, ()=>{               
                wx.canvasToTempFilePath({
                    x: 0,
                    y: 0,
                    width: canvasW,
                    height: canvasH,
                    destWidth: canvasW,
                    destHeight: canvasH,
                    canvasId: 'myCanvas',
                    success: (res)=> {
                        // console.log(res.tempFilePath)
                        this.sharePic = res.tempFilePath
                    }, 
                })
            });
        }

        getImageInfo(this.stdEndData.avatarUrl)   //getImageInfo 自己封装的小程序方法
        .then(avatarUrl => {
            drawBG(avatarUrl.path)
        })
    }