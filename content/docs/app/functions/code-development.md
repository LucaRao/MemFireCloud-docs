---
    weight: 53
    title: "代码开发"
    description: "Getting started with code development."
    icon: "article"
    draft: false
    toc: true
---

本文介绍使用云函数编写代码的相关基础概念，包括请求处理函数、函数实例生命周期回调方法等。

## **请求处理程序**

MemFire Cloud云函数的请求处理程序，是函数代码中处理请求的方法。当您的云函数被调用时，会运行您提供的Handler方法处理请求。对于Node.js语言的云函数而言，函数入口为 [文件名].[函数名]，默认为index.handler；

您可以使用HTTP请求处理程序更方便地处理HTTP请求。当调用函数时，MemFire Cloud云函数使用您提供的执行方法来处理HTTP请求。



### **HTTP Handler签名**

Node.js的HTTP Handler的签名如下。您只需实现一个函数，就能响应HTTP请求。

```JavaScript
exports.handler = (req, resp, context) => {
    console.log("receive body: ", req.body.toString());
    resp.setHeader("Content-Type", "text/plain");
    resp.send('<h1>Hello, world!</h1>');
}     
```

示例解析如下：

- `handler`：HTTP Handler名称。
- `req`：HTTP请求结构体。
- `resp`：HTTP返回结构体。
- `context`：上下文信息。


### **HTTP请求结构体**

{{< table "table-striped-columns" >}}

| **字段** | **类型** | **描述**                                                     |
| -------- | -------- | ------------------------------------------------------------ |
| headers  | Object   | 存放来自HTTP客户端的键值对。                                 |
| path     | String   | 表示HTTP路径。                                               |
| queries  | Object   | 存放来自HTTP路径中的查询部分的键值对，值的类型可以为字符串或数组。 |
| method   | String   | 表示HTTP方法。                                               |
| clientIP | String   | 客户端的IP地址。                                             |
| url      | String   | 请求的地址。                                                 |
 {{< /table >}}
**说明** Headers键值对中的`key`中包含以下字段或以`x-fc-`开头的`key`均会被忽略，因此，不支持自定义。

- connection
- keep-alive


### **HTTP响应结构体**
{{< table "table-striped-columns" >}}
| **字段**                                   | **类型**                        | **描述**     |
| ------------------------------------------ | ------------------------------- | ------------ |
| response.setStatusCode(statusCode)         | interger                        | 设置状态码。 |
| response.setHeader(headerKey, headerValue) | String，String                  | 设置响应头。 |
| response.deleteHeader(headerKey)           | String                          | 删除响应头。 |
| response.send(body)                        | Buffer，String，Stream.Readable | 发送响应体。 |
 {{< /table >}}
**重要** 

`headerKey`中包含以下字段或以`x-fc-`开头的`headerKey`均会被忽略，因此，不支持自定义。

- connection
- content-length
- date
- keep-alive
- server
- upgrade



### **限制说明**

- 请求限制
- 如果超过以下限制，会返回**400**状态码和**InvalidArgument**错误码。
{{< table "table-striped-columns" >}}
| **字段** | **限制说明**                                                 | **HTTP状态码** | **错误码**      |
| -------- | ------------------------------------------------------------ | -------------- | --------------- |
| headers  | 请求头中的所有键和值的总大小不能超过4 KB。                   | 400            | InvalidArgument |
| path     | 请求路径以及所有查询参数的总大小不能超过4 KB。               |                |                 |
| body     | 同步调用请求的Body的总大小不能超过16 MB，异步调用请求的Body的总大小不能超过128 KB。 |                |                 |
 {{< /table >}}
- 响应限制
- 如果超过以下限制，会返回**502**状态码和**BadResponse**错误码。
{{< table "table-striped-columns" >}}
| **字段** | **限制说明**                               | **HTTP状态码** | **错误码**  |
| -------- | ------------------------------------------ | -------------- | ----------- |
| headers  | 响应头中的所有键和值对的大小不能超过4 KB。 | 502            | BadResponse |
 {{< /table >}}

### **示例：获取HTTP请求详细信息并返回Body**

#### **示例代码**

```Java
module.exports.handler = function (request, response, context) {
    // get requset headervar reqHeader = request.headers
    var headerStr = ' 'for (var key in reqHeader) {
        headerStr += key + ':' + reqHeader[key] + '  '
    };

    // get request infovar url = request.url
    var path = request.path
    var queries = request.queries
    var queryStr = ''for (var param in queries) {
        queryStr += param + "=" + queries[param] + '  '
    };
    var method = request.method
    var clientIP = request.clientIP
    var body = request.body
    var respBody = new Buffer('requestHeader:' + headerStr + '\n' + 'url: ' + url + '\n' + 'path: ' + path + '\n' + 'queries: ' + queryStr + '\n' + 'method: ' + method + '\n' + 'clientIP: ' + clientIP + '\n' + 'body: ' + body + '\n')
    response.setStatusCode(200)
    response.setHeader('content-type', 'application/json')
    response.send(respBody)
};
```

## **函数实例生命周期回调方法**

MemFire Cloud云函数支持Initializer生命周期回调方法。

### **Initializer回调**

您可以将数据库场景下连接池构建、函数依赖库加载等耗时较长的业务逻辑放到Initializer回调中，避免每次运行函数都会做重复的操作，降低函数延时。

初始化回调程序（Initializer回调）在函数实例启动成功之后，运行请求处理程序（Handler）之前执行。函数计算保证在一个实例生命周期内，成功且最多成功执行一次Initializer回调。例如，您的Initializer回调第一次执行失败后系统会重试，直到成功为止，然后再执行您的请求处理程序。

Initializer回调只有一个**context**输入参数，使用方法和事件请求处理程序一样。

一个最简单的Initializer回调如下所示。

```JavaScript
exports.initialize = function(context, callback) {
  console.log('initializer');
  callback(null, "");
};
```

MemFire Cloud平台默认的**Initializer 回调程序**为`index.initialize`，那么云函数在配置Initializer属性后会去加载`index.js`中定义的`initialize`方法。


### **方法签名**

- 输入参数只有`context`，为您的云函数调用提供在调用时的运行上下文信息。
- 无返回值。

### 函数调用说明

- **前提：**当用户想通过路由传输类似于/a/b这样的path信息时，那么我们建议使用查询参数（query parameter）来实现这一目的。
- **使用：**在该场景下，用户应该将路径信息改为/a，并在该路径后添加查询参数?action=b，以便服务器可以正确地解析和处理请求。


