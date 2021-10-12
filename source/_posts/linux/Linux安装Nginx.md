---
title: Linux安装Nginx
date: 2021-10-04
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633760025000.webp
tags: linux安装软件
categories: linux
---

## 前言

说到Nginx，可能大家最先想到的就是其负载均衡以及反向代理的功能。

那今天就来安装一下吧

### 下载地址

Nginx 下载地址：http://nginx.org/en/download.html 

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633744245000.png)

开发版本主要用于 Nginx 软件项目的研发，稳定版本说明可以作为 Web 服务器投入商业应用。这里我们选择当前稳定版本：nginx-1.21.3 

将鼠标悬浮在版本号上 左下角会出现对应信息

### Windows版本安装

下载后缀为zip的安装包并解压至任意目录

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633769302000.png)

解压目录如下:

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633769409000.png)

#### 目录介绍

- conf 目录：存放 Nginx 的主要配置文件，很多功能实现都是通过配置该目录下的 nginx.conf 文件
- docs 目录：存放 Nginx 服务器的主要文档资料，包括 Nginx 服务器的 LICENSE、OpenSSL 的 LICENSE 、PCRE 的 LICENSE 以及 zlib 的 LICENSE ，还包括本版本的 Nginx服务器升级的版本变更说明，以及 README 文档
- html 目录：存放了两个后缀名为 .html 的静态网页文件，这两个文件与 Nginx 服务器的运行相关。
- logs 目录：存放 Nginx 服务器运行的日志文件。
- nginx.exe：启动 Nginx 服务器的exe文件，如果 conf 目录下的 nginx.conf 文件配置正确的话，通过该文件即可启动 Nginx 服务器。

#### 启动和关闭 Nginx

##### 启动

​	双击解压之后目录中的 nginx.exe 文件，出现一闪而过的画面，则启动成功。

​	然后在浏览器中输入 http://localhost 或者 http://localhost:80 出现如下界面即启动成功。

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633769883000.png)



​	该页面即是上面解压目录中 html 目录下的 index.html 文件。

##### 关闭

进入到解压之后的目录，输入如下命令：

> nginx.exe -s stop

或者也可以打开任务管理器，找到 nginx 的进程，直接右键结束。

### linux虚拟机安装

选择的 Linux 系统为 CentOS7.6。

#### 安装 nginx环境

##### 安装C++编译环境

>yum install gcc-c++

对于 gcc，因为安装nginx需要先将官网下载的源码进行编译，编译依赖gcc环境，如果没有gcc环境的话，需要安装gcc。

##### 安装模块依赖库

>yum install pcre*
>
>yum install openssl*
>
>yum install zlib*

或者

>yum -y install pcre* openssl* zlib*

对于 pcre，prce(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式，所以需要在linux上安装pcre库。

　　对于 zlib，zlib库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip，所以需要在linux上安装zlib库。

　　对于 openssl，OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。nginx不仅支持http协议，还支持https（即在ssl协议上传输http），所以需要在linux安装openssl库。

#### 编译安装

由于nginx较小所以使用wget下载

>#### wget http://nginx.org/download/nginx-1.21.3.tar.gz

解压至  /usr/local/ 目录下

>tar -zxvf nginx-1.21.3.tar.gz -C /usr/local

进入该目录(nginx下) 并执行以下命令

>./configure
>
>make  &&  make install

执行完之后 local下会有两个文件夹 nginx/nginx-1.21.3

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633771122000.png)

#### 启动

我们进入到/usr/local/nginx/sbin 目录，通过如下命令启动 nginx：

>cd ./nginx/sbin/
>
>./nginx

当然你也可以配置环境命令，这样在任意目录都能启动 nginx。

Linux 没有消息就好消息，不提示任何信息说明启动成功。

或者也可以输入如下命令，查看 nginx 是否有服务正在运行：

>ps -ef | grep ngix
>
>lsof -i:80

然后我们在浏览器输入Linux系统的IP地址，出现windows安装成功的界面即可。

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633769883000.png)

或者输入curl命令：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633771490000.png)

#### 关闭

有两种方式：

  - 方式1：快速停止

    >cd /usr/local/nginx/sbin
    >
    >./nginx -s stop

    此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。不太友好。

  - 方式2：平缓停止

    >cd /usr/local/nginx/sbin
    >
    >./nginx -s quit

    此方式是指允许 nginx 服务将当前正在处理的网络请求处理完成，但不在接收新的请求，之后关闭连接，停止工作。

#### 重启

- 方式1：先停止再启动

  >./nginx -s quit
  >
  >./nginx

  相当于先执行停止命令再执行启动命令。

- 方式2：重新加载配置文件

  >./nginx -s reload

  通常我们使用nginx修改最多的便是其配置文件 nginx.conf。修改之后想要让配置文件生效而不用重启 nginx，便可以使用此命令。

#### 检查配置文件语法是否正确

- 方式1：通过如下命令，指定需要检查的配置文件

  >./nginx -t -c  /usr/local/nginx/conf/nginx.conf

- 方式2：通过如下命令，不加 -c 参数，默认检测nginx.conf 配置文件。

  >./nginx -t
