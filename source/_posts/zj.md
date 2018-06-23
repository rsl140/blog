---
title: css常用样式||知识点
date: 2017-07-11 08:36:32
tags: css
categories: FE
keywords: css style
---
>常用样式整理

# 居中div

- 水平居中：给div设置一个宽度，然后添加margin:0 auto属性

``` css
     div{
        width:200px;
        margin:0 auto;
      }
```

- 让绝对定位的div居中

``` css
     div {
        position: absolute;
        width: 300px;
        height: 300px;
        margin: auto;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        background-color: pink; /* 方便看效果 */
     }
```

- 水平垂直居中一

``` css
确定容器的宽高
 div {
    position: relative;     /* 相对定位或绝对定位均可 */
    width:500px;
    height:300px;
    top: 50%;
    left: 50%;
    margin: -150px 0 0 -250px;      /* 外边距为自身宽高的一半 */
    background-color: pink;     /* 方便看效果 */

  }
```

- 水平垂直居中二

``` css
 未知容器的宽高，利用 `transform` 属性

 div {
    position: absolute;     /* 相对定位或绝对定位均可 */
    width:500px;
    height:300px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: pink;     /* 方便看效果 */

 }
```

- 水平垂直居中三

``` css
 利用 flex 布局
 实际使用时应考虑兼容性

 .container {
    display: flex;
    align-items: center;        /* 垂直居中 */
    justify-content: center;    /* 水平居中 */

 }
 .container div {
    width: 100px;
    height: 100px;
    background-color: pink;     /* 方便看效果 */
 }  
```

# display值的作用

``` bash

block           块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
none            缺省值。象行内元素类型一样显示。
inline          行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
inline-block    默认宽度为内容宽度，可以设置宽高，同行显示。
list-item       象块类型元素一样显示，并添加样式列表标记。
table           此元素会作为块级表格来显示。
inherit         规定应该从父元素继承 display 属性的值。
```
<!--  more  -->


# position的值relative和absolute定位原点

``` bash
    absolute
        生成绝对定位的元素，相对于值不为 static的第一个父元素进行定位。
    fixed （老IE不支持）
        生成绝对定位的元素，相对于浏览器窗口进行定位。
    relative
        生成相对定位的元素，相对于其正常位置进行定位。
    static
        默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right z-index 声明）。
    inherit
        规定从父元素继承 position 属性的值。
```

# CSS选择符,属性继承
``` bash
*   1. id选择器（ # myid）
    2. 类选择器（.myclassname）
    3. 标签选择器（div, h1, p）
    4. 相邻选择器（h1 + p）
    5. 子选择器（ul > li）
    6. 后代选择器（li a）
    7. 通配符选择器（ * ）
    8. 属性选择器（a[rel = "external"]）
    9. 伪类选择器（a:hover, li:nth-child）

*   可继承的样式： font-size font-family color, UL LI DL DD DT;

*   不可继承的样式：border padding margin width height ;
```

# CSS优先级算法
``` bash
     优先级就近原则，同权重情况下样式定义最近者为准;
     载入样式以最后载入的定位为准;

  优先级为:
    同权重: 内联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）。
    !important >  id > class > tag
    important 比 内联优先级高
```

# 初始化CSS样式

``` css
淘宝的样式初始化代码：
  body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
  body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
  h1, h2, h3, h4, h5, h6{ font-size:100%; }
  address, cite, dfn, em, var { font-style:normal; }
  code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
  small{ font-size:12px; }
  ul, ol { list-style:none; }
  a { text-decoration:none; }
  a:hover { text-decoration:underline; }
  sup { vertical-align:text-top; }
  sub{ vertical-align:text-bottom; }
  legend { color:#000; }
  fieldset, img { border:0; }
  button, input, select, textarea { font-size:100%; }
  table { border-collapse:collapse; border-spacing:0; }
```

#清除浮动
``` css
.clearfix::before, .clearfix::after {
    content: " ";
    display: table;
}
.clearfix::after {
    clear: both;
}
.clearfix {
    *zoom: 1;
}

sass版
&::after,&::before{
    content: " ";
      visibility: hidden;
      display: block;
      height: 0;
      clear: both;
}
```

[参考出处](https://github.com/markyun/My-blog/blob/master/Front-end-Developer-Questions/Questions-and-Answers/README.md)
