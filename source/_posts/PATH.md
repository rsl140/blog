---
title: mac for command not found
date: 2017-05-09 15:49:57
tags: mac
categories: path
keywords: mac,path,环境变量
---
>装环境java环境变量后导致全局环境变量没有了，以下为解决方法

# 在命令行中输入

``` bash
    1.export PATH=/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:$PATH #这样可以保证命令行暂时可以使用。关闭终端后再使用就不行了。

    2.cd ~/ #进入当前用户的home目录

    3.touch .bash_profile # 创建bash_profile执行命令

    4.open .bash_profile # 打开并编辑bash_profile

    5.在打开的记事本中会显示之前配置的path 保存文档

    6.source .bash_profile # 更新命令 命令行(终端)要保持不要关闭

```
<!--  more  -->
