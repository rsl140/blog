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


# 文件命名
- 文件名不能含有空格
- 文件名建议只使用小写字母，不使用大写字母。（但比如说像github的说明类文件，README，则应该全部使用大写）
- 文件名包含多个单词时，单词之间建议使用半角的连词线 ( - ) 分隔。

```
// 反例
├── index.html
├── main.js
└── components
    ├── pageheader
    ├── pagearticle
    └── pageheader

// 正例
├── index.html
├── main.js
└── components
    ├── page-header
    ├── page-article
    └── page-header
```


# 变量
变量命名：
- 命名方式：小驼峰式命名方法
- 命名规范：类型+对象描述的方式，如果没有明确的类型，就可以使前缀为名词
-

javascript变量命名类型 | 变量命名前缀
---|---
array数组| a
boolean布尔值| b
float浮点数| l
function 函数| fn
int 整型| i
object 对象| o
regular 正则| r
string 字符串| s

```
var aName = [1, 2, 3];
var oBtn = document.getElementById('btn');
function fnName(){};
var iCount = 0;
var sName = "zhuyujia";
```
# 函数

动词 | 含义 | 返回值
--- |---|---
can | 判断是否可执行某个动作 ( 权限 ) | 函数返回一个布尔值 true：可执行；false：不可执行
has | 判断是否含有某个值 | 函数返回一个布尔值 true：含有此值；false：不含有此值
is | 判断是否为某个值 | 函数返回一个布尔值 true：为某个值；false：不为某个值
get | 获取某个值 | 函数返回一个非布尔值
set | 设置某个值 | 无返回值、返回是否设置成功或者返回链式对象

```
//是否可阅读
function canRead() {
    return true;
}
//获取姓名
function getName {
    return this.name;
}
```

# 类的成员

# 注释规范

## 单行注释

- 单独一行：//(双斜线)与注释文字之间保留一个空格
- 在代码后面添加注释：//(双斜线)与代码之间保留一个空格，并且//(双斜线)与注释文字之间保留一个空格。
- 注释代码：//(双斜线)与代码之间保留一个空格。

```
// 调用了一个函数；1)单独在一行
setTitle();
var maxCount = 10; // 设置最大量；2)在代码后面注释
// setName(); // 3)注释代码
```

## 多行注释

- 若开始(/* 和结束 / )都在一行，推荐采用单行注释
- 若至少三行注释时，第一行为/，最后行为/，其他行以开始，并且注释文字与*保留一个空格。

```
/*
 * 代码执行到这里后会调用setTitle()函数
 * setTitle()：设置title的值
 */
setTitle();
```

## 函数（方法）注释


注释名| 语法 | 含义 | 示例
---|---|---|---
@param | @param参数名 {参数类型} 描述信息 | 描述参数的信息 | @param name {String} 传入名称
@return | @return{返回类型} 描述信息 | 描述返回值的信息 | @return {Boolean} true:可执行;false:不可执行
@author | @author作者信息 [附属信息：如邮箱、日期] | 描述此函数作者的信息 | @author张三 2019/06/01
@version | @versionXX.XX.XX | 描述此函数的版本号 | @version 1.03
@example | @example示例代码 | @example setTitle('测试') | 如下

```
/**
 - 合并Grid的行
 - [@param](/user/param) grid {Ext.Grid.Panel} 需要合并的Grid
 - [@param](/user/param) cols {Array} 需要合并列的Index(序号)数组；从0开始计数，序号也包含。
 - [@param](/user/param) isAllSome {Boolean} ：是否2个tr的cols必须完成一样才能进行合并。true：完全一样；false(默认)：不完全一样
 - [@return](/user/return) void
 - [@author](/user/author) polk6 2019/06/01
 - [@example](/user/example)
 - _________________                             _________________
 - |  年龄 |  姓名 |                             |  年龄 |  姓名 |
 - -----------------      mergeCells(grid,[0])   -----------------
 - |  18   |  张三 |              =>             |       |  张三 |
 - -----------------                             -  18   ---------
 - |  18   |  王五 |                             |       |  王五 |
 - -----------------                             -----------------
*/
function mergeCells(grid, cols, isAllSome) {
    // Do Something
}
```

# vue

## VUE Script 标签内部声明顺序
1. 副作用 (触发组件外的影响)
- el
2. 全局感知 (要求组件以外的知识)
- name
- parent
3. 组合 (向选项里合并属性)
- extends
- mixins
4. 接口 (组件的接口)
- inheritAttrs
- model
- props/propsData
5. 组件类型 (更改组件的类型)
- functional
6. 模板修改器 (改变模板的编译方式)
- delimiters
- comments
7. 本地状态 (本地的响应式属性)
- data
- computed
8. 模板依赖 (模板内使用的资源)
- components
- directives
- filters
9. 事件 (通过响应式事件触发的回调)
- watch
10. 非响应式的属性 (不依赖响应系统的实例属性)
- methods
11. 生命钩子函数
- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- activated
- deactivated
- beforeDestroy
- destroyed
12. 渲染 (组件输出的声明式描述)
- template/render
- renderError

## 元素特性的顺序
```
- v-for
- v-if / v-show
- id
- ref / key / slot
- v-model
- v-on//强烈建议用简写 @，如@change
```

[参考资料](https://www.jianshu.com/p/8b095857f73e)


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