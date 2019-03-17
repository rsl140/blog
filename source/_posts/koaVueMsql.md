---
title: koaVueMsql
date: 2019-03-17 16:05:53
tags: vue mysql koa
categories: FE
keywords: vue mysql koa
---
>

# koa2+vue2+mysql 全栈开发记录

> 基于想要自己制作一个个人项目为由，于是有了这么一个开发记录（梳理开发过程也是一个知识巩固的过程）

## koa2+vue2+mysql 个人的一个通用DEMO(本篇文章的范例)
[koa2+vue2+mysql GITHUB地址](https://github.com/rsl140/FE-demo)

## 前端工具

- vue
- vue-router
- vuex
- axios
- element ui `页面UI组件`
- echartsjs `百度强大的图表展示`
- vue-admin-template `花裤衩大佬的一个实用管理后台模版` [配套教程](https://segmentfault.com/a/1190000009275424)
- vue-i18n `国际化`
- scss
<!--  more  -->

## 后端工具

- koa
- koa-bodyparser `解析 PUT / POST 请求中的 body`
- koa-convert
- koa-json
- koa-jwt `jwt鉴权`
- koa-logger
- koa-mysql-session
- koa-onerror
- koa-router
- koa-session-minimal
- koa-static
- koa-views
- koa2-cors `处理跨域`
- md5 `加密`
- moment `时间处理`
- mysql

## 前端篇
> 前端这边其实没什么好写的，主要是在`vue-admin-template`基础上做了一些修改

### src/utils/request.js的修改
-  request拦截器
``` js
  // request拦截器
  service.interceptors.request.use(
    config => {
      if (store.getters.token) {
        // config.headers['X-Token'] = getToken()
        // 因为是jwt方式鉴权 所以在header头中改为如下写法
        config.headers['Authorization'] = 'Bearer ' + getToken() // 让每个请求携带自定义token 请根据实际情况自行修改
      }
      return config
    },
    error => {
      // Do something with request error
      console.log(error) // for debug
      Promise.reject(error)
    }
  )
  ```
- response 拦截器
``` js
  // response 拦截器
  service.interceptors.response.use(
    response => {
      /**
      * code为非0是抛错 可结合自己业务进行修改
      */
      const res = response.data
      if (res.code !== 0) { // 因为后台返回值为0则是成功，所以将原来的20000改为了0
        Message({
          message: res.message,
          type: 'error',
          duration: 5 * 1000
        })

        // 70002:非法的token; 50012:其他客户端登录了;  50014:Token 过期了;
        if (res.code === 70002 || res.code === 50012 || res.code === 50014) {
          MessageBox.confirm(
            '你已被登出，可以取消继续留在该页面，或者重新登录',
            '确定登出',
            {
              confirmButtonText: '重新登录',
              cancelButtonText: '取消',
              type: 'warning'
            }
          ).then(() => {
            store.dispatch('FedLogOut').then(() => {
              location.reload() // 为了重新实例化vue-router对象 避免bug
            })
          })
        }
        return Promise.reject('error')
      } else {
        return response.data
      }
    },
    error => {
      console.log('err' + error) // for debug
      Message({
        message: error.message,
        type: 'error',
        duration: 5 * 1000
      })
      return Promise.reject(error)
    }
  )
```
### 封装了一个Echart组件
- 具体参考我的另外一个文章 [vue中使用echarts 使用记录](https://segmentfault.com/a/1190000017914552)

## 后端篇

### 构建项目目录

- 通过项目生成器生成[koa-generator](https://github.com/17koa/koa-generator)

1. `npm install -g koa-generator`
2. `koa2 /server && cd /server`
3. `npm install`

- 安装组件

1. `npm i jsonwebtoken koa-jwt koa-mysql-session koa-session-minimal koa2-cors md5 moment mysql save --save`

- 配置app.js

``` js
  const Koa = require('koa')
  const jwt = require('koa-jwt')
  const app = new Koa()
  const views = require('koa-views')
  const json = require('koa-json')
  const onerror = require('koa-onerror')
  const bodyparser = require('koa-bodyparser')
  const logger = require('koa-logger')
  const convert = require('koa-convert');

  var session = require('koa-session-minimal')
  var MysqlStore = require('koa-mysql-session')
  var config = require('./config/default.js')
  var cors = require('koa2-cors')

  const users = require('./routes/users')
  const account = require('./routes/account')

  // error handler
  onerror(app)

  // 配置jwt错误返回
  app.use(function(ctx, next) {
    return next().catch(err => {
      if (401 == err.status) {
        ctx.status = 401
        ctx.body = ApiErrorNames.getErrorInfo(ApiErrorNames.INVALID_TOKEN)
        // ctx.body = {
        //   // error: err.originalError ? err.originalError.message : err.message
        // }
      } else {
        throw err
      }
    })
  })

  // Unprotected middleware
  app.use(function(ctx, next) {
    if (ctx.url.match(/^\/public/)) {
      ctx.body = 'unprotected\n'
    } else {
      return next()
    }
  })

  // Middleware below this line is only reached if JWT token is valid

  app.use(
    jwt({ secret: config.secret, passthrough: true }).unless({
      path: [/\/register/, /\/user\/login/]
    })
  )

  // middlewares
  app.use(convert(bodyparser({
    enableTypes:['json', 'form', 'text']
  })))
  app.use(convert(json()))
  app.use(convert(logger()))
  app.use(require('koa-static')(__dirname + '/public'))

  app.use(views(__dirname + '/views', {
    extension: 'pug'
  }))

  // logger
  app.use(async (ctx, next) => {
    const start = new Date()
    await next()
    const ms = new Date() - start
    console.log(`${ctx.method} ${ctx.url} - ${ms}ms`)
  })

  // cors
  app.use(cors())

  // routes
  app.use(users.routes(), users.allowedMethods())
  app.use(account.routes(), account.allowedMethods())

  // error-handling
  app.on('error', (err, ctx) => {
    console.error('server error', err, ctx)
  });

  module.exports = app

```

- 新建config文件夹用于存放数据库连接等操作
> default.js
``` js
// 数据库配置
  const config = {
    port: 3000,
    database: {
      DATABASE: 'xxx', //数据库
      USERNAME: 'root', //用户
      PASSWORD: 'xxx', //密码
      PORT: '3306', //端口
      HOST: '127.0.0.1' //服务ip地址
    },
    secret: 'jwt_secret'
  }

  module.exports = config

```
### 数据库相关(mysql)
> createTables.js 一个简单的用户角色权限表 [用户、角色、权限表的关系(mysql)](https://blog.csdn.net/harbor1981/article/details/78149203)
``` js
// 数据库表格创建
const createTable = {
  users: `CREATE TABLE IF NOT EXISTS user_info (
      id INT PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '(自增长)',
      user_id VARCHAR ( 100 ) NOT NULL COMMENT '账号',
      user_name VARCHAR ( 100 ) NOT NULL COMMENT '用户名',
      user_pwd VARCHAR ( 100 ) NOT NULL COMMENT '密码',
      user_head VARCHAR ( 225 ) COMMENT '头像',
      user_mobile VARCHAR ( 20 ) COMMENT '手机',
      user_email VARCHAR ( 64 ) COMMENT '邮箱',
      user_creatdata TIMESTAMP NOT NULL DEFAULT NOW( ) COMMENT '注册日期',
      user_login_time TIMESTAMP DEFAULT NOW( ) COMMENT '登录时间',
      user_count INT COMMENT '登录次数'
    ) ENGINE = INNODB charset = utf8;`,
    role: `CREATE TABLE IF NOT EXISTS role_info (
      id INT PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '(自增长)',
      role_name VARCHAR ( 20 ) NOT NULL COMMENT '角色名',
      role_description VARCHAR ( 255 ) DEFAULT NULL COMMENT '描述'
    ) ENGINE = INNODB charset = utf8;`,
    permission: `CREATE TABLE IF NOT EXISTS permission_info (
      id INT PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '(自增长)',
      permission_name VARCHAR ( 20 ) NOT NULL COMMENT '权限名',
      permission_description VARCHAR ( 255 ) DEFAULT NULL COMMENT '描述'
    ) ENGINE = INNODB charset = utf8;`,
    userRole: `CREATE TABLE IF NOT EXISTS user_role (
      id INT PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '(自增长)',
      user_id INT NOT NULL COMMENT '关联用户',
      role_id INT NOT NULL COMMENT '关联角色',
      KEY fk_user_role_role_info_1 ( role_id ),
      KEY fk_user_role_user_info_1 ( user_id ),
      CONSTRAINT fk_user_role_role_info_1 FOREIGN KEY ( role_id ) REFERENCES role_info ( id ) ON DELETE CASCADE ON UPDATE CASCADE,
      CONSTRAINT fk_user_role_user_info_1 FOREIGN KEY ( user_id ) REFERENCES user_info ( id ) ON DELETE CASCADE ON UPDATE CASCADE
    ) ENGINE = INNODB charset = utf8;`,
    rolePermission: `CREATE TABLE IF NOT EXISTS role_permission (
      id INT PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '(自增长)',
      role_id INT NOT NULL COMMENT '关联角色',
      permission_id INT NOT NULL COMMENT '关联权限',
      KEY fk_role_permission_role_info_1 ( role_id ),
      KEY fk_role_permission_permission_info_1 ( permission_id ),
      CONSTRAINT fk_role_permission_role_info_1 FOREIGN KEY ( role_id ) REFERENCES role_info ( id ) ON DELETE CASCADE ON UPDATE CASCADE,
      CONSTRAINT fk_role_permission_permission_info_1 FOREIGN KEY ( permission_id ) REFERENCES permission_info ( id ) ON DELETE CASCADE ON UPDATE CASCADE
    ) ENGINE = INNODB charset = utf8;`
}

module.exports = createTable
```

- 创建lib文件夹 用于存储数据库查询语句
> mysql.js
``` js
const mysql = require('mysql')
const config = require('../config/default')
const createTables = require('../config/createTables.js')

var pool = mysql.createPool({
  host: config.database.HOST,
  user: config.database.USERNAME,
  password: config.database.PASSWORD,
  database: config.database.DATABASE
})

let query = function(sql, values) {
  return new Promise((resolve, reject) => {
    pool.getConnection(function(err, connection) {
      if (err) {
        resolve(err)
      } else {
        connection.query(sql, values, (err, rows) => {
          if (err) {
            reject(err)
          } else {
            resolve(rows)
          }
          connection.release()
        })
      }
    })
  })
}

let createTable = function(sql) {
  return query(sql, [])
}

// 建表
// createTable(createTables.users)
// createTable(createTables.role)
// createTable(createTables.permission)
// createTable(createTables.userRole)
// createTable(createTables.rolePermission)

// 查询用户是否存在
let findUser = async function(id) {
  let _sql = `
        SELECT * FROM user_info where user_id="${id}" limit 1;
    `
  let result = await query(_sql)

  if (Array.isArray(result) && result.length > 0) {
    result = result[0]
  } else {
    result = null
  }
  return result
}
// 查询用户以及用户角色
let findUserAndRole = async function(id) {
  let _sql = `
      SELECT u.*,r.role_name FROM user_info u,user_role ur,role_info r where u.id=(SELECT id FROM user_info where user_id="${id}" limit 1) and ur.user_id=u.id and r.id=ur.user_id limit 1;
    `
  let result = await query(_sql)

  if (Array.isArray(result) && result.length > 0) {
    result = result[0]
  } else {
    result = null
  }
  return result
}

// 更新用户登录次数和登录时间
let UpdataUserInfo = async function(value) {
  let _sql =
    'UPDATE user_info SET user_count = ?, user_login_time = ? WHERE id = ?;'
  return query(_sql, value)
}

module.exports = {
  //暴露方法
  createTable,
  findUser,
  findUserAndRole,
  UpdataUserInfo,
  getShopAndAccount
}

```
### koa 路由配置
- 接口报错信息统一方法 创建error文件夹
> ApiErrorNames.js
``` js
/**
 * API错误名称
 */
var ApiErrorNames = {};

ApiErrorNames.UNKNOW_ERROR = "UNKNOW_ERROR";
ApiErrorNames.SUCCESS = "SUCCESS";

/* 参数错误：10001-19999 */
ApiErrorNames.PARAM_IS_INVALID = 'PARAM_IS_INVALID';
ApiErrorNames.PARAM_IS_BLANK = 'PARAM_IS_BLANK';
ApiErrorNames.PARAM_TYPE_BIND_ERROR = 'PARAM_TYPE_BIND_ERROR';
ApiErrorNames.PARAM_NOT_COMPLETE = 'PARAM_NOT_COMPLETE';

/* 用户错误：20001-29999*/
ApiErrorNames.USER_NOT_LOGGED_IN = 'USER_NOT_LOGGED_IN';
ApiErrorNames.USER_LOGIN_ERROR = 'USER_LOGIN_ERROR';
ApiErrorNames.USER_ACCOUNT_FORBIDDEN = 'USER_ACCOUNT_FORBIDDEN';
ApiErrorNames.USER_NOT_EXIST = 'USER_NOT_EXIST';
ApiErrorNames.USER_HAS_EXISTED = 'USER_HAS_EXISTED';

/* 业务错误：30001-39999 */
ApiErrorNames.SPECIFIED_QUESTIONED_USER_NOT_EXIST = 'SPECIFIED_QUESTIONED_USER_NOT_EXIST';

/* 系统错误：40001-49999 */
ApiErrorNames.SYSTEM_INNER_ERROR = 'SYSTEM_INNER_ERROR';

/* 数据错误：50001-599999 */
ApiErrorNames.RESULE_DATA_NONE = 'RESULE_DATA_NONE';
ApiErrorNames.DATA_IS_WRONG = 'DATA_IS_WRONG';
ApiErrorNames.DATA_ALREADY_EXISTED = 'DATA_ALREADY_EXISTED';

/* 接口错误：60001-69999 */
ApiErrorNames.INTERFACE_INNER_INVOKE_ERROR = 'INTERFACE_INNER_INVOKE_ERROR';
ApiErrorNames.INTERFACE_OUTTER_INVOKE_ERROR = 'INTERFACE_OUTTER_INVOKE_ERROR';
ApiErrorNames.INTERFACE_FORBID_VISIT = 'INTERFACE_FORBID_VISIT';
ApiErrorNames.INTERFACE_ADDRESS_INVALID = 'INTERFACE_ADDRESS_INVALID';
ApiErrorNames.INTERFACE_REQUEST_TIMEOUT = 'INTERFACE_REQUEST_TIMEOUT';
ApiErrorNames.INTERFACE_EXCEED_LOAD = 'INTERFACE_EXCEED_LOAD';

/* 权限错误：70001-79999 */
ApiErrorNames.PERMISSION_NO_ACCESS = 'PERMISSION_NO_ACCESS';
ApiErrorNames.INVALID_TOKEN = 'INVALID_TOKEN';

/**
 * API错误名称对应的错误信息
 */
const error_map = new Map();

error_map.set(ApiErrorNames.SUCCESS, { code: 0, message: '成功' });
error_map.set(ApiErrorNames.UNKNOW_ERROR, { code: -1, message: '未知错误' });
/* 参数错误：10001-19999 */
error_map.set(ApiErrorNames.PARAM_IS_INVALID, { code: 10001, message: '参数无效' });
error_map.set(ApiErrorNames.PARAM_IS_BLANK, { code: 10002, message: '参数为空' });
error_map.set(ApiErrorNames.PARAM_TYPE_BIND_ERROR, { code: 10003, message: '参数类型错误' });
error_map.set(ApiErrorNames.PARAM_NOT_COMPLETE, { code: 10004, message: '参数缺失' });
/* 用户错误：20001-29999*/
error_map.set(ApiErrorNames.USER_NOT_LOGGED_IN, { code: 20001, message: '用户未登录' });
error_map.set(ApiErrorNames.USER_LOGIN_ERROR, { code: 20002, message: '账号不存在或密码错误' });
error_map.set(ApiErrorNames.USER_ACCOUNT_FORBIDDEN, { code: 20003, message: '账号已被禁用' });
error_map.set(ApiErrorNames.USER_NOT_EXIST, { code: 20004, message: '用户不存在' });
error_map.set(ApiErrorNames.USER_HAS_EXISTED, { code: 20005, message: '用户已存在' });
/* 业务错误：30001-39999 */
error_map.set(ApiErrorNames.SPECIFIED_QUESTIONED_USER_NOT_EXIST, { code: 30001, message: '某业务出现问题' });
/* 系统错误：40001-49999 */
error_map.set(ApiErrorNames.SYSTEM_INNER_ERROR, { code: 40001, message: '系统繁忙，请稍后重试' });
/* 数据错误：50001-599999 */
error_map.set(ApiErrorNames.RESULE_DATA_NONE, { code: 50001, message: '数据未找到' });
error_map.set(ApiErrorNames.DATA_IS_WRONG, { code: 50002, message: '数据有误' });
error_map.set(ApiErrorNames.DATA_ALREADY_EXISTED, { code: 50003, message: '数据已存在' });
/* 接口错误：60001-69999 */
error_map.set(ApiErrorNames.INTERFACE_INNER_INVOKE_ERROR, { code: 60001, message: '内部系统接口调用异常' });
error_map.set(ApiErrorNames.INTERFACE_OUTTER_INVOKE_ERROR, { code: 60002, message: '外部系统接口调用异常' });
error_map.set(ApiErrorNames.INTERFACE_FORBID_VISIT, { code: 60003, message: '该接口禁止访问' });
error_map.set(ApiErrorNames.INTERFACE_ADDRESS_INVALID, { code: 60004, message: '接口地址无效' });
error_map.set(ApiErrorNames.INTERFACE_REQUEST_TIMEOUT, { code: 60005, message: '接口请求超时' });
error_map.set(ApiErrorNames.INTERFACE_EXCEED_LOAD, { code: 60006, message: '接口负载过高' });
/* 权限错误：70001-79999 */
error_map.set(ApiErrorNames.PERMISSION_NO_ACCESS, { code: 70001, message: '无访问权限' });
error_map.set(ApiErrorNames.INVALID_TOKEN, { code: 70002, message: '无效token' });

//根据错误名称获取错误信息
ApiErrorNames.getErrorInfo = (error_name) => {

    var error_info;

    if (error_name) {
        error_info = error_map.get(error_name);
    }

    //如果没有对应的错误信息，默认'未知错误'
    if (!error_info) {
        error_name = UNKNOW_ERROR;
        error_info = error_map.get(error_name);
    }

    return error_info;
}

//返回正确信息
ApiErrorNames.getSuccessInfo = (data) => {

    var success_info;
    let name = 'SUCCESS';
    success_info = error_map.get(name);
    if (data) {
        success_info.data = data
    }

    return success_info;
}

module.exports = ApiErrorNames;
```
- 创建controller文件夹
> 对路由users进行逻辑编写
``` js
const mysqlModel = require('../lib/mysql') //引入数据库方法
const jwt = require('jsonwebtoken')
const config = require('../config/default.js')
const ApiErrorNames = require('../error/ApiErrorNames.js')
const moment = require('moment')

/**
 * 普通登录
 */
exports.login = async (ctx, next) => {
  const { body } = ctx.request
  try {
    const user = await mysqlModel.findUser(body.username)
    if (!user) {
      // ctx.status = 401
      ctx.body = ApiErrorNames.getErrorInfo(ApiErrorNames.USER_NOT_EXIST)
      return
    }
    let bodys = await JSON.parse(JSON.stringify(user))
    // 匹配密码是否相等
    if ((await user.user_pwd) === body.password) {
      let data = {
        user: user.user_id,
        // 生成 token 返回给客户端
        token: jwt.sign(
          {
            data: user.user_id,
            // 设置 token 过期时间
            exp: Math.floor(Date.now() / 1000) + 60 * 60 // 60 seconds * 60 minutes = 1 hour
          },
          config.secret
        )
      }
      ctx.body = ApiErrorNames.getSuccessInfo(data)
    } else {
      ctx.body = ApiErrorNames.getErrorInfo(ApiErrorNames.USER_LOGIN_ERROR)
    }
  } catch (error) {
    ctx.throw(500)
  }
}

/**
 * 获取用户信息
 */
exports.info = async (ctx, next) => {
  const { body } = ctx.request
  // console.log(body)
  try {
    const token = ctx.header.authorization
    let payload
    if (token) {
      payload = await jwt.verify(token.split(' ')[1], config.secret) // 解密，获取payload
      const user = await mysqlModel.findUserAndRole(payload.data)
      if (!user) {
        ctx.body = ApiErrorNames.getErrorInfo(ApiErrorNames.USER_NOT_EXIST)
      } else {
        let cont = user.user_count + 1
        let updateInfo = [
          cont,
          moment().format('YYYY-MM-DD HH:mm:ss'),
          user.id
        ]
        await mysqlModel
        .UpdataUserInfo(updateInfo)
        .then(res => {
          let data = {
            avatar: 'https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif',
            name: user.user_id,
              // roles: [user.user_admin === 0 ? 'admin' : '']
            roles: [user.role_name]
          }
          ctx.body = ApiErrorNames.getSuccessInfo(data)
        })
        .catch(err => {
          ctx.body = ApiErrorNames.getErrorInfo(ApiErrorNames.DATA_IS_WRONG)
        })
      }
    } else {
      ctx.body = ApiErrorNames.getErrorInfo(ApiErrorNames.INVALID_TOKEN)
    }
  } catch (error) {
    ctx.throw(500)
  }
}

/**
 * 退出登录
 */
exports.logout = async (ctx, next) => {
  try {
    // ctx.status = 200
    ctx.body = ApiErrorNames.getSuccessInfo()
  } catch (error) {
    ctx.throw(500)
  }
}

```

> routes中的users.js
``` js
const router = require('koa-router')() //引入路由函数
const userControl = require('../controller/users') //引入逻辑
// const config = require('../config/default.js')

router.get('/', async (ctx, next) => {
  'use strict'
  ctx.redirect('/user/login')
})
// 路由中间间，页面路由到／，就是端口号的时候，（网址），页面指引到／/user/login

router.get('/user/info', userControl.info)
router.post('/user/logout', userControl.logout)
router.post('/user/login', userControl.login)

module.exports = router
//将页面暴露出去
```
### 备注
`ctx.request` 获取post请求中的body
`ctx.query.xx` 获取get请求中的参数
`router.prefix('/account')` 给router实例添加前缀
