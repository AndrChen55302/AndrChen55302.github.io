---
title: redis基础
date: 2021-09-06
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630858942000.png
tags: redis基础
categories: redis
---

# redis

## 1.什么是Redis数据库

Redis是一个完全开源的高性能的key-value数据库

## 2.数据库

### 2.1关系型数据库

例如：Oracle、Mysql、SQLServer

关系型数据库是指采用了关系模型来组织数据的数据库

以行和列的形式存储数据，以便于用户理解

#### 2.1.1 关系型数据库特性

1.关系型数据库，是采用了关系模型来组织数据的

2.最大的特点是事务的一致性

3.简单来说，关系模型就是二维的表格模型

而一个关系型数据库就是由二维表以及之间的联系所组成的一个数据组织

#### 2.1.2 关系型数据库的优点

1.二维表符合逻辑，容易理解，其关系模型相对网状和层次等其他关系模型更加容易理解

2.使用方便，SQL语言让操作的关系型

3.易于维护

#### 2.1.3 关系型数据库的缺点

1.固定的表结构

2.高并发的读写需求

3.海量数据的高效率读写

### 2.2非关系型数据库

常见的非关系型数据库:Redis ,MongoDB

非关系型数据库可以为大数据建立快速、可扩展的存储库，为了解决大规模数据集合多重数据种类带来的挑战

#### 2.2.1 非关系型数据库的特性

1.使用了键值对的形式来存储数据

2.非关系型数据库严格来说不是一种数据库应该是一种数据结构化的存储方式的集合

#### 2.2.2 非关系型数据库的优点

1.无序经过SQl层的解析，镀锡性能很高

2.基于键值对，数据没有耦合性，容易扩展

3.存储数据的格式

​	nosql的存储格式是key-value形式

​	可以存取文档，图片等

​	而关系型数据库只能支持基本类型

4.处理高并发，大批量的数据的能力强

#### 2.2.3 非关系型数据库的缺点

1.不提供sql，学习较难

2.事务处理能力弱

3.没有完整的约束，对于复杂业务场景支持差

#### 2.2.4 Redis命令

redis-server.exe redis.windows.conf :启动服务

redis-cli.exe -h 127.0.0.1 -p 6379 :访问服务

1.set name zhangSan 存值

2.get name 读值

3.keys * 查看所有的key

4.del name 删除key

5.ttl name 以秒为单位返回key剩余的时间

​	返回 -1 ：一直存在

​	返回 -2 ：已过期(不可用)

6.expire userName 60 设置key的过期时间，key过期后将不可用，单位s

7.exists name 检查key是否存在

​	1：存在

​	0：不存在

### 3.Redis支持的数据类型

1.Redis通常被称为数据结构服务器，因为我们的value可以是

字符串(String)

字符串(Hash/Map)

字符串(list)

字符串(sets)

字符串(sorted sets)

#### 3.1 String类型

1.String是Redis最基本的类型

2.String是二进制安全的，可以包含任何数据

3.String类型，一个键最大能存储512MB

#### 3.2 Hash类型

1.Redis Hash 是一个键值对集合

2.是一个String类型的映射表 hash特别适合用户存储对象

```cmd
127.0.0.1:6379> HMSET user userName "zhangSan" userCode "123" password "123"
OK
127.0.0.1:6379> hgetall user
1) "userName"
2) "zhangSan"
3) "userCode"
4) "123"
5) "password"
6) "123"
```

#### 3.3 list类型

1.Redis列表式简单的字符串列表，按照插入顺序排序

```cmd
127.0.0.1:6379> LPUSH userList zhangsan
(integer) 1
127.0.0.1:6379> LPUSH userList lisi
(integer) 2
127.0.0.1:6379> LPUSH userList gouDan
(integer) 3
127.0.0.1:6379> LRANGE userList 0 10
1) "gouDan"
2) "lisi"
3) "zhangsan"
```

#### 3.4 set类型

1.Redis的set是String类型的无序列表

2.Redis中集合时通过哈希表实现的，所以添加，删除，查找都很简单

```cmd
127.0.0.1:6379> Sadd nameSet "aaa"
(integer) 1
127.0.0.1:6379> Sadd nameSet "bbb"
(integer) 1
127.0.0.1:6379> Sadd nameSet "ccc"
(integer) 1
127.0.0.1:6379> SMEMBERS nameSet
1) "bbb"
2) "aaa"
3) "ccc"
```

#### 3.5 sorted sets类型

1.有序列表和我们的set一样是String类型的集合

2.不允许有重复的成员

3.关联一个double类型的分数，然后通过分数来为Redis中的集合成员进行从小到大的排序

```cmd
127.0.0.1:6379> ZADD list 1 qwe
(integer) 1
127.0.0.1:6379> ZADD list 2 asd
(integer) 1
127.0.0.1:6379> ZADD list 3 zxc
(integer) 1
127.0.0.1:6379> ZRANGE list 0 10
1) "qwe"
2) "asd"
3) "zxc"
127.0.0.1:6379> ZRANGE list 0 10 WITHSCORES
1) "qwe"
2) "1"
3) "asd"
4) "2"
5) "zxc"
6) "3"
```







