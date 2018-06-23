---
title: Hexo&常见问题
date: 2017-03-23
categories: FE
tags: hexo
---
Hexo官网: [Hexo](https://hexo.io/)

## 搭建本地博客

### 环境配置

``` bash
	 git，node
```
### 安装hexo

``` bash
	npm install hexo-cli -g
	hexo init #初始化
	npm install
	hexo server (hexo s) #本地运行默认4000端口
```
<!-- more -->
### 使用
缩写：
``` bash
	hexo c #缓存清理
	hexo g #生成静态页面
	hexo d #发布到github
	hexo s #本地运行
```
### push 到github
找到_config.yml文件，翻到最下面 ：后面都要有空格

``` bash
	deploy:
		type: git
		repository: https://github.com/rsl140/blog.git
		branch: master
	执行
		npm install hexo-deployer-git --save
		hexo d
```
### 常用命令

``` bash
	hexo new "postName" #新建文章
	hexo new page "pageName" #新建页面
	hexo generate #生成静态页面至public目录
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
	hexo deploy #将.deploy目录部署到GitHub
	hexo help  #查看帮助
	hexo version  #查看Hexo的版本
```
### 主题

``` bash
	在根目录下 git clone https://github.com/iissnan/hexo-theme-next themes/next #其他类似
	#启用
	#修改博客目录D:\hexo\_config.yml中的theme属性，将其设置为jacman。
	theme: next
	#在主题更新之前，一定要备份好主题目录下的_config.yml文件，尤其是到后面修改了很多配置之后。
	cd themes/next
	git pull origin master
```
#### 主题的配置
　　主题的配置在D:\hexo\themes\next\_config.yml文件里面了。
``` bash
menu:  #导航栏
  主页: /
  归档: /archives
  关于: /about

#导航栏上如何添加新页面呢？输入下面命令，新建一个页面，在里添加内容即可。
hexo new page "about"
#导航栏里的文件都在D:\Hexo\source\目录下在相应的文件里修改，然后预览再部署。注意，修改任何的配置内容，都需要生成之后才有效。
```
### 本站使用的第三方配置

数据统计：[不蒜子统计](http://theme-next.iissnan.com/third-party-services.html#analytics-tencent)

评论系统：[DISQUS](http://theme-next.iissnan.com/third-party-services.html#disqus)

### 标签(tag)页面创建

``` bash
	hexo new page tags #新建页面

	type: "tags" #在页面中设置type标签
	comments: false #禁用评论功能，如果有

	menu: #在配置文件中添加tags
	  home: /
	  tags: /tags
#分类页面同理 categories
```

### 修改文章范本

范本文档目录为：scaffolds/post.md
草稿文本目录为：scaffolds/draft.md

``` bash
	title:  #文章标题
	date:  #时间，一般不用改
	categories:  #目录分类
	tags:  #标签，格式可以是[Hexo,总结]，中间用英文逗号分开
	keywords:  #文章关键词，多个关键词用英文逗号隔开
```

### 我遇到的错误解决方法

#### github不显示页面的问题
在配置里的url如下设置

``` bash
	url: https://rsl140.github.io/blog/m
	root: /blog #子文件夹目录名
```

#### hexo 无法生成新文章
``` bash
	安装hexo步骤从新走一次
	提示hexo不是内部执行命令时 执行
	sudo npm install -g hexo
```
#### site设置
``` bash
	# Site
	title: XiaoSi
	subtitle:
	description: 小思
	author: Summer
	language: zh-Hans #中文
	timezone:   Asia/Shanghai #北京时间
```
#### github备份

``` bash
//假设hexo文件夹是已经生成的hexo博客目录
//如果themes/next(风格名字目录)下面有.git，请删除这个.git文件夹。
#cd hexo
#git init  //初始化本地仓库
#git add source themes scaffolds _config.yml package.json package-lock.json  //将必要的文件依次添加
#git commit -m "blog hexo"
#git branch hexo  //新建hexo分支
#git checkout hexo  //切换到hexo分支上
#git remote add origin git@github.com:user/user.github.io.git  //将本地与Github项目对接
#git push origin hexo  //push到Github项目的hexo分支上

其他终端使用：

> 前提:nodejs,git,hexo已经安装好，并配置好环境变量。

``` bash
#git clone -b hexo git@github.com:user/user.github.io.git  //将Github中hexo分支clone到本地
#cd user.github.io
#npm install
```
[参考原文](https://blog.csdn.net/shile/article/details/78714189)
```