---
layout: post
title: "规范-分支命名规范"
date: 2020-07-21
description: "规范-分支命名规范"
tag: h5
---

#### 分支命名规范

### 背景

由于部门开发人员较多，大家对于分支的命名没有统一的规范，造成代码分支命名比较混乱，没有语义化，特此规范分支命名

在git push之前对分支进行校验，加入不符合规范禁止提交

### 规范说明

    master : 主分支，默认创建，用于线上的正式代码发布
    dev: 开发环境分支，用于开发代码提交到开发环境

    release : (测试环境1)分支，用于开发代码提交到测试环境1
    release02 : (测试环境2)分支，用于开发代码提交到测试环境2
    release03 : (测试环境3)分支，用于开发代码提交到测试环境3

    feature : 功能开发分支，用于日常的项目功能开发
    pre : 灰度分支，用于开发代码提交到灰度环境
    hotfix : 紧急问题修复分支，用于线上bug紧急修复


功能分支需补充分支的其他信息，格式为 `$type_$personname_$createtime_$description`

    type：feature/hotfix, 分支类别
    personname：使用者
    createtime：创建时间，年月日，例：180306
    description：描述，创建分支的目的，可用项目名

示例：feature_lyc_20200721_login

### Quick Start

`husky`负责提供更易用的`git hook`

    npm i -D husky

```package.json```

    "husky": {
        "hooks": {
        "pre-push": "node ./scripts/check-branch.js"
        }
    },

```check-branch.js```

    const execSync = require("child_process").execSync;
    const chalk = require("chalk");

    const currentBranch = execSync("git symbolic-ref --short -q HEAD")
    .toString()
    .trim();
    console.log(chalk.blue("⏰ pre-push hook start!\n"));
    console.log(chalk.blue(`👉 当前的分支:${currentBranch}\n`));
    const branchReg = /^(master|release|pre|dev){1}$|^(master_|release_|pre_|dev_){1}.*$|^(feature|hotfix){1}_(.+)_(\d+)_(.+)$/;
    if (!branchReg.test(currentBranch)) {
    console.log(chalk.red("\n当前分支命名不符合规范，请按照如下规范"));
    console.log(
        `${chalk.red("$type")}_${chalk.green("$personname")}_${chalk.yellow(
        "$createtime"
        )}_${chalk.blue("$description")}`
    );
    const username = getGitUserInfo().username;
    const date = getCurrentDateDay();
    console.log(
        `${chalk.red("feature")}_${chalk.green(username)}_${chalk.yellow(
        date
        )}_${chalk.blue("initproject")}  功能开发分支`
    );
    console.log(
        `${chalk.red("hotfix")}_${chalk.green(username)}_${chalk.yellow(
        date
        )}_${chalk.blue("initproject")} 紧急问题修复分支\n`
    );

    console.log(
        `重命名当分支参考操作：${chalk.cyanBright(
        `git branch -m ${currentBranch} feature_${username}_${date}_initproject`
        )}`
    );

    process.exit(1);
    }
    console.log(chalk.greenBright("✔️ 分支校验通过\n"));

    // 获取当前的年月日
    function getCurrentDateDay() {
    const date = new Date();
    const year = date.getFullYear();
    let month = date.getMonth() + 1;
    month = month < 10 ? "0" + month : month;
    const day = date.getDate();
    return `${year}${month}${day}`;
    }

    // 获取git用户的信息
    function getGitUserInfo() {
    const username = execSync("git config user.name").toString().trim();
    const email = execSync("git config user.email").toString().trim();
    return {
        username,
        email,
    };
    }
