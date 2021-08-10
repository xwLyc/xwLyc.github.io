---
layout: post
title: "真机调试 spy-debugger"
date: 2020-03-30
description: "真机调试 spy-debugger"
tag: h5
---

#### 真机调试 spy-debugger

​移动端调试一直都是一个痛点，因为移动终端对于我们来说是一个黑盒，它无法像PC端一样，我们可以通过F12很方便的调出开发者工具。在开发中经常会遇到同样一份代码在某个型号的手机上运行出现错误，其他手机都好好的，开发的时候Chrome上也没有报错。如果没有调试工具这种情况下我们就很难定位问题，下面介绍我们的主角spy-debugger。

### 关于spy-debugger

> 一站式页面调试、抓包工具。远程调试任何手机浏览器页面，任何手机移动端webview（如：微信，HybridApp等）。支持HTTP/HTTPS，无需USB连接设备。  

一看到这个介绍，想想之前ios调试仅限于safari调试，安卓需要开启`chrome://inspect/` ,而且都需要USB链接。对于目前在家办公mac pro需要转接器才能USB链接就更没法调试了。

而且还支持端内调试，可以进行抓包，刚好适合我们现在的业务情况。`vconsole` 也是一个很不错的工具了，但是每次都要点开查看以及看埋点也不停地滚动记录，线上环境也没法开启`vconsole`，看不到详细的数据。

心里不得不感慨一句，靠，牛逼啊...

**特性**

    1、页面调试＋抓包

    2、操作简单，无需USB连接设备

    3、支持HTTPS。

    4、spy-debugger内部集成了weinre、node-mitmproxy、AnyProxy。

    5、自动忽略原生App发起的https请求，只拦截webview发起的https请求。对使用了SSL pinning技术的原生App不造成任何影响。

    6、可以配合其它代理工具一起使用(默认使用AnyProxy) 

### 使用案例

**初体验**

`spy-debugger `启动后，手机端打开网校app，访问聚合页，我们看到的是这样的。

我们可以实时看到访问地址，页面元素，资源，网络请求，控制台以及请求抓包等相关信息。的确是描述的那样，可以远程调试任何手机浏览页面，支持https,无需USB链接调试，确实方便了很多。

![pic](../../../images/2020/03/04.png)


### 安装

Windows 下：

    npm install spy-debugger -g

 Mac 下：

    sudo npm install spy-debugger -g

### 快速上手

**第一步：**手机和PC保持在同一网络下（比如同时连到一个Wi-Fi下）

**第二步：**终端命令行输入`spy-debugger`，按命令行提示用浏览器打开相应地址。

**第三步：**设置手机的HTTP代理，代理IP地址设置为PC的IP地址，端口为`spy-debugger`的启动端口(默认端口：9888)。

Android设置代理步骤：`设置 - WLAN - 长按选中网络 - 修改网络 - 高级 - 代理设置 - 手动`

iOS设置代理步骤：`设置 - 无线局域网 - 选中网络 - HTTP代理手动`

**第四步：**必须先设置完第三步代理后再通过(非微信)手机浏览器访问http://s.xxx安装证书（暂无安卓手机，以iOS为例）

  在手机的 `设置-> 通用-> 描述文件与设备管理` 找到 `node-mitmproxy CA` 证书，并点击安装

  iOS新安装的证书需要手动打开证书信任 ，`设置->通用->关于本机->证书信任设置`-> 找到`node-mitmproxy CA`（打开）

**第五步：**用手机浏览器访问你要调试的页面即可。

**注： 打开证书信任很重要，不然是没办法打开页面进行调试的**



###  自定义选项

端口（默认端口：9888）

      Spy-debugger -p 8888  //设置手机配置代理端口

开启`AnyProxy代`理 （使用外部代理）

`spy-debugger` 默认使用 `anyproxy代`理，启动后的页面是这样的


![pic](../../../images/2020/03/05.png)



要想正常使用，还需要安装一下证书，点击下载`rootca`证书，选择信任，就可以正常拦截`https`请求了。当然要保证手机端开启`node-mitmproxy CA `认证。


![pic](../../../images/2020/03/06.png)



喜欢用charles的大佬也可以配置 charles ，传送门使用外部代理



### 页面编辑模式

    启动命令：spy-debugger -w true

内部实现原理：在需要调试的页面内注入代码：`document.body.contentEditable=true`。暂不支持使用了iscroll框架的页面。

**是否允许weinre监控iframe加载的页面（默认false）**

    Spy-debugger -i true

**是否只拦截浏览器发起的https请求（默认true）**

    Spy-debugger -b false

有些浏览器发出的`connect`请求没有正确的携带`userAgent`，这个判断有时候会出错，如UC浏览器。

这个时候需要设置为false。大多数情况建议启用默认配置：true，由于目前大量App应用自身（非WebView）发出的请求会使用到SSL pinning技术，自定义的证书将不能通过app的证书校验。。

**是否允许HTTP缓存（默认false）**

    spy-debugger -c true



`spy-debugger`内部集成了`weinre`，`spy-debugger`原理是拦截所有html页面请求注入`weinre`所需要的js代码，简化了`weinre`需要给每个调试的页面添加js代码，让页面调试更加方便。
