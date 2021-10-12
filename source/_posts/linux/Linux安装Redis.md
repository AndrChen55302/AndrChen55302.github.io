---
title: Linux安装Redis
date: 2021-10-05
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633760025000.webp
tags: linux安装软件
categories: linux
---

## 前言

安装redis 和熟悉一些简单的命令

### 下载Redis

 - 创建 在opt文件夹下创建soft

   > ​	mkdir /opt/soft 

- 使用 cd /opt/soft 进入该文件夹

- 使用 wget下载

  > ​	wget https://download.redis.io/releases/redis-5.0.13.tar.gz 

### 安装

 - 解压下载好的安装包

   > ​	tar -zxvf redis-5.0.13.tar.gz 

- 查看该文件下的文件 ls

  ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632737971000.png)

  `` 这里需要root用户的权限 

- 会爆一个错误 gcc未找到 需要 c++的依赖库

  ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632738073000.png)

- 下载 gcc

  > yum install gcc

- 再次出错

  ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632738865000.png)

- 运行 make MALLOC=libc

- 运行 make install

  ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632739061000.png)

- 已经可以启动

### 配置redis默认配置文件

  - 需要配置项
    - 设置运行外网连接：注释#bind 127.0.0.1 ,默认情况下只允许本地连接 69
    - 设置登录密码：添加一行 requirepass 12345 ，12345是密码 507
    - 设置redis后台启动：daemonize no 改为 daemonize yes 136
    - 关闭保护模式： 找到protected-mode yes 改成 protected-mode no 88

- 返回上一级redis-5.0.13 并 修改redis.conf

  > ​	cd ../
  >
  > ​	vim redis.conf

- 设置运行外网连接

  ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632740249000.png)

  在redis.conf中找到这个注释掉 并保存

- 设置登录密码

  ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632740849000.png)

  

- 相信这两个改过以后 剩下两个就不用多说了

- 已经可以启动redis了  注意路径
