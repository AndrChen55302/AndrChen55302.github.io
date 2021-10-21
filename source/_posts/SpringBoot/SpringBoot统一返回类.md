---
title: SpringBoot统一返回类
date: 2021-09-28
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633779120000.webp
tags: SpringBoot集成
categories: SpringBoot
---
## 前言

在进行接口开发时，一般需要一个固定的返回样式，成功和失败的时候，都按照这种格式来进行统一的返回，这样，在与其他人进行接口之间的联调时不会显得很杂乱无章。而这种固定的格式如果放在Java的每个接口单独处理时，又会在接口开发时很繁琐，所以这个时候可以采用封装一个实体类，统一返回固定模板格式的内容。

## 封装模板

先看一下没有封装之前，接口代码和返回格式：

request

```class
/**
 * 用户修改
 * @return 返回修改的用户信息
 */
@PutMapping(value = "update")
public User update(@RequestBody User user) {
		User updatedUser = userService.update(user);
		return updatedUser;
}
```

responese

```java
{
	"userId": "0d67cfa7-f6a1-46b6-8e5a-b605afc98c44",
	"username": "ww",
	"password": "123456",
	"status": 0,
	"createTime": 310863886132307,
	"updateTime": 312955781619836
}
```

很显然，这种原始的内容返回虽然很直观，但是如果在发生错误的时候，那么接口的返回就比较的不自然了，甚至会将底层的错误对外暴露，下面介绍下一个简单的统一接口样式的封装：

### 枚举类CodeResultEnums：定义返回码code及提示信息msg

```enum
/**
 * @Author: ThinkPad
 * @Creation: 2021/9/15 11:20
 * @Desc: 状态码管理
 */
public enum CodeResultEnums {
    //通用的状态码
    SUCCESS(200,"成功"),
    ERROR(400,"失败"),

    //自定义的状态码
    USER_CODE_EXIST(50001,"用户名已被使用"),//用于注册
    USER_LOGIN_NULL(50002,"用户或密码为空"),//用于登录
    USER_LOGIN_EXIST(50003,"用户名不存在"),//用于登录
    ;

    private Integer code;
    private String massage;


    CodeResultEnums(Integer code, String massage) {
        this.code = code;
        this.massage = massage;
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMassage() {
        return massage;
    }

    public void setMassage(String massage) {
        this.massage = massage;
    }
}
```

### 控制器返回值类型

```class
/**
 * @Author: ThinkPad
 * @Creation: 2021/9/15 12:22
 * @Desc: 控制器返回值
 */
@Data
public class R<T> {

    private Integer code;
    private String message;
    private T data;

    //1.构造函数->自定义状态码
    public R() {
    }

    public R(Integer code, String message, T data) {
        this.code = code;
        this.message = message;
        this.data = data;
    }

    public R(Integer code, String message) {
        this.code = code;
        this.message = message;
    }

    //2.构造函数->引用枚举做状态码
    public R(CodeResultEnums codeResultEnums) {
        this.code= codeResultEnums.getCode();
        this.message = codeResultEnums.getMassage();
    }
    public R(CodeResultEnums codeResultEnums, T data) {
        this.code= codeResultEnums.getCode();
        this.message = codeResultEnums.getMassage();
        this.data = data;
    }
}
```

### 统一返回的实体类型

```responese
/**
 * @Author: ThinkPad
 * @Creation: 2021/9/15 13:42
 * @Desc: 统一返回的实体类型
 */
public class ResponseDataUtils {
    public static <T> R buildSuccess(T data) {
        return new R<T>(CodeResultEnums.SUCCESS, data);
    }

    public static R buildSuccess() {
        return new R(CodeResultEnums.SUCCESS);
    }

    public static R buildSuccess(String msg) {
        return new R(CodeResultEnums.SUCCESS.getCode(), msg);
    }

    public static R buildSuccess(Integer code, String msg) {
        return new R(code, msg);
    }

    public static <T> R buildSuccess(Integer code, String msg, T data) {
        return new R<T>(code, msg, data);
    }

    public static R buildSuccess(CodeResultEnums CodeResultEnums) {
        return new R(CodeResultEnums);
    }

    public static <T> R buildError(T data) {
        return new R<T>(CodeResultEnums.ERROR, data);
    }

    public static R buildError() {
        return new R(CodeResultEnums.ERROR);
    }

    public static R buildError(String msg) {
        return new R(CodeResultEnums.ERROR.getCode(), msg);
    }

    public static R buildError(Integer code, String msg) {
        return new R(code, msg);
    }

    public static <T> R buildError(Integer code, String msg, T data) {
        return new R<T>(code, msg, data);
    }

    public static R buildError(CodeResultEnums CodeResultEnums) {
        return new R(CodeResultEnums);

    }
}
```

### controller

```controller
@ApiOperation("查询所有用户信息")
    @GetMapping("/getAll")
    public R getUserList(){
        return ResponseDataUtils.buildSuccess(userService.list());
    }
```




