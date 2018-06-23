---
title: mac 命令行记录
date: 2017-05-24 9:23:32
tags:
categories:
keywords:
---
>命令行笔记

# how to exit vim？据说有1kk+的人搜索

Hit the Esc key; vim goes into command mode. Then you can type

`:q` to quit (short for :quit) 退出
`:q!` to quit without saving (short for :quit!) 不保存退出
`:wq` to write and quit (think write and quit) 编辑退出
`:wq!` to write and quit even if file has only read permission (if file does not have write permission: force write) 只读模式编辑退出
`:x` to write and quit (similar to :wq, but won’t write if there are no changes)
`:qa` to quit all (short for :quitall)
When you press :, a : will appear at the bottom of the screen.

Or you can press Esc `ZZ` (Esc `Shift+Z` `Shift+Z`) to write/save if the file was changed, then quit.

Or if you don’t want to save changes you can use ZQ instead.

Vim has extensive help, so type Esc:`help Return` and you will have all your answers and even a neat tutorial.

来源：How to exit the Vim editor?
<!--  more  -->
# 查看端口

``` 
    netstat -antl| grep 8082 //监听端口是否在使用
```

# Tomcat启动

找到bin目录下使用：
```
    ./startup.sh //启动
    ./shutdown.sh //关闭
```

# 新建文件
``` bash
touch 123.txt
```
