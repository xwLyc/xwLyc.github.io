---
layout: post
title: "规范-代码质量规范"
date: 2020-08-30
description: "规范-代码质量规范"
tag: h5
---

#### 代码质量规范

### 背景

团队多人开发项目，每个人的保存风格不一致，经常导致合并冲突。更甚者，正好同时开发一个文件，导致根本没法合并，不得不关掉自动保存功能。

一旦没有了自动保存功能，也没有提交校验，代码质量将得不到保证，极有可能引发线上问题。

### 解决方案
使用rules-cli 约束团队代码规范，

### 安装方式  

    `npm install -g @xes/rules-cli` # 外网不可用

然后使用 `rules-cli all`  安装全部功能

    1. checkbranch 分支校验
    2. checkmessage git提交信息校验
    3. checkcode 代码质量规范

也可以单独可以使用 `rules-cli checkcode` 安装代码质量规范

### rules-cli 做了什么

    1. 安装功能所需依赖
    2. 将相关钩子及配置写入package.json中（ commit , husky, lint-staged等）
    3. 创建(修改)相关规则以及配置文件（.rulesconfig .vscode .eslintrc.js .stylelintrc.js .eslintignore .sytlelintignore等）

### 代码质量检查范围

代码质量检查从js,css,vue三个方面入手

    1. eslint继承了recommend
    2. vue继承airbnb
    3. stylelint继承standard以及recessorder

提交代码时会执行rules-cli `checkcode` 写入的钩子`pre-commit`方法，针对暂存区的js内容进行eslint检查，样式进行检查并修复。

    "husky": {
        "hooks": {
        "pre-commit": "lint-staged",
        }
    },
    "lint-staged": {
        "src/**/*.js": [
        "eslint --ext .js"
        ],
        "src/**/*.vue": [
        "eslint --ext .vue",
        "stylelint --fix .vue"
        ],
        "src/**/*.{css,scss}": [
        "stylelint --fix .css,.scss"
        ]
    }

如果没有使用编辑器保存自动格式化(修复)相关功能的话，保存势必出现很多error，一个个去修改恐怕不太现实，所以还需要结合vscode保存自动格式化功能。

### 如何使vscode保存按照eslint,styelint的规范自动格式化（重要）

1. 安装vetur, eslint，stylelint插件(必要，安装后Vscode才能给相关代码错误提示)

2. 启用eslint校验功能， vscode右下角点击ESLint，allowed 变为对勾即可

3. 配置vscode的 settings.json 保存格式化功能。


已集成到`rules-cli` `checkcode`里，会在当前目录下新建一个`.vscode/setting.json`文件并写入相关配置。已无需在手动修改vscode的全局`settings.json`文件

    如果项目目录是如下结构，根目录是projects，而我们安装的checkcode是在client目下安装的，需要把client目录下的 .vscode 文件夹移到根目录(projects)下，并将根目录的 .gitignore 要忽略的 .vscode 注释或删除掉。

    最后重启vscode即可。

    projects/ 
    client/
    server/
    ...


### 遇到的问题：

**1. 当vscode打开了多个项目时，ESLint默认会把顶层目录作为workingDirectory ，从那个目录下加载插件等依赖，导致找不到依赖。以至于ESLint不起作用。**

此时我们可以在这些顶层目录下创建一个 `.vscode/setting.json` 文件，写入如下内容：
    {
    "eslint.workingDirectories": [
        { "mode": "auto" }
    ]
    }

这样 `ESLint` 会改为把 `package.json` 存在的目录作为 `workingDirectory`，也就能正常引入依赖了。


**2. 如果遇到eslint不起作用，不防点击编辑器右下角的ESLint，去查看输出日志。根据错误信息去做出相应处理。**
(eg: 有的可能提示需要全局安装`eslint`。 `npm install -g eslint`即可)

**3. ESLint日志报错信息：Configuration for rule "camelcase" is invalid:  Value {"properties":"never","ignoreDestructuring":false} should NOT have additional properties.**

这个问题搜了很久没找到解决方案，使用排除法，最终定位是因为当前项目eslint版本较低，跟airbnb版本不匹配导致。更新到最新依赖即可（已添加到cli中，安装使用@lasted）

**4. Eslint日志报错信息：Failed to load plugin 'vue' declared in '.eslintrc.js': Cannot find module 'eslint/lib/util/traverser' Require stack:**

问题同上面一条，eslint-plugin-vue 更新到最新版本（已添加到cli中，安装使用@lasted）

**5. 初次打开项目编辑器右下角可能会有Vetur错误弹窗，Vetur can't find tsconfig.json, jsconfig.json in /xxxx/xxxxxx.**

由于Vetur插件版本问题造成的。

可以直接在vscode设置中使用 `"vetur.ignoreProjectWarning": true`,  来关闭此警告。

快捷键 `"Command" + ","` 打开设置，输入settings搜索，打开`settings.json` 文件，新增`"vetur.ignoreProjectWarning": true` 即可




### 如何忽略文件

1. 对于某些旧代码改动较大，不想进行校验的，在项目新建一个`.eslintignore`，`.stylelintignore` 在里面分别指定要忽略校验的 文件

2. 对于旧项目如何将所有文件（多层级）过滤，只校验新增文件呢？

    - src
        - api
        - assets
        - compnonents
        - pages
            - home
                - index.vue
                - index.js
            - about
                - index.vue 
                - index.js
        - utils

对于上述目录结构，我想新增一个detail文件夹，只校验这里的内容，使用如下方案，不起作用。

    src/**/*
    !src/pages/detail/

多次尝试，只能如下一级一级去添加忽略方式

    src/api/*
    src/assets/*
    src/components/*
    src/pages/*

    # 要进行校验的文件
    !src/pages/detail/


### 规则文件说明

```.eslintrc.js```

    module.exports = {
    root: true,
    env: {
        "node": true,
    },
    extends: [
        'plugin:vue/recommended', // 已继承'eslint:recommended'
        '@vue/airbnb',
    ],
    parserOptions: {
        parser: 'babel-eslint',
    },
    rules: {
        // 闭合标签
        "vue/html-self-closing": ["error", {
        "html": {
            "void": "any",
            "normal": "any",
            "component": "always"
        },
        "svg": "always",
        "math": "always"
        }],
        // 最大每行最大属性个数
        "vue/max-attributes-per-line": ["error", {
        "singleline": 3,
        "multiline": {
            "max": 1,
            "allowFirstLine": false
        }
        }],
        // 无参数重新分配
        "no-param-reassign": ['error', {
        "props": true,
        "ignorePropertyModificationsFor": [
            'state', // for vuex state
            'vm', // for beforeRouter next(vm)
            'e' // for e.returnvalue
        ]
        }],
        // 禁止空块语句
        "no-empty": "warn",
        // 最大长度100
        "max-len": "warn",
        // 多行使用拖尾逗号
        "comma-dangle": ["error", "only-multiline"],
        // 连在一起时仅声明一个变量
        "one-var": "off",
        // 关闭转义校验
        "no-useless-escape": "off",
        // camelcase警告处理
        "camelcase": "off",
        // 父级变量声明阴影重叠
        "no-shadow": "off",
        'no-plusplus': [
        "error",
        { "allowForLoopAfterthoughts": true }
        ], // 允许在 for 循环的最后一个表达式中使用 ++ 和 --
        'arrow-parens': ["error", "as-needed"],
        'arrow-body-style': [
        "error",
        "as-needed",
        { "requireReturnForObjectLiteral": true }
        ], // 强制或禁止在箭头功能主体周围使用花括号
        'array-callback-return': 'off',
        "no-debugger": 'error', // 禁止使用debugger
        "no-constant-condition": 'error', // 禁止在条件中使用常量表达式
    }
    };


```.stylelintrc.js```

    module.exports = {
    defaultSeverity: "error",
    extends: ["stylelint-config-standard", "stylelint-config-recess-order"],
    plugins: [ "stylelint-scss" ],
    rules: {
        "at-rule-no-unknown": [true, {"ignoreAtRules" :[
        "mixin", "extend", "content"
        ]}],
        "color-hex-case": "upper", // 彩色十六进制大小写-大写
        "color-no-invalid-hex": true, // 颜色值不能为无效值
        "unit-case": "lower", // 单位小写
        "block-no-empty": true, // 不允许空块
        "shorthand-property-no-redundant-values": true, // 可简写的属性不重复写
        "no-descending-specificity": null, // 无下降特异性
        "block-opening-brace-space-after": "always-single-line", // 模块单行时“{”后空一格
        "block-opening-brace-space-before": "always", // “{”前空一格
        "block-closing-brace-space-before": "always-single-line", // 模块单行时“}”前空一格
        "font-family-no-missing-generic-family-keyword": null, // 字体系列没有缺少通用系列关键字
    }
    }




