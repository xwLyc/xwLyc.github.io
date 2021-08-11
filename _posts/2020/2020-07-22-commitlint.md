---
layout: post
title: "规范-git提交规范"
date: 2020-07-21
description: "规范-git提交规范"
tag: h5
---

#### git提交规范

### 背景

团队人多，经常`git commit message` 的时候，懒得写只写个修改或者其它杂乱的名字，都不知道修改了啥。

优雅的提交，方便团队协作和快速定位问题，也是其中重点。

### 安装

    npm install --save-dev @commitlint/config-conventional @commitlint/cli

    // 生成配置文件commitlint.config.js，当然也可以是 .commitlintrc.js
    echo "module.exports = {extends: ['@commitlint/config-conventional']};" > commitlint.config.js


### 项目内配置

根目录下创建 `commitlint.config.js`文件

    module.exports = { extends: ['@commitlint/config-conventional'] };


在husky的配置加入CommitlIint配置

    "husky": {
        "hooks": {
            "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
        }
    },

此时提交不符合规范的message就会终止提交



参考文章： https://segmentfault.com/a/1190000017790694
