---
title: Contributions不显示(git)
date: 2017-06-20 12:23:00
tags:
categories:
keywords:
---
>当我换到MAC得时候重新配置了git之后 Contributions 不显示了 后来搜了很多方法，今天终于解决。

## 原因：建立用户的时候用错了email 和github关联的email不一样 导致了Contributions不显示，值显示了新建repo的记录
进入repo 查看git log 日志发现 提交邮箱和关联邮箱不一致。

## 方法一：添加这个邮箱到自己的github账户中
进入自己的设置里 email选项中新增log下的邮箱。（这时会收到一个验证邮件，点击确认。）
返回主页后回发现Contributions出现了。

## 方法二：修改提交记录中的邮箱为已被关联的邮箱(个人不太建议)

<!--  more  -->

## 如何正确设置你的 git 个人信息
接下来全局设置好你的正确信息，以后就放心的用Github进行版本管理

```
    git config --global user.email "你的邮件地址"
    git config --global user.name "你的Github用户名"
```
