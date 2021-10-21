---
title: natapp
date: 2021-10-21
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1634803480000.webp
tags: natapp
categories: SpringBoot
---

# natapp的使用

### 1.natapp是干什么的？
(1).在进行微信公众号开发时，我们需要搭建网站，并且随时都有可能修改网站内容进行调试。如果能够将内网ip映射到外网上，将大大方便我们的调试。每次发布只需eclipse/Idea运行应用即可。
(2).通过natapp将内网映射到外网，还可以方便我们其他工作，比如外网展示网站等。
总之一句话，我们使用natapp主要是用来实现内网穿透。便于开发。

### 2.下载

打开==> [NATAPP-内网穿透 基于ngrok的国内高速内网映射工具](https://natapp.cn/) 进入官网

![image-20211021113255472](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021113255472.png)



点击导航栏的==客户端下载==，选择需要的版本，本次演示使用的是 windows 64位，点击后即可下载

![image-20211021113535251](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021113535251.png)



### 3.注册登录

接下来我们开始注册

![image-20211021113839637](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021113839637.png)

界面如下，进行注册

![image-20211021113920751](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021113920751.png)

注册完成之后，点击登录进入一下界面

![image-20211021114108360](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021114108360.png)

之后我们要进行实名认证，

![image-20211021114404394](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021114404394.png)

### 4.购买隧道与配置

认证后，点击购买隧道

![image-20211021114433644](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021114433644.png)

![image-20211021114447087](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021114447087.png)

选择安全隧道，进行配置。这里我使用的是Web协议和80端口，完成之后点击免费购买

![image-20211021114557586](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021114557586.png)



配置购买成功的隧道

下图是我购买成功的隧道，这是我申请的一个免费隧道，如果购买了其他的隧道也会在下面显示

![image-20211021115012476](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021115012476.png)

点击复制复制获取你的token

``这里点击"点击复制"可能没用(那为什么要加一个复制按钮····)，可以点击显示，然后手动复制``

![image-20211021140826248](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021140826248.png)

### 5.启动

接下来将我们之前下载的natapp解压出来

![image-20211021141308596](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021141308596.png)

然后双击启动，效果如下图

![image-20211021141341981](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021141341981.png)

 然后在这个cmd窗口输入命令**natapp -authtoken 你的token**，回车运行

注意：这个token就是刚才在免费渠道获取的那个

> natapp -authtoken 你的token

显示效果如下

![image-20211021142139003](https://cdn.jsdelivr.net/gh/get103/image1/imgimage-20211021142139003.png)

```
Forwarding              http://ac4esy.natappfree.cc -> 127.0.0.1:80   
```

此时http://ac4esy.natappfree.cc就映射到了你本地的80端口，需要映射到其他端口，可以在natapp进行配置。

到了这一步，你的电脑已经映射到外网了。







