---
title: Linux虚拟机安装JDK1.8
date: 2021-10-02
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633760025000.webp
tags: linux安装软件
categories: linux
---
## 前言

安装好Linux系统之后 系统会自带JDK 叫openjdk 如果只是运行的话是没问题的，但是想要在开发，那就需要自己去安装Oracle公司的JDk。

### 卸载 OpenJDK

- 查看已有的 JDK

>rpm -qa | grep java

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632727838000.png)

- 卸载 OpenJDK

  - 卸载单个

    >yum -y remove javapackages-tools-3.4.1-11.el7.noarch

  - 卸载具有相同点的

    > yum remove -y java-1.*.0-openjdk-*

​       `` 全部卸载完成之后进行下一步操作

### 安装JDK

- 下载

  >https://www.oracle.com/java/technologies/downloads/#java8 JDK8 连接

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632729034000.png)

- 将下载好的JDK8放入Linux opt 目录下，解压到/usr/local 目

  -  先试用 cd 命令进入到 opt目录下

  -  使用命令解压

    >tar -zxvf jdk的文件名 –C /usr/local/

  - 重命名

    `` 前置要求  进入local 目录

    > mv jdk1.8.0_301 jdk1.8

### 配置JDK的环境变量

 - 接下来我们需要配置一下全局的环境变量。

   > 我们需要修改 etc 下面的 profile 文件。命令如 下：vim /etc/profile。我们需要在 profile 文件中添加如下内容

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632730147000.png)

 - 使用命令刷新配置文件

   >source /etc/profile

- 查看环境变量

  > echo $PATH

### 检测是否安装成功

​	命令 ：java -version

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632730457000.png)



