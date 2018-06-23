---
title: hexo draft 草稿
date: 2017-04-11 18:09:35
tags: markdown
categories: 文本编辑
keywords: hexo,草稿
---
>想写些东西，但是暂时不想发布，就找到了用这种方法来保存

创建草稿的方法是
``` bash
    hexo new draft name #name为文档名
```
创建好文档后存储的目录为:`source/_drafts/` 使用`hexo g`时，草稿不会被创建。

想要查看草稿文章的话可使用`hexo s --draft` 或者在 `_config.yml`文件中进行配置`render_drafts: true`,本地启动时会渲染草稿.

发布草稿使用如下方法
``` bash    
    hexo publish name #name为草稿.md的文件名
```

<!--  more  -->

