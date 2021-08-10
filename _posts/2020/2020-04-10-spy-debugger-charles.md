---
layout: post
title: "spy-debugger + Charles 移动端调试"
date: 2020-03-30
description: "真机调试 spy-debugger + Charles 移动端调试"
tag: h5
---

#### spy-debugger + Charles 移动端调试

### charlse电脑安装证书

已安装过的可以直接跳过

**第1步：**点击`Charles`的 `Help > SSL Proxying > Install Charles Root Cetificate` 然后就会弹出证书安装页面，将`Charles`的证书设置为始终信任即可：

![pic](../../../images/2020/03/07.png)


![pic](../../../images/2020/03/08.png)



**第2步：**点击`Charles`的`Proxy > Access Control Settings` 进行配置让手机连接代理时，不需要点允许连接确认操作。

![pic](../../../images/2020/03/09.png)


上述配置，表示允许任意IP的设备连接该代理服务，不会有允许连接确认对话框。

### IOS手机安装证书

**第1步：**点击`Charles`的`Help > SSL Proxying > Install Charles Root Cetificate on a Mobile Device or Remote Browser `然后会弹出一个对话框，告诉你手机要设置的代理IP地址和端口

注意：手机和电脑必须连接同一个WiFi才可以。

![pic](../../../images/2020/03/10.png)


**第2步**：根据提示在手机上配置Wi-Fi网络代理，在手机上点击设置 > 无线局域网  选中后 

**第3步：**点击配置代理，选择手动，服务器和端口输入Charles对话显示的IP和端口号，配置好后，记得点击存储

**第4步**：在Safari浏览器输入chls.pro/ssl ，下载并安装证书

![pic](../../../images/2020/03/11.png)


**第5步：**在手机的 设置 > 通用 > 描述文件与设备管理 找到Charles Proxy CA 证书，点击安装

**第6步：**在手机的 设置 > 通用 > 关于本机 > 证书信任设置 将 Charles Proxy CA 打开

此时，Charles所有的准备工作都完成了，接下来我们就可以启动`spy-debugger`进行移动端H5调试了。

### 启动spy-debugger

**第1步**：启动命令

    spy-debugger -e http://127.0.0.1:8888 // 启动spy-debugger服务，并设置外部代理为Charles的服务

上述命令表示启动`spy-debugger`调试服务，并将所有的资源请求都转发到Charles的代理服务上。其实我们打开Charles程序的时候，它实际上是在本地启动了一个http的服务，监听在8888端口上。

![pic](../../../images/2020/03/12.png)


**第2步：**在手机上设置代理服务器和端口为`spy-debugger`的IP和端口:

![pic](../../../images/2020/03/13.png)


**第3步：**在浏览器打开http://127.0.0.1:63890/client/

![pic](../../../images/2020/03/14.png)


**第4步：**在网校App里找到聚合H5页面打开


![pic](../../../images/2020/03/15.png)



**第5步：**此时在浏览器上的Remote选项卡上就可以看到，连接的终端了

![pic](../../../images/2020/03/16.png)


**第6步：**我们可以在Elements选项上进行页面元素的选择和调试，可以发现我们鼠标放到元素上，手机端上会实时看到选中效果

![pic](../../../images/2020/03/17.png)


**第7步：**我们还可以在Console选项卡下查看代码输出的console信息，我们也可以这里输入页面要执行的代码

![pic](../../../images/2020/03/18.png)

![pic](../../../images/2020/03/19.png)



**第8步：**此时我们发现所有的请求都被转发到了Charles上


![pic](../../../images/2020/03/20.png)



OK，到这里`spy-debugger + Charles`进行移动端调试的接入流程就介绍完了，更多关于spy-debugger的功能和使用方法，可以参考[spy-debuger的官方README](https://www.npmjs.com/package/spy-debugger)
