---
title: 本地开发公众号(测试号)
date: 2017-10-23 11:48:08
tags: 微信开发
categories: wechat
keywords: 公众号 微信
---
>微信开发全过程(node)

# 本地进行微信公众号的开发和调试

最开始是放在个人服务器上的。但是每次测试时部署麻烦，后来尝试使用[natapp.cn](https://natapp.cn)通过内网穿透实现本地测试。

注册后，可以尝试先使用免费的。[网站自带教程点击这里](https://natapp.cn/article/natapp_newbie)

由于我是mac电脑 使用natapp需要先授权`chmod a+x natapp`然后运行`./natapp`(推荐使用`config.ini`方式，配置好`authtoken`，放在同级目录下即可)

链接时遇到一直链接不上服务器的问题，仔细看了下教程，应该是DNS问题，修改本地DNS为阿里云公共DNS：`223.5.5.5` `223.6.6.6`后，成功链接。

# 账号链接(测试号)

## 微信接口配置信息

URL：填写服务器地址(需有appid信息相符才能验证成功)
Token：任意填写(后面的配置需与填写的一致)

## 创建项目
开发语言：node.js
开发框架：express，express教程参考[官网](http://expressjs.com/)
插件：[wechat](https://github.com/node-webot/wechat)、[wechat-api](https://github.com/node-webot/wechat-api)

## 建立接口链接
### 创建路由
<!--  more  -->
由于是在express基础上编程，首先在routes文件夹下添加wechat.js路由文件
``` javascript
var express = require('express');
var router = express.Router();
var wechat = require('wechat');//<---引入wechat插件

var config = {
  token : 'you token',
  appid : 'you appid',
  appsecret :'you appsecret',
  encodingAESKey : ''
};

router.use(express.query());

router.use('/', wechat(config, function(req, res, next) {
    console.log(req.weixin);
    var message = req.weixin;
    //文本
    if (message.Content === '1') {

        res.reply('hehe');
    }

}));

module.exports = router;
```
### app.js修改
``` javascript
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');

var index = require('./routes/index');
var users = require('./routes/users');
var wechat = require('./routes/wechat');// <-- 增加wechat地址

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
// app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', index);
app.use('/users', users);
// 调用微信接口
app.use('/wechat', wechat);// <-- 调用微信接口

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```
改造完成后运行程序，建立连接。配置完成。

# 菜单创建(wechar-api使用)
## 根目录下创建config.json文件
存储基本数据<span style="color: red;">（注：注释内容务必删除，json不支持注释功能）</span>
``` json
{
    "token":"youtoken",
    "appID": "youappID",
    "appSecret": "youappSecret",
    "encodingAESKey": "",
      "menu": {// 菜单内容，微信详细数据参考管方文档 
        "button": [
            {
                "type": "click",
                "name": "1",
                "key": "V1001_TODAY_MUSIC"
            },
            {
              "name":"2",
              "sub_button": [
                  {
                    "type": "click",
                    "name": "2.1",
                    "key": "V1001_TODAY_SHARE"
                  },
                  {
                    "type": "click",
                    "name": "2.2",
                    "key": "V1001_STU_SHARE"
                  }
              ]
            },
            {
                "type": "click",
                "name": "3",
                "key": "V1001_MY_STUDAY"
            }
        ]
    }
}
```

## 改造wechat.js
``` javascript
var express = require('express');  
var router = express.Router();
// 引入微信插件
var wechat = require('wechat');
var WechatAPI = require('wechat-api');//<--调用wechat-api
// 引入config
var configApi = require('../config.json');//<--调用config
  
var config = configApi;
  
router.use(express.query());
// use wechat-api
var api = new WechatAPI(config.appID, config.appSecret);//<--创建api实例

var menu = configApi.menu;//<--提取config中的菜单内容
  
router.use('/', wechat(config, function(req, res, next) {  
  console.log(req.weixin);  
  var message = req.weixin;  
  //文本  
  if (message.Content === '1') {  

      res.reply('hehe');  
  }
  // 第一次关注发送欢迎信息
  if(message.MsgType === 'event' && message.Event === 'subscribe') {

    res.reply('Hello world');
  }
}));
// 创建微信菜单
api.createMenu(menu, function(err, result){
     console.log(result);
     console.log(err);
    });
module.exports = router;
```

## 项目目录
![项目目录](https://user-images.githubusercontent.com/7655291/32205958-4ff84272-bdc0-11e7-97d9-fcb2c23dccba.jpg)

