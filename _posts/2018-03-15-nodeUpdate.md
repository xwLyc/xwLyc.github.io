---
layout: post
title: "[Node]升级到最新版本以及指定版本方法"
date: 2018-03-15
description: "[Node]升级到最新版本以及指定版本方法"
tag: node
---   

### [Node]升级到最新稳定版方法

网上搜到一个可行方法,赶紧记录下来.

#### Mac环境升级

第一步，先查看本机node.js版本：

    $ node -v

第二步，清除node.js的cache：

    $ sudo npm cache clean -f

第三步，安装 n 工具，这个工具是专门用来管理node.js版本的，别怀疑这个工具的名字，是他是他就是他，他的名字就是 “n”

    $ sudo npm install -g n

第四步，安装最新版本的node.js

    $ sudo n stable

#### 如果想升级到指定版本或者切换版本，这一步可以这么操作

使用 n 加版本号就可以安装其他版本，比如：

    sudo  n  8.11.2

再使用 n ，通过上下键，就可以选择不同的版本啦


第五步，再次查看本机的node.js版本：

    $ node -v

### 如图


![pic](../../../images/2018/03/a.png)