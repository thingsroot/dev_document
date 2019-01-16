
# 后台API接口规范

本规范适用于冬笋物联后台服务的所有HTTP/Web API接口。

> 版本: v1.0

## 原则

API路径示例:

>
> ```https://ioe.thingsroot.com/api/v1/METHOD_FAMLIY.method```
> ```https://ioe.thingsroot.com/api/master/METHOD_FAMLIY.method```

路径命名并不遵守REST API的标准，而且仅使用GET/POST的HTTP方法。

服务接口对外强制要求使用HTTPS和SSL，以保证安全性。

每个方法都需要一系列的参数来定义你要执行的目标, 其基本原则为：

* GET 请求使用Query String参数
* POST 请求使用JSON数据 ```application/json```
* 或者 POST/GET 使用表单数据 ```application/x-www-form-urlencoded```, 某些方法并不一定能够支持表单模式。
* [files.upload] 使用 ```mutipart/form-data``` 提交文件, 而其他参数使用表单 ```application/x-www-form-urlencoded``` 数据格式
* HTTP头部的Accept字段必须是 ```application/json```，当前所有接口返回的数据都是JSON对象

一些接口方法，例如使用数据作为参数的方法，并不适合使用 ```application/x-www-form-urlencoded``` 表单数据来构造，所以请尽量在使用API的时候使用JSON数据 ```application/json```

### POST 数据

尽量使用JSON数据，而不是表单数据。 解析JSON数据错误时，服务器将返回:

* ```invalid_json``` - 无法解析POST中的JSON数据，或者Content-Type并不是正确的值
* ```json_not_object``` - JSON数据的根节点并不是字典对象，后台需要将JSON对象的一系列的KEY作为接口的参数列表一一对应起来使用。当遇到数据、字符串、数字等根节点类型则将返回此错误信息。

## 接口返回处理

所有的HTTP/Web API返回的数据都包含一个JSON对象，而且包含一个顶层的boolean属性 ```ok```, 表示接口执行成功与否。

当执行失败时，JSON对象中的 ```error``` 属性是一个简单程序可读的字符串错误码。 即使当ok为true时，服务也可以通过 ```warning``` 字段提示警告信息，格式为字符串的警告码或警报码数组。示例:

```json
{
    "ok": true,
    "stuff", "something_done"
}
```

```json
{
    "ok": false,
    "error": "somthing_bad"
}
```

```json
{
    "ok": true,
    "warning": "something_problematic",
    "stuff": "something_done"
}
```

### i18n支持

所有的错误码，警报码都必须是简单的程序可读的英文字符串，并且仅包含小写字符以及下划线(_)。 所有接口描述中包含本接口可能出现的错误码（标准错误、警告除外）

所有非数据库来源的字符串也需要遵守上述约定，方便Web页面提供i18n的支持。

## 用户认证

用户认证方式:

* 不使用Cookie的时候，要在请求中包含Authentication头部，内容为用户的Access Token
* 使用Cookie进行认证的时候，需要在头部包含csrf-token。

## 使用 Open API 标准

我们使用Open API 标准的[3.0.2版本](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md) 来规范接口。

> [下载ThingsRoot接口文件](thingsroot_openapi.json)  
> [ThingsRoot接口说明](thingsroot_openapi.md)  
> [EOLinker 系统](https://api.thinsroot.com) -- 仅限内部使用

## 其他设计要求

API设计主要参考slack的Web API，同时也需要参考 HTTP API Design里面的一些设计思路。

## 参考资料

* [Slack Web API 文档 | Slack Web Api Basics](https://api.slack.com/web#basics)
* [OpenAPI标准文档 | OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification)
* [HTTP API设计原则 | HTTP API Design](https://github.com/interagent/http-api-design)
* [OpenAPI 编辑器 | OpenAPI Editor](https://github.com/thingsroot/openapi-gui)
* [OpenAPI 查看测试工具 | Angular-Swagger-UI](https://github.com/thingsroot/angular-swagger-ui)

## 修订历史

| 版本 | 修订人 | 说明 | 日期 |
| -- | -- | -- | -- |
| v1.0 | Dirk Chang | Initial version | 01/16/2019 |