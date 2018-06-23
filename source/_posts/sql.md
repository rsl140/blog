---
title: mysql总结
date: 2017-08-02 20:16:46
tags: sql
categories: sql
keywords: sql
---
>mysql数据库总结

# sql的组成

## 数据定义语言（DDL），用来定义和管理数据对象，如数据库、表等

`create、drop、alter`

## 数据操作语言（DML），用于操作数据库对象中包含的数据

`insert、update、delete`

## 数据查询语言（DQL），用于查询数据库

`select`

## 数据控制语言（DCL），用来管理数据库，包括管理权限和数据更改

`grant、commit、rollback`

# 数据库(database)管理

## 创建数据库

``` sql
    create database 表名;
```

<!--  more  -->


## show 查看所有数据库

``` sql
    show databases;
```

## alter 修改数据库编码

默认创建的数据库默认不支持中文字符，如果我们需要它支持中文字符，则将它的编码设置为utf8格式:
``` sql
    ALTER DATABASE 表名 CHARACTER SET UTF8;
```

## 使用数据库

``` sql
use 库名;
```

## 查看当前使用的数据库

``` sql
select database();
```

## 删除数据库

``` sql
drop database 库名;
```

## 数据库备份

``` sql
    mysqldump -u root -p --default-character-set=utf8 world > bakWorld.sql;
```

# 数据表（table）管理
首先创建`create`数据库，其次`use`数据库，否则操作都会不成功。

## 创建表

``` sql
create table 表名 (
    ID int AUTO_INCREMENT PRIMARY KEY,
    NAME varchar(20) not null,
    AGE int not null,
    BIRTHDAY datetime);
```

## 显示表

显示当前数据库所有的数据表
``` sql
    show tables;
```

## 查看表结构

``` sql
    desc 表名;
```

## 修改表的编码

``` sql
ALTER TABLE KEYCHAIN CONVERT TO CHARACTER SET UTF8;
```

## 向表中添加字段

``` sql
    alter table 表名 add 字段名 属性(如：int);
```

## 修改表中的字段属性

``` sql
    alter table 表名 MODIFY 字段名 新属性;
```

## 删除表中的属性

``` sql
    alter table 表名 DROP column 字段名;
```

## 重命名表名

``` sql
RENAME TABLE 表名 TO 新表名;
```

## 修改表字段允许为空或不允许为空

``` sql
alter table 表名 MODIFY 字段名 属性(如：int(3)) null(not null);-- 允许为空或不允许为空
```

## 利用已有数据创建新表

``` sql
create table 新表名 select * from 表名;
```

## 删除约束

``` sql
ALTER TABLE 表名 DROP CONSTRAINT 字段名 CASCADE;
```

## 新建主键约束
``` sql
ALTER TABLE TAB_NAME ADD CONSTRAINT PK_NAME PRIMARY KEY(FILED,...);
```

# 数据的操作及管理

数据表的基本操作，包含增、删、改、查数据。

## 增加数据

``` sql
insert into 表名 VALUES (null, '1', 2, '3');
```

## 查看数据

``` sql
select * from 表名;-- *代表所有字段
```

## 删除数据

``` sql
delete from 表名 where name = 'xxx';
```

## 修改数据

``` sql
update 表名 set name='xxxx' where name = 'xxx'; -- xxxx为修改后的数据，xxx为在表中查找的数据
```

## 查询手机号码区号以133和135开头的员工的姓名,性别,部门及联系电话
``` sql
select name as 姓名,sex as 性别,department as 部门,phone as 联系电话 from employee where phone like '133%' or phone like '135%';-- %为通配符 _也是通配符区别在于%为通配之后所有，_为通配后一位 通配需用like关键字
```

- select 查找不重复（distinct）的数据，使数据按条件排序（order by），按查询条件显示数据（where）

## 子查询
``` sql
select * from student where birthday>(select birthday from student where stuname='周墨嘉');
```

# 函数

``` sql
select length('老九学堂'); -- 12, 返回字节数
select char_length('你好啊'); -- 3, 返回字符数
select position('def' in 'abcdefg'); -- 4, 返回子串substr在字符串str第一个出现的位置，如果substr不是在str里面，返回0  
select position('学堂' in '老九学堂'); -- 3
select lower('MySQL数据库');-- mysql数据库
select rtrim(ltrim('   abc   ')); -- abc,清除左右空格
select left('中华人民共和国',3);-- 中华人，从字符串左右两边返回指定数目的字符
select replace('中华人民共和国', '人民', '民众');-- 中华民众共和国，字符串替换
select reverse('中华人民共和国'); -- 国和共民人华中, 颠倒字符
select concat(99, '人斩');-- 拼接字符串
select now();-- 返回当前日期、时间
select adddate(now(),interval 5 day);-- 返回5天后的时间
-- 返回是今年的第几天
select dayofyear(now());
select dayofweek(now());
select day(now());
-- 日期之差
select timestampdiff(year,'2008-8-8',now());

-- 嵌套
select (
replace(replace (replace(loginpwd,'O','0'),'l','1'),'o','0')
) from Student;
```

# 例题

在某公司员工表中，为了节省空间，员工编号是按照“部门编号-员工编号”格式存放的，如：4-7,5-6,1-21,10-11,10-1,1-12,2-10,......,12-21,12-12,14-10,14-7等
现在公司希望通过sql语句进行排序，首先按照前半部分的数字进行排序，然后再按照后半部分的数字进行排序，输出结果如下：1-12,1-21,2-10,4-7,5-6,10-1,10-11......,12-12,12-21,14-7,14-10
数据表名称：Employee
字段名称：EmpId

``` sql
create database cardinfo;
ALTER DATABASE cardinfo CHARACTER SET UTF8;
use cardinfo;

create table employee(
    id int(4) primary key not null auto_increment,
    empid varchar(20) 
) charset=utf8;

insert into employee(empid) values
    ('1-1'),('2-1'),('1-3'),('4-1'),('1-11'),('5-22'),('3-12'),
    ('6-1'),('2-12'),('3-13'),('5-21'),('1-7'),('6-5'),('3-3');

select empid as 排序,
cast(left(empid,position('-' in empid)-1) as signed) as 左边,
cast(substr(empid,position('-' in empid)+1)as signed) as 右边
from employee order by 左边 asc,右边 asc;
```


## 左向外联接的结果集包括  LEFT OUTER子句中指定的左表的所有行，而不仅仅是联接列所匹配的行。如果左表的某行在右表中没有匹配行，则在相关联的结果集行中右表的所有选择列表列均为空值。

``` sql
select st.stuname 姓名,st.identity_card 身份证,sc.SubjectID,if(isnull(sc.score),'缺考',sc.score) as '成绩'  from 
student st left join score sc on(st.stuid=sc.stuid) order by st.stuname;

-- 显示如下

    +-----------+------------------+-----------+--------+
    | 姓名      | 身份证           | SubjectID | 成绩   |
    +-----------+------------------+-----------+--------+
    | 周墨嘉    | 510123323333333x |      NULL | 缺考   |
    | 李彤彤    | 510123325555555x |         2 | 81     |
    | 陈天乐    | 510123322222222x |         3 | 90     |
    | 陈天乐    | 510123322222222x |         2 | 60     |
    | 陈天乐    | 510123322222222x |         1 | 85     |
    | 马云      | 510128349124793  |      NULL | 缺考   |
    | 黄诗怡    | 510123324444444x |         2 | 63     |
    | 黄颖琦    | 510123321111111x |         1 | 80     |
    | 黄颖琦    | 510123321111111x |         3 | 78     |
    | 黄颖琦    | 510123321111111x |         2 | 55     |
    +-----------+------------------+-----------+--------+
    10 rows in set (0.01 sec)

```

<span style="color: red">left 是以左边为主 显示全部内容，右边的表为辅，右边的表若没有值则显示为空。同理right</span>

# JDBC

``` java
        // 使用PreparedStatement防注入
        // 加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        //创建连接
        Connection con=DriverManager.getConnection("jdbc:mysql://localhost:3306/school","服务器名","数据库密码");
        //3、执行sql语句
        String querySql="select * from grade where id=? and grade_name=?";
        PreparedStatement pst=con.prepareStatement(querySql);
        pst.setInt(1,2);//第一个参数表示第几个问号，第二个参数表示参数的值
        pst.setString(2,"十一年级");
        ResultSet rs =pst.executeQuery();
        //pst.executeUpdate();修改、删除、新增使用这个方法
        while(rs.next()){
            System.out.println(rs.getInt(1)+"\t"+rs.getString(2));
        }
        rs.close();
        pst.close();
        con.close();
```
