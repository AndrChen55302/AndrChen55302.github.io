---
title: SpringBoot集成Hutool工具类
date: 2021-09-30
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633779120000.webp
tags: SpringBoot集成
categories: SpringBoot
---
## 前言

在用户进行注册时，需要填写密码 这个密码我们不能直接将密码存入数据库中 我们需要使用一些工具对其进行加密,再存入。

### 引入依赖

在pom文件里添加

```x
        <!-- 引入 Hutool工具类 -->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.6.6</version>
        </dependency>
```

### 使用Hutool

编写实体类 User

```class
package com.yuntu.model;

import lombok.Data;

@Data
public class User {
    private String username;
    private String password;
}

```

编写controller

```controller
import cn.hutool.crypto.SecureUtil;


@RestController
@Slf4j
public class UserController {

    @PostMapping("/register")
    public User saveUser(@RequestBody User user){

        String md5Password = SecureUtil.md5(user.getPassword());
        user.setPassword(md5Password);
        log.info(user.toString());
        return user;
    }
}

```

使用postman测试

http://localhost:8080/register

参数类型为 JSON

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633778791000.jpg)

返回经过加密的密码

>{
>
>  "username": "zhangsan",
>
>  "password": "e10adc3949ba59abbe56e057f20f883e"
>
>}




