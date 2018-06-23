---
title: Vue2.0
date: 2017-10-31 16:28:08
tags: vue
categories: FE
keywords: vue web 前端
---
>vue搭建后台管理页面(刚接触，踩了很多坑，好记性不如烂笔头，还是需要多多总结)

转载请注明出处

参考资料：
[Vue](https://cn.vuejs.org/v2/guide/installation.html)，[Vue-router](https://router.vuejs.org/zh-cn/)，[muse-ui](http://www.muse-ui.org/#/install)

# 用vue-cli脚手架创建一个基于webpack的Vue项目
环境：node
全局安装vue-cli脚手架：`npm install -g vue-cli`(安装过慢请尝试使用淘宝镜像)
新建文件夹后，命令行输入`vue init webpack 项目名`.
`npm install`安装依赖项，开发调试请使用`npm run dev`
`npm install --save muse-ui`安装页面插件
<!--  more  -->
# 使用muse-ui快速搭建页面
## 修改src目录下main.js
``` javascript
import Vue from 'vue'
import App from './App'
import MuseUI from 'muse-ui' //<--引入MuseUI
import router from './router'
import 'muse-ui/dist/muse-ui.css'//<--引入MuseUI主题
import 'muse-ui/dist/theme-carbon.css' // 使用 carbon 主题

Vue.use(MuseUI)//<--引入MuseUI

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: { App }
})

```
## components文件夹下新建home.vue,List.vue
引入muse-ui模板
### home.vue
由于要使用vue-router做路由内嵌套路由跳转，从官方issue中查到可以直接在标签添加`to="/"`实现
如：
`<mu-list-item title="Sent mail" to="/list" value="list1" >`
``` html
<template>
  <div class="layout">
    <div class="header">
      <div class="logo">
        Muse-UI
      </div>
      <div class="nav">
        <mu-tabs :value="activeTab" @change="handleTabChange" class="tab">
          <mu-tab value="tab1" title="Nav One"/>
          <mu-tab value="tab2" title="Nav TwO"/>
          <mu-tab value="tab3" title="Nav Three"/>
        </mu-tabs>
      </div>
    </div>
    <div class="content">
      <div class="content-left">
        <mu-list @change="handleListChange" :value="activeList">
          <mu-list-item title="Sent mail" to="/list" value="list1" ><!--vue路由跳转-->
            <mu-icon slot="left" value="send" />
          </mu-list-item>
          <mu-list-item title="Drafts" value="list2">
            <mu-icon slot="left" value="drafts"/>
          </mu-list-item>
          <mu-list-item title="Inbox" toggleNested value="list3">
            <mu-icon slot="left" value="inbox"/>
            <mu-list-item slot="nested" title="Starred" value="list4">
              <mu-icon slot="left" value="grade"/>
            </mu-list-item>
            <mu-list-item slot="nested" disabled title="Sent mail" toggleNested value="list5">
              <mu-icon slot="left" value="send"/>
              <mu-list-item title="Drafts" slot="nested" value="list6">
                <mu-icon slot="left" value="drafts"/>
              </mu-list-item>
            </mu-list-item>
            <mu-list-item slot="nested" title="Inbox" toggleNested value="list7">
              <mu-icon slot="left" value="inbox"/>
              <mu-list-item title="Drafts" slot="nested" value="list8">
                <mu-icon slot="left" value="drafts"/>
              </mu-list-item>
            </mu-list-item>
          </mu-list-item>
        </mu-list>
      </div>
      <div class="content-right">
        <div class="breadcrumb">
          <mu-breadcrumb>
            <mu-breadcrumb-item href="/">Home</mu-breadcrumb-item>
            <mu-breadcrumb-item href="/">VideoGame</mu-breadcrumb-item>
            <mu-breadcrumb-item>Download</mu-breadcrumb-item>
          </mu-breadcrumb>
        </div>
        <div class="body">
          <mu-sub-header>阳光</mu-sub-header>
          <mu-content-block>
            散落在指尖的阳光，我试着轻轻抓住光影的踪迹，它却在眉宇间投下一片淡淡的阴影。调皮的阳光掀动了四月的心帘，温暖如约的歌声渐起。似乎在诉说着，我也可以在漆黑的角落里，找到阴影背后的阳光，找到阳光与阴影奏出和谐的旋律。我要用一颗敏感赤诚的心迎接每一缕滑过指尖的阳光！
          </mu-content-block>
        </div>
      </div>
    </div>
    <div class="footer">
      Muse-UI ©2017 Created by Muse-UI
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      activeTab: 'tab1',
      activeList: 'list1'
    }
  },
  methods: {
    handleTabChange (val) {
      this.activeTab = val
    },
    handleListChange (val) {
      this.activeList = val
    }
  }
}
</script>
<style scoped>
.layout{
  background-color: rgb(236, 236, 236);
}

.header{
  background-color: #7e57c2;
}

.logo{
  font-size: 24px;
  color: white;
  display: inline-block;
  padding: 10px 20px;
}

.nav{
  display: inline-block;
  width: calc(100% - 150px);
  margin: 0 auto;
}

.tab{
  margin: 0 auto;
  width: 400px;
  background-color: rgba(0, 0, 0, 0);
}

.content{
  overflow: hidden;
}

.content-left{
  width: 30%;
  float: left;
  background-color: white;
  margin-bottom: -4000px;
  padding-bottom: 4000px;
}

.content-right{
  width: 70%;
  display: inline-block;
  float: right;
  padding: 10px 20px;
  background-color: rgba(0, 0, 0, 0)
}

.breadcrumb{
  margin: 10px 0;
}

.body{
  background-color: white;
  border-radius: 5px;
  min-height: 500px;
}

.footer{
  padding: 20px 0;
  text-align: center;
}
</style>
```
### List
``` html
<template>
<div>
  <mu-raised-button label="unselect" @click="unselect"/>
  <mu-table multiSelectable enableSelectAll ref="table">
    <mu-thead>
      <mu-tr>
        <mu-th>ID</mu-th>
        <mu-th>Name</mu-th>
        <mu-th>Status</mu-th>
      </mu-tr>
    </mu-thead>
    <mu-tbody>
      <mu-tr>
        <mu-td>1</mu-td>
        <mu-td>John Smith</mu-td>
        <mu-td>Employed</mu-td>
      </mu-tr>
      <mu-tr>
        <mu-td>2</mu-td>
        <mu-td>Randal White</mu-td>
        <mu-td>Unemployed</mu-td>
      </mu-tr>
      <mu-tr>
        <mu-td>3</mu-td>
        <mu-td>Stephanie Sanders</mu-td>
        <mu-td>Employed</mu-td>
      </mu-tr>
      <mu-tr>
        <mu-td>4</mu-td>
        <mu-td>Steve Brown</mu-td>
        <mu-td>Employed</mu-td>
      </mu-tr>
    </mu-tbody>
  </mu-table>
</div>
</template>
<script>
export default {
    methods: {
    unselect () {
      this.$refs.table.unSelectAll()
    }
  }
}
</script>
```

## 修改router文件夹下index.js
路由使用了vue得嵌套路由模板routes后内容为路由跳转逻辑
``` javascript
import Vue from 'vue'
import Router from 'vue-router'
import MuseUI from 'muse-ui'
import 'muse-ui/dist/muse-ui.css'
import 'muse-ui/dist/theme-carbon.css' // 使用 carbon 主题
import Lists from '@/components/List'//<--从components目录下引入模板页面
import Home from '@/components/home'//<--从components目录下引入模板页面

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'indexPage',
      component: Home,
      children:[
                {
                    path:'list',
                    name: 'list',
                    component: Lists,
                }
            ]
    },
    {
        path:'*',
        redirect: '/'
    }
  ]
})
```
目前页面暂时搭载完成，后期会继续更新

# 跨域问题
由于本地调试需要前后台关联所以在vue 搭建的目录里config中的index.js
``` javascript
dev: {
    env: require('./dev.env'),
    port: process.env.PORT || 8080,
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/users': {
        target:'http://127.0.0.1:3000',
        changeOrigin: true
      }
    }
```

`Access-Control-Allow-Origin`问题
在Express服务器文件夹里添加：
``` javascript
// var app = require('express')();
app.all("*", function (req, res, next) {
  res.header('Access-Control-Allow-Origin', '*');
  res.header("Access-Control-Allow-Headers", "Content-Type,Content-Length, Authorization, Accept,X-Requested-With");
  res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
  next();
});
```
# node获取axios post传值问题(express)
由于axios post传值方法是json 所以很多后台读取不到。查了很多资料。。 具体原因就是headers的方式问题(4种表头传值方法 看的有点蒙比，不过大概知道是什么问题了)
后来发现express其实提供了解决方法：'bodyParser'插件
用express搭建的话会生成如下代码:
``` javascript
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
```

具体到路由的话 直接使用res.body获取取值就可以了！

前台axios用qs
``` javascript
import axios from 'axios';
import qs from 'qs';//axios自带 不用在安装了

axios.post('http://127.0.0.1:3000/users/findAdmin',qs.stringify({ 
        'username': name ,
        'pwd': pwd
      }))
      .then((res)=>{
        if(res.data.rs.length === 0){
          alert("请输入正确的用户名");
        }else{
          alert(200);
          this.$router.push('/admin');
        }
        console.log(res);
      }).catch((res)=>{
        alert("系统错误");
      })
```
