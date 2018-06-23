---
title: Markdown的语法
date: 2017-04-11 00:41:55
tags: markdown
categories: 文本编辑
---
# 标题

``` bash 
# 一级标题
## 二级标题
### 三级标题
以此类推，#后接一个空格

```

# 列表
无序列表使用星号、加号或是减号作为列表标记：

``` bash 
*   Red
*   Green
*   Blue

```
有序列表则使用数字接着一个英文句点

``` bash 
1.  Bird
2.  McHale
3.  Parish

```
<!-- more -->
# 分隔线

``` bash 
* * *

***

*****

- - -

---------------------------------------

```

# 链接
Markdown 支持两种形式的链接语法： 行内式和参考式两种形式。

不管是哪一种，链接文字都是用 [方括号] 来标记。

要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可

``` bash 
This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.

```

# 图片

``` bash 
![Alt text](/path/to/img.jpg)

![Alt text](/path/to/img.jpg "Optional title")

```

# 引用

``` bash 
> 引用

```

# 表格

``` bash 
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

```

# 阅读全文

``` bash 
<!-- more -->

```
