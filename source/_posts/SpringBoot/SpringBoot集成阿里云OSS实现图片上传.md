---
title: SpringBoot集成阿里云OSS实现图片上传
date: 2021-10-14
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633779120000.webp
tags: SpringBoot集成
categories: SpringBoot
---
# SpringBoot集成阿里云OSS实现图片上传

## 前言

如果需要上传到服务器的图片过多，而且还不想上传到自己服务器，可以使用阿里云的OSS服务器来存储

集成第三方平台的话，需要好好看对方的API文档！

## 前置条件

- 在在阿里云上注册账号并开通OSS服务

- 有一个Bucket并设置设置读写权限为公共读写其余选项视情况而定

## 整合

### 1、记录重要数据

- Endpoint 地域结点、
- ![image-20211014145150214](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/image-20211014145150214.png)
- 记录创建成功的AccessKeyId 和 AccessSecret
- ![image-20211014150011539](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20211014150011539.png)
- 上面创建Bucket时所取得名字 BucketName

### SpringBoot整合

> **文章目录如下:**

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1634195690000.png)

####  在pom文件中引入依赖

```pom
        <!-- 日期工具栏依赖 -->
        <dependency>
            <groupId>joda-time</groupId>
            <artifactId>joda-time</artifactId>
            <version>2.10.1</version>
        </dependency>

        <!-- 引入 Hutool工具类 -->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.6.6</version>
        </dependency>
```

#### 配置application.yml

```yml
#阿里云OSS配置
aliyun:
  oss:
    file:
      bucketName: endure-oss-demo
      endpoint: http://oss-cn-beijing.aliyuncs.com
      keyid: LTAI5tLupJD6Uzbu5Hoe3xra
      keysecret: xa0o6LP2YS8r6j8m1D0Jb5K4ResJYF

```

#### 创建工具类

```class
package com.yuntu.utils;


import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class ConstantPropertiesUtils implements InitializingBean {
    //读取配置文件内容
    @Value("${aliyun.oss.file.endpoint}")
    private String endpoint;

    @Value("${aliyun.oss.file.keyid}")
    private String keyId;

    @Value("${aliyun.oss.file.keysecret}")
    private String keySecret;

    @Value("${aliyun.oss.file.bucketname}")
    private String bucketName;


    //定义公开静态常量
    public static String END_POIND;
    public static String ACCESS_KEY_ID;
    public static String ACCESS_KEY_SECRET;
    public static String BUCKET_NAME;


    @Override
    public void afterPropertiesSet() throws Exception {
        END_POIND = endpoint;
        ACCESS_KEY_ID = keyId;
        ACCESS_KEY_SECRET = keySecret;
        BUCKET_NAME = bucketName;
    }


}
```

#### 创建文件上传接口

```service
package com.yuntu.service;

import org.springframework.web.multipart.MultipartFile;

public interface OssService {
    //上传头像到oss
    String uploadFileAvatar(MultipartFile file);
}

```

#### 实现上传功能接口实现类

```impl
@Service
public class OssServiceImpl implements OssService {
    //上传头像到oss
    @Override
    public String uploadFileAvatar(MultipartFile file) {
        // 工具类获取值
        String endpoint = ConstantPropertiesUtils.END_POIND;
        String accessKeyId = ConstantPropertiesUtils.ACCESS_KEY_ID;
        String accessKeySecret = ConstantPropertiesUtils.ACCESS_KEY_SECRET;
        String bucketName = ConstantPropertiesUtils.BUCKET_NAME;

        try {
            // 创建OSS实例。
            OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

            //获取上传文件输入流
            InputStream inputStream = file.getInputStream();
            //获取文件名称
            String fileName = file.getOriginalFilename();

            //1 在文件名称里面添加随机唯一的值
            String uuid = UUID.randomUUID().toString().replaceAll("-","");
            // yuy76t5rew01.jpg
            fileName = uuid+fileName;

            //2 把文件按照日期进行分类
            //获取当前日期
            String datePath = new DateTime().toString("yyyy/MM/dd");
            //拼接
            fileName = datePath+"/"+fileName;

            //调用oss方法实现上传
            //第一个参数  Bucket名称
            //第二个参数  上传到oss文件路径和文件名称   aa/bb/1.jpg
            //第三个参数  上传文件输入流
            ossClient.putObject(bucketName,fileName , inputStream);

            // 关闭OSSClient。
            ossClient.shutdown();

            //把上传之后文件路径返回
            //需要把上传到阿里云oss路径手动拼接出来
            String url = "https://"+bucketName+"."+endpoint+"/"+fileName;
            return url;
        }catch(Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

#### 在Controller 中调用获得图片的url

```controller
@RestController
@Api("阿里云OSS图片上传Controller")
@CrossOrigin
public class OssController {
    @Resource
    private OssService ossService;
    //上传头像的方法
    @ApiOperation("图片上传")
    @PostMapping("/imgUpload")
    public String uploadOssFile(MultipartFile file) {

        //获取上传文件  MultipartFile
        //返回上传到oss的路径
        String url = ossService.uploadFileAvatar(file);
        return url;
    }
}
```

#### 测试

- 自己用postman测试不知道为什么获取不到文件
- 用vue和element-ui写了个图片上传
- ![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/image-20211014162209221.png)
- 完成！

  



