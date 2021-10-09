---
title: Linux虚拟机安装Maven
date: 2021-10-03
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633760025000.webp
tags: linux安装软件
categories: linux
---
## 前言

Maven的重要性无需多说 下就完了

### 前置条件

- 总的需要一个Linux系统吧 哈哈

- 已经下载过JDK了 最好是8版本的

- maven安装包：

  >https://maven.apache.org/download.cgi  3.8.2

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632731352000.png)

### 安装 Maven

- 在 opt下创建一个文件夹maven并进入

  >mkdir /opt/maven 
  >
  >cd /opt/maven

- 将 安装吧移入 maven文件夹下并解压

  >tar -zxvf apache-maven-3.8.2-bin.tar.gz

- 重命名

  >mv apache-maven-3.8.2 maven-3.8.2

### 配置环境变量

 - vim /etc/profile

   ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632732291000.png)

- 重新刷新配置文件

  > ​	source /etc/profile

### 设置阿里云镜像和本地仓库

 - vim /opt/maven/maven-3.8.2/conf/settings.xml

 - 设置本地仓库

   ><localRepository>/opt/maven/local-repository/</localRepository>

- 设置阿里云镜像加速

  ><mirror>  
  >    <id>nexus-aliyun</id>  
  >    <mirrorOf>central</mirrorOf>  
  >    <name>Nexus aliyun</name>  
  >    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  ></mirror>

### 查看是否成功

​	命令 mvn -version

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1632732719000.png)







