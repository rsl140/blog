---
title: ECM6初探
date: 2017-09-06 14:50:54
tags: ECM6
categories: FE
keywords: ECM6
---
>ECM6学习总结 [ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/let)

# let

1. 声明变量用，类似var.只在代码块内有效.
``` javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
2. 不存在变量提升，声明变量一定要再声明后使用否则会报错
3. 暂时性死区（temporal dead zone，简称 TDZ）
``` javascript
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```
4. 不允许在<span style="color: red;">相同作用域</span>内，重复声明同一个变量
<!--  more  -->
5. <span style="color: red;">注意：ES6的块级作用域允许声明函数的规则，只在使用大括号的情况下成立。</span>

# const

1. `const`声明一个只读的常量。一旦声明，常量的值就不能改变。
2. 只在声明所在的块级作用域内有效
3. 实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动
4. 对于`复合类型的数据`（主要是对象和数组），变量指向的内存地址，保存的只是一个指针
5. 如果真的想将对象冻结，应该使用Object.freeze方法。
``` javascript
const foo = Object.freeze({});
// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```

# 变量的解构赋值

``` javascript
按照对应位置，对变量赋值
    let [foo, [[bar], baz]] = [1, [[2], 3]];
    foo // 1
    bar // 2
    baz // 3
```
- 如果解构不成功，变量的值就等于undefined
- 如果是不完全解构，值匹配部分数组也是可以的
- 只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值
- 解构赋值允许指定默认值
``` javascript
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

# rest参数

(形式为：...变量名)
用于获取函数的多余参数
rest参数后不能再有其他参数(只能是最后一个参数)
``` javascript
// 使用argument变量的写法
function sortNumbers() {
    return Array.prototype.slice.call(arguments).sort();
}
// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```

