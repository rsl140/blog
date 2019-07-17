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