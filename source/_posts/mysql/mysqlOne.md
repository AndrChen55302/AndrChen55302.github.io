---
title: mysql数据库基本操作
date: 2021-09-05
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630745290000.png
tags: mysql数据库基本操作
categories: mysql
---
## 前言
一些关于数据库的基本操作

### 操作database
```mysql
查询数据库：show databases;

创建数据库：create database 库名;

删除数据库：drop database 库名

查看指定库的表：show tables where 目标库名
```

### 操作table

#### 创建表
```mysql
创建表                   //判断表名是否重复
                 语法: create table [if not exists]表名(
                                   字段1 数据类型[字段属性][约束][索引][注释]，
                                    ......
                                   字段1 数据类型[字段属性][约束][索引][注释]                                                                   )[表类型][表字符集][注释];
                 示例:#创建学生表
                       create table `student`(
                                     `studentNo` int(4) primary key
                                     `name` char(10),
                                      .....);
                 注意:多字段使用逗号分隔，保留字用撇号括起来，单行注释:#......,
                                                             多行注释:/*.....*/
```

```mysql

删除表：drop table 表名

查看表结构：desc 表名

查看表：show tables

查看创建表信息：show create table 表名

修改表名  alter table 旧表名 rename 新表名;
添加字段  alter table 表名 add 字段名 数据类型[属性];
修改字段  alter table 表名 change 原字段名 新字段名  数据类型[属性];
删除字段  alter table 表名 drop 字段名;

```
### 操作数据
#### 查询
```mysql
精确查询：select * from 表名 where 条件语句

　　　　　　运算符查询：select * from 表名 where id = 1+1

　　　　　　　　　　　　select * from 表名 where id < 100

　　　　　　逻辑查询：select * from 表名 where and条件

　　　　　　　　　　　select * from 表名 where or条件

　　　　　　模糊查询：select * from 表名 where 列名 like'值'　　值：%a%(查找中间有a的数据)  a%(查找以a开头的数据)  %a（查找以a结尾的数据）

　　　　　　排序与受限查询：select * from 表名 where order by 列名 desc　　desc：表示从大到小排序  asc：表示从小到大排序

　　　　　　　　　　　　　　select * form 表名 limit x,y　　x：表示跳过多少条  y：表示去多少条

　　　　　　聚合排序：select count(列名) from 表名　　

　　　　　　　　　　　count：计算表中某个列或多个列中数据的次数

　　　　　　　　　　　avg：平均值

　　　　　　　　　　　max：最大值

　　　　　　　　　　　min：最小值

　　　　　　　　　　　sum：总和　　一般用于计算价格

　　　　　　区间查询：select * from 表名 where 字段 between 0 and 10　　查找0到10区间的数据

　　　　　　分组查询：select 展示的列 from 表名 group by 参考列

　　　　　　　　　　　select name,count(列) from 表名 group by name

　　　　　　　　　　　select  name,count(content)  from  表名  group  by   name  having   count(content)  >  5　　having 是在聚合的基础上再筛选
```

```mysql
添加：insert into 表名 value()

删除：delete from 表名 （慎用，删除整个表数据）

delete from 表名 where 条件语句

修改：update 表名 set 字段名=值 where 条件语句
```





