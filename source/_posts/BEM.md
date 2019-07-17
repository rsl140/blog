---
title: 代码规范备忘
date: 2019-07-17 11:01:58
tags: FE
categories:
keywords: 代码规范
---
> 提高代码质量、统一代码规范

[参考资料](https://juejin.im/post/5d1c6550518825330a3bfa01)
<!--  more  -->

## vue
### script 标签内部声明顺序
``` bash
data > prop > components > filter > computed >  watch > beforeCreate > created > beforeMount > mounted > beforeUpdate > updated > beforeDestroy > destroyed > methods
```

### vscode 使用vscode-stylelint
安装
``` bash
npm install --save-dev
stylelint
stylelint-config-recommended
stylelint-config-standard
stylelint-order
prettier-stylelint #vscode 设置中设置"prettier.stylelintIntegration": true 自动格式化
```
根目录下 vi .stylelintrc
```
{
  "extends": "stylelint-config-standard",
  "plugins": [
      "stylelint-order",
      "stylelint-scss"
  ],
  "rules": {
    "block-no-empty": null,
    "color-no-invalid-hex": true,
    "comment-empty-line-before": [ "always", {
       "ignore": ["stylelint-commands", "after-comment"]
    }],
    "declaration-colon-space-after": "always",
    "indentation": [2, {
      "except": ["value"]
    }],
    "at-rule-no-unknown": null,
    "scss/at-rule-no-unknown": true,
    "max-empty-lines": 4,
    "rule-empty-line-before": [ "always", {
      "except": ["first-nested"],
      "ignore": ["after-comment"]
    } ],
    "unit-whitelist": ["px", "em", "rem", "%", "s", "vh", "vw"]
  }
}


```
设置中配置
```
    "css.validate": false,
    "less.validate": false,
    "scss.validate": false
```