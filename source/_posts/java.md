---
title: java
date: 2017-05-09 15:48:26
tags:
categories:
keywords:
---
>java 培训笔记

# 第三章

## Scanner
重点：Scanner input= new Scanner(System.in) 在整个程序里面，只用写一次就可以了。然后input可以对于任意输出来进行调用。
input.close();//清除input里面的值

## boolean（布尔）类型
boolean类型的值
真：true
假：false
例如：boolean isTiaoJian1=javaScore>98 && musicScore >80;
例如命名：boolean isBig
注：需要注意的一点是，boolean在命名的时候开头需要用is

## if结构
多重if执行结构为1次或0次，不会执行更多的次数。所以多重if需要遵循从前往后的执行顺序。

在if里面，如果是字符串char和String进行比较的话，需要使用equals方法
例如：if(sex.equals("男"))
当如果需要对常量和变量进行比较的时候，需要把常量写在前面，变量写后面
例如：if("男".equals(sex))

## switch中可以使用的数据类型：
1. char、short、int、long、bool 基本类型都可以用于switch语句。 2. float、double都不能用于switch语句。 3. enum类型,即枚举类型可以用于switch语句。4.String可以用于switch语句，但是限定于jdk1.7版本以上

<!--  more  -->

## *比较switch和多重if选择结构
相同点
都是用来处理多分支条件的结构
不同点
switch选择结b构：只能处理等值条件判断的情况，而且条件必须是整型变量或字符型变量或String。
多重if选择结构：没有swithc选择机构的限制，特别适合某个变量处于某个连续区间时的情况。

## 退出命令
System.exit(0);  //程序结束执行，0正常退出，1是异常推出

## 判定输入是否异常
hasNextInt //异常判断

==比较的是变量的地址
equals比较的是String类型变量的值

# 第四章

循环的四要素
1、初始化循环变量
2、判断循环条件
3、执行循环操作
4、迭代循环变量

while循环和do-while循环的区别
*语法不同

*执行次序不同
*初始情况不满足循环条件时
1、while循环一次都不会执行
2、do-while循环不管任何情况都至少执行一次

# 第五章

## 案例：实现整数反转
``` java
    public static void mian(String[ ] args){
    int val=12345;
    int r_digit;
    System.out.print("反转后的整数是：");
    while(val!=0){
            r_digit=val%10;
            System.out.print(r_digit);
            val=val/10;
        }
    }
```

## for循环执行条件
1、参数初始化
2、条件判断
3、循环操作
4、更新循环变量
5、条件判断（如果为true）则再次执行循环体

## for循环的条件使用方式
``` java
/*
*1.初始化循环变量可以不写在for里面
*2.迭代可以不写在for里面
*3.判断条件也可以不写在for里面，但要在循环体里面写上break
*/
案例展示
public static void mian(String[ ] args){
    int sum=0;//累加
    int i=0;
    for(){
        if(i>10){
            break;//终止循环
            }
        sum+=i;
        i++;
    }
    System.out.println("1到10累加的和为："+sum);
}
```

## break和Continue；
break   终止整个循环
continue  终止当前次的循环
使用场合
break可用于switch结构和循环结构中
Continue只能用于循环结构
作用（循环结构中）
break语句终止某个循环，程序跳转到循环块外的下一条语句。
continue跳出本次循环，进入下一次循环
？？continue在多重判断里面是否有作用呢？？

ctrl+shift+o是导入包
java里面的%的使用 NumberFormat(数字格式化)  
//百度搜索 java NumberFormat
/*指定要输出的格式化，"##.00%"这个格式是固定的*/
DecimalFormat df1=new DecimalFormat("##.00%");
/*格式化*/
String str=df1.format(0.389);
System.out.println(str)

## java如何获取当前时间
``` java
    Data currentDate=new data();
    System.out.println(currentDate);//标准格里高利历
    /*简易化日期显示，"yyyy/MM/dd hh:mm:ss"这个格式是固定的*/
    SimpleDateFormat sdf=new SimpleDateFormat("yyyy/MM/dd hh:mm:ss" );
    注：大写的mm是月，小写的mm是分钟
    String dateStr=sdf.format(currentDate);
    System.out.println(dateStr);
    Date newDate=sdf.parse(dateStr);//把字符转化成日期
```

嵌套循环的特点：
外层循环循环1次，内层循环要循环所有次。

# 第六章

## 什么是数组：
数组是一个变量，存储相同数据类型的一组数据
数组和变量的区别
变量：声明一个变量就是在内存空间划出一块合适的空间
数组：声明一个数组就是在内存空间划出一串连续的空间

## 数组的结构和基本要素
标识符：数组的名称，用于区分不同的数组
数组元素：实际就是变量名，用来确定数组的起始位置
元素下标：数组中每个元素的编号
数组元素：数组中存放内容

元素下标标明了元素在数组中的位置，从0开始
数组长度固定不变，避免数组越界
数组中的所有元素必须是相同的数据类型

## 使用数组的四步：
1、声明数组 int[] a;
2、分配空间 a=new int[5];
数据类型[ ]  数组名 = new 数据类型[大小]；
3、赋值 a[0]=8;
4、处理数据 a[0]=a[0]*10;

## 关于length
数组的长度使用：数组名.length
数组的length：没有括号的叫属性，有括号的叫方法

当基本数据类型被定义到堆空间的时候，java默认分配值（int double boolean）
引用数据数据类型不会分配值，是空的（String）
int：0
double:0.0
boolean:false
String:null

## 关于数组错误异常的提示：
NullpointerException  空指针异常  检查是否没有没有对引用类型赋值
ArrayIndexOutOfboundsException 数组下标越界   检查是否越界，超出长度

## for each（加强循环） 
for（String pro:products）{
System.out.println(pro);
}
步长 是指访问数组里面的量的时候，所需要的执行顺序。
例如：如果我要访问一个数组中每一个数的话，就可以用加强循环
   如果我要访问一个组数中不是每一个数的画（或者需要获取下标的时候也不能使用），就不能用加强循环。

`Arrays.sort`(数组);//对数组进行升序排列

基本数据类型：改变一个变量的值，不会去影响另外一个变量的值。

`compare`

## 对象
用来描述客观事物的一个实体，由一组属性和方法构成。
对象的属性和方法`通常`都是封装在一起的。
### 类
具有相同属性和方法的一组对象的集合

#### 定义一个类的步骤
1.定义类名
2.编写类的属性
3.编写类的方法

#### 常见错误
不能在方法外部直接写程序逻辑代码 如:
``` java
    public class demo{
        int age =20;

        if(age < 20){
            System.out.println("XXXX"); // 逻辑程序不能写在方法外部 除非加个代码块{}
        }
        public int show(){
            return age;
        }
    }
```

#### 作用域
局部变量的作用域仅限于定义他的方法
成员变量的作用域在整个类内部都是可见的

java会给成员变量一个初值
java不会给局部变量赋予初值
在同一个方法中，不允许有同名局部变量；在不同的方法中，可以有同名局部变量
两类变量同名时，局部变量具有更高的优先级

`堆内存`：保存对象的真正数据，都是每一个对象的属性内容
`栈内存`：保存的时一块堆内存的地址

#### 递归
``` java
    public class Hello{
        public static void main(String[] arry){
            System.out.println(add(100));
        }

        public static int add(int num){
            if(num == 1){//结束条件
                return 1;//不再向后继续加了
            }
            return num+add(num-1);//修改参数内容
        }
    }
```
# 字符串(String)

`concat` 字符串链接(数字类型调用不了)
`substring(3)`从第3个(包含)开始截取到最后
`substring(2,5)`从第2个(包含)开始截取到第5个(不包含)

## 文件名判断方法
``` java
    String fileName = "djkl.java";
    int index = fileName.lastIndexOf(".");
    //index != -1 找不到
    if(index != -1 && index != 0 && fileName.substring(index+1).equals("java")){
            System.out.println("有效文件名！");
    }else{
            System.out.println("文件名无效！");
    }

    /*检查邮箱格式*/
    if(email.indexOf('@') != -1 && email.indexOf('.') > email.indexOf('@')){
        emailCorrect = true;
    }else{
        System.out.println("Email无效！");
    }
```

## String StringBuffer StringBuilder的区别

# 第十章

## 构造函数
当没有显式声明有参的构造方法的时候，java会默认生成一无参的构造方法
一旦声明了有参的构造函数后，java就不会生成无参的构造函数

## static
可以用来修饰属性、方法和代码块
修饰的属性和方法称为类属性（类变量）、类方法
不使用static修饰的属性和方法，属于单个对象，通常称为实例属性（实例变量）、实例方法
static修饰的变量和方法可以通过类名和对象名访问，而不用static修饰的变量和方法只能通过对象名访问
final修饰的变量成为常量，其值固定不变

static定义的方法不能调用static的方法或属性
非static定义的方法可以调用static的属性或方法

## 封装
将类的某些信息隐藏在类内部，不允许外部程序直接访问，而是通过该类提供的方法来实现对隐藏信息的操作和访问
<span style="color: red">好处：</span>只能通过规定方法访问数据，方便加入控制语句，方便修改实现

## 抽象类
1.抽象类的关键字是abstract
2.在一个类如果至少有一个abstract方法，那么这个类必须是抽象类
3.抽象类里可以包含普通方法和属性
4.抽象类的子类必须实现父类的抽象方法，如果不想实现，那么这个子类必须为抽象类
5.abstract方法不能写为私有，必须是可见的
6.抽象方法不能有实现

``` java
    public abstract class PetAbstract extends Pet {
    public abstract void print();//不能为private
    
    public void text(){
        System.out.print("这是一个普通方法");
    }
}
```

## 多态
/|位置|方法名|参数表|返回值|访问修饰符
-|----|-----|-----|-----|---------
方法重写|子类|相同|相同|相同|不能比父类更严格
方法重载|同类|相同|相同|无关|无关

<span style="color: red">多态的必要条件</span>
1.要有继承
2.要有重写
3.父类引用指向子类对象

多态的好处
1.可替换性
2.可扩充性
3.接口性
4.灵活性
5.简化性

## IO流

File file = new File("/Users/`UserName #自己的用户名`/Desktop/1.html");
`File.separator` 用于代替"/"
``` java
File.separator+"Users"+File.separator+"UserName"+File.separator+"Desktop"+File.separator+"desktop"+File.separator+"1.html"
```

## 多线程

进程：是程序的一次动态执行过程，它对应了从代码加载、执行至执行完毕的一个完整过程。
特点：
1.进程是系统运行程序的基本单位
2.每一个进程有自己的独立的内存空间、一组系统资源。
3.每一个进程的内部数据和状态都是完全独立。

线程：进程内部的一个执行单位，它使程序中一个单一的顺序控制流程。

- 引入进程是为了多个程序并发执行，提高资源的利用率和系统吞吐量
- 引入线程是为了减少程序在并发执行时付出的时空开销
- 进程间不能共享内存，但线程之间共享内存非常容易
- 使用多线程实现多任务并发比多进程的效率高

### 实现方法
``` java
方法一:继承Java.lang.Thread类,并覆盖run()方法
    class MyThread extends Thread {
     public void run( ) {
         /* 覆盖该方法*/
      }
    }
方法二:实现Java.lang.Runnable接口,并实现run()方法
    class MyThread implements Runnable{
      public void run( ) {
            /* 实现该方法*/ 
      }
    }
```

# Tomcat
`$ /Library/Tomcat/bin/startup.sh`开启服务器
`$ /Library/Tomcat/bin/shutdown.sh`关闭服务器
`localhost:8080`服务器开启地址
webapps 网站放置地址
`localhost:8080/javaJar`