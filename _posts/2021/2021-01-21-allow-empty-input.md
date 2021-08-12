---
layout: post
title: " No files matching the pattern '/xxx/xxx/...' were found."
date: 2021-01-11
description: " No files matching the pattern '/xxx/xxx/...' were found."
tag: h5
---

使用.stylelintignore添加忽略文件校验，在`lint-staged`执行校验规范时，报错

```
✖ stylelint --fix:
Error: No files matching the pattern "/Users/lyc/Desktop/tal/monkey-wukong-wap/src/pages/reportActivity/assets/iconfont/iconfont.css" were found.
    at /Users/lyc/Desktop/tal/monkey-wukong-wap/node_modules/stylelint/lib/standalone.js:212:12
    at processTicksAndRejections (internal/process/task_queues.js:97:5)
```
解决方案：`lint-staged` stylelint添加`--allow-empty-input`
```
"lint-staged": {
  "*.{js,vue}": [
    "vue-cli-service lint"
  ],
  "*.{less,vue}": [
    "stylelint --allow-empty-input"
  ]
}
```

最终在 https://github.com/stylelint/stylelint/issues/4712 找到解决方案，

但是仍不是特别清楚具体原因以及什么时候添加这个`--allow-empty-input`属性。


