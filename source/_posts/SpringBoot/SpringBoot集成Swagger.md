---
title: SpringBoot集成Swagger
date: 2021-09-30
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633779120000.webp
tags: SpringBoot集成
categories: SpringBoot
---
## 前言

Springfox Swagger能够动态生成 API 接口供先后端进行交互和在线调试接口

### Swagger简介

 Swagger是什么？官方说法：Swagger是一个规范和完整的框架，用于建立、描述、调试和可视化 RESTful 风格的 Web 服务。通俗地说，Swagger 是一个主要用来在线建立文档的插件，主要用来高质量地动态生成 API 接口供先后端进行交互和在线调试接口，若是不生成的话就须要写静态文档来交互，那样不只速度慢并且不容易修改。发现了痛点就要寻找解决方案，故Swagger应运而生。java

  Springfox Swagger是Spring 基于swagger规范，能够将基于SpringMVC和Spring Boot项目的源码自动生成JSON格式的描述文件。自己不是属于Swagger官网提供的。Spring Boot 框架是目前很是流行的微服务框架，因此，在Spring Boot 项目中集成Springfox很是有意义，能够保证及时更新API文档，下降先后端沟通成本，提升系统迭代效率。

### 添加pom依赖

若想为Spring Boot项目添加无需任何配置的springfox，需引入其maven依赖库：

```xml
        <!--整合swagger的starter-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>
```

### 配置类

```class
package com.yuntu.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

@Configuration
public class SwaggerConfig {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors
                        .basePackage("com.yuntu.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Auth认证系统APi")
                .version("Version 1.0")
                .description("Auth认证系统")
                .contact(
                        new Contact(
                                "Accp",
                                "http://www.sean.com.cn",
                                "188@163.com"))
                .build();
    }
}
```

>@Configuration标注在类上，相当于把该类作为spring的xml配置文件中的`<beans>`，作用为：配置spring容器(应用上下文)

### 丰富文档内容

在完成了上述配置后，已经算是初步集成Springfox Swagger3，能够启动服务查看API文档内容了。可是这样的文档主要针对请求自己，描述信息的主要来源是函数的命名，对用户并不友好，咱们一般须要使用swagger注解增长一些说明文字来使得API文档内容更加丰满。Swagger中经常使用的注解及其说明：

- @Api：用在类上，说明该类的做用。
- @ApiOperation：为API增长方法说明。
- @ApiImplicitParams : 用在方法上包含一组参数说明。
- @ApiImplicitParam：给方法入参增长说明。
- @ApiResponses：用于表示一组响应
- @ApiResponse：用在@ApiResponses中，通常用于表达一个错误的响应信息
  -  l  code：数字，例如400
  -  l  message：信息，例如"必填参数不可为空"
  -  l  response：抛出异常的类  

- @ApiModel：描述一个Model的信息（通常用在请求参数没法使用@ApiImplicitParam注解进行描述的时候）
  - @ApiModelProperty：描述一个model的属性

### 案例

修改API实体类User，添加swagger注解：

```class
package com.yuntu.model;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import java.io.Serializable;

@Data
@ApiModel("用户实体")
public class User implements Serializable {

    @ApiModelProperty("用户名")
    private String username;
    @ApiModelProperty("用户密码")
    private String password;
}

```

在实体类使用注解@ApiModel和 @ApiModelProperty能够添加属性备注，这些备注信息将展现在swagger页面的Schema中，效果以下：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633780720000.png)

在 controller 包下新建 UserController.java 类，经过在控制器类上增长 @Api 注解，能够给控制器添加标签信息。经过在接口方法上增长 @ApiOperation 注解来添加对接口的描述。

```controller
package com.yuntu.controller;

import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/test")
@Api(value = "myApi", tags = "API测试接口")
public class TestController {

    @ApiOperation("测试接口1")
    @GetMapping("/test1")
    public String test1(@ApiParam("测试参数1") String test){
        return "test1";
    }

    @ApiOperation("测试接口2")
    @PostMapping("/test2")
    public String test2(@ApiParam("测试参数2") String test){
        return "test2";
    }

}
```

controller效果图如下

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633780782000.png)

这个时候已经算是进一步集成Springfox-Swagger3了，启动项目后访问`http://IP:port/auth/swagger-ui/index.html`能够看到咱们的swagger文档界面，其中，IP表示服务器IP地址，port表示项目配置的端口号，your-app-root表示项目配置的根路径。在个人项目中，其访问路径是`http://127.0.0.1:8080/auth/swagger-ui/index.html`，在浏览器访问此URL进入以下文档界面：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633912338000.png)

### 使用 SwaggerUI访问API

 找到用户查询API test1并进入接口详情页面，能够在详情页面右侧看到** Try it out** 按钮，单击此按钮便可进入接口调用界面，Execte 单击执行便可请求test1方法：

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633912507000.png)

返回结果

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633912617000.png)





