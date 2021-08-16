---
layout: post
title: "process.cwd()和__dirname的区别"
date: 2021-02-24
description: "process.cwd()和__dirname的区别"
tag: node
---

**process.cwd()**返回当前工作目录。 如：调用node命令执行脚本时的目录。

**__dirname**返回源代码所在的目录。

eg：对于d:\dir\index.js。

    console.log(`cwd: ${process.cwd()}`);
    console.log(`dirname: ${__dirname}`);

命令 | process.cwd() | __dirname
node index.js  | d:\dir  | d:\dir 
node dir\index.js | d: | d:\dir 
