---
layout: post
title: "commitizen 安装后执行 git-cz 弹出 vim窗口"
date: 2021-03-21
description: "commitizen 安装后执行 git-cz 弹出 vim窗口"
tag: h5
---

`commitzen`和`cz-conventional-changelog`需要搭配使用，
一开始以为不需要写入log记录，就没安装，导致执行`git-cz`命令时，并未出现想要的交互效果。


### 配置

安装好`commitzen`和`cz-conventional-changelog`之后，还需要添加一些配置：
package.json 中添加：

```json
"scripts": {
  "commit": "git-cz"
},
"config": {
  "commitizen": {
    "path": "node_modules/cz-conventional-changelog"
  }
}
```

在配置中，我们需要指定 Adapter，否则在执行`git-cz`的时候，不会出现选择的交互界面，而是直接弹出 vim 编辑器编辑 message。

配置好之后使用`npm run commit`替代`git commit`提交。
