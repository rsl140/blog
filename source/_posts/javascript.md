---
title: javascript
date: 2017-04-11 11:10:07
categories: FE
tags: js
---
>看了javascript高级程序设计后对js的一些归纳总结，以及一些面试题的代码

# 概念

 - js的对象均继承于boject
 - call() 调用一个对象的一个方法，以另一个对象替换当前对象(第二参数任意)
 - apply() 应用某一对象的一个方法，用另一个对象替换当前对象(第二参数必须为数组或argument)
 - callee,caller(递归)
 - === 全等 与==的区别在于操作数未经任何转换时就相等(==在对比的时候做了隐式转换)。
 - prototype(原型) 指向一个对象(指针)
 - hasOwnProperty() 确定用的是实例还是原型对象
 - push() 向数组的末尾添加一个或多个元素，并返回新的长度
 - unshift() 向数组的开头添加一个或更多元素，并返回新的长度。
 - pop() 删除并返回数组的最后一个元素
 - shift() 删除并返回数组的第一个元素
 - 构造函数是一种可初始化其类的实例的成员函数。 构造函数具有与类相同的名称，没有返回值。 构造函数可以具有任意数量的参数，类可以具有任意数量的重载构造函数。
 - 闭包就是能够读取其他函数内部变量的函数
<!-- more -->
# 笔记

## slice() 方法
 - 可提取字符串的某个部分，并以新的字符串返回被提取的部分。
 - 从 start 开始（包括 start）到 end 结束（不包括 end）为止的所有字符

``` javascript
    stringObject.slice(start,end)
```

## sort() 方法
用于对数组的元素进行排序
如果想按照其他标准进行排序，就需要提供比较函数，该函数要比较两个值，然后返回一个用于说明这两个值的相对顺序的数字。
比较函数应该具有两个参数 a 和 b，其返回值如下： 
 - 若 a 小于 b，在排序后的数组中a应该出现在b之前，则返回一个小于0的值。那么 a 会被排列到 b 之前
 - 若 a 等于 b，则返回 0。 
 - 若 a 大于 b，则返回一个大于 0 的值。b 会被排列到 a 之前
[参考资料](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
``` javascript
    var arr=[1,3,4,52010,3001,20,123]
        arr.sort(function(a,b){
            if(a < b){return -1;}// a 会被排列到 b 之前
            else if(a > b){return 1;}// b 会被排列到 a 之前
            else if(a == b){return 0;}//此时不排序
        })

        data.sort(function(a,b){
            return a[1] - b[1];//升序排列
        })
```

## jq
### siblings()方法

``` javascript
    $('#navli').click(function(){ $(this).addClass('active').siblings().removeClass('active'); });//当前元素添加active样式，同辈元素删除active样式
```
# SetTimeout与setInterval区别
 - setTimeout   在执行时,是在载入后延迟指定时间后,去执行一次表达式,仅执行一次
 - setInterval 在执行时,它从载入后,每隔指定的时间就执行一次表达式

setTimeout (表达式,延时时间)
setInterval(表达式,交互时间)
延时时间/交互时间是以豪秒为单位的(1000ms=1s)
``` javascript
    function run(){
        // 其他代码
        setTimeout(function(){
            run();
        }, 10000);
    }
    run();
```
以上面的代码来说, 虽然设置的是10s执行一次, 但是实际时间却是需要`// 其他代码的执行时间`来确定
即setTimeout的间隔时间是, `// 其他代码执行时间` + 10s

``` javascript
    setInterval(function(){
        run();
    }, 10000);
```
而setInterval, 不会有上面的问题, 但是如果run()的执行时间, 操作大于10s, 那么甚至可能跳过任务
[参考资料](https://segmentfault.com/q/1010000005989491)

***
# 有关JS的面试题

## 删除数组内重复内容

``` javascript
    var arr=[7,1,2,3,3,4,"abc",4,5,5,6,1,9,3,25,4,"abc"];

    function deletRepeat(){
        var renArray=[];//空数组用来存储删除重复内容后的数组
        var obj={};//创建一个空对象
        var index = 0;
        for(var i=0;i<arr.length;i++){
            if(obj[arr[i]]==undefined){//当对象中没有对应的内容时返回undefined
                renArray[index]=arr[i];
                index++;
                obj[arr[i]]=1;//为当前对象添加一个值为1
            }
            else if(obj[arr[i]]==1){
                continue;
            }
        }
        return renArray;
    }
    alert(deletRepeat(arr));
```

## 数组的随机排序
``` javascript 
    var arr = [1,2,3,4,5,6,7,8,9,10]; 

    //方法一：
    function randSort1(arr){
        for(var i = 0,len = arr.length;i < len; i++ ){
            var rand = parseInt(Math.random()*len);
            var temp = arr[rand];
            arr[rand] = arr[i];
            arr[i] = temp;
            }
        return arr;
        }
        console.log(randSort1(arr));
    //方法二:
    function randSort2(arr){
        var mixedArray = [];
        while(arr.length > 0){
            var randomIndex = parseInt(Math.random()*arr.length);
            mixedArray.push(arr[randomIndex]);
            arr.splice(randomIndex, 1);
        }
        return mixedArray;
    }
    console.log(randSort2(arr));
    //方法三:  
        arr.sort(function(){
            return Math.random() - 0.5;
        })
        console.log(arr);
```

# 一个有意思的题目：
``` javascript
    function Foo() {
        getName = function () { alert (1); };
        return this;
    }
    Foo.getName = function () { alert (2);};
    Foo.prototype.getName = function () { alert (3);};
    var getName = function () { alert (4);};
    function getName() { alert (5);}

    //请写出以下输出结果：
    Foo.getName();//2
    getName();//4
    Foo().getName();//1
    getName();//1
    new Foo.getName();//2
    new Foo().getName();//3
    new new Foo().getName();//3
```
[参考资料](http://www.cnblogs.com/xxcanghai/p/5189353.html)