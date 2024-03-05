---
    weight: 157
    title: "helloworld"
    description: "Getting started with helloworld."
    icon: "article"
    draft: false
    toc: true
---


本文介绍如何在MemFire Cloud控制台快速开发一个简单的 Hello World应用。MemFire Cloud为您提供一个简单的Hello World示例，你可以在下载该示例到本地，并通过控制台使用该示例创建函数。创建成功后，您可以测试该函数，查看函数执行的结果。

## **使用说明**

关于以下示例的使用方法，请参见对应代码库的readme.md文件。

## 前提条件

- 拥有MemFire Cloud账号;

- 创建MemFire Cloud应用开发。

## 操作步骤

### 步骤1：准备代码包

准备好Hello World样例，并打包ZIP文件。

Helloword的代码包地址详见：[MemFire / memfire-fc-helloworld · 极狐GitLab](https://jihulab.com/memfire/memfire-fc-helloworld/)

编写的Hello World样例，只有一个index.js文件，代码如下：

```JavaScript
var getRawBody = require('raw-body');
var getFormBody = require('body/form');
var body = require('body');

/*
please implement the initializer function as below：
exports.initializer = (context, callback) => {
  console.log('initializing');
  callback(null, '');
};
*/

exports.handler = (req, resp, context) => {
    console.log('hello world');

    var params = {
        path: req.path,
        queries: req.queries,
        headers: req.headers,
        method : req.method,
        requestURI : req.url,
        clientIP : req.clientIP,
    }

    getRawBody(req, function(err, body) {
        for (var key in req.queries) {
          var value = req.queries[key];
          resp.setHeader(key, value);
        }
        resp.setHeader('Content-Type', 'application/json');
        params.body = body.toString();
        resp.send(JSON.stringify(params, null, '    '));
    });

    /*
    getFormBody(req, function(err, formBody) {
        for (var key in req.queries) {
          var value = req.queries[key];
          resp.setHeader(key, value);
        }
        params.body = formBody;
        console.log(formBody);
        resp.send(JSON.stringify(params));
    });
    */
}
```

### 步骤2：创建云函数

1、登录MemFire Cloud控制台，进入我的应用->某应用->云函数页面。

2、在云函数页面，单击**新建函数**；

3、在创建云函数弹框中，输入函数名称helloworld，并上传步骤1中准备好的ZIP代码包，无需配置环境变量，不开启initializer回调，然后单击**保存**；

<img src="../../../img/helloworld-1.png">

- **基础信息**：设置函数的基本信息，包括函数名称、请求处理程序。
{{< table "table-striped-columns" >}}
| **配置项**   | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| 函数名       | 函数名称，必填项；只能包含字母、数字、下划线；不能以数字开头，长度在 1-64 之间；同一应用下的云函数不能重名； |
| 请求处理程序 | 即函数入口，默认为index.handler；在“运行环境”为 Node.js 时，当前值的格式为 [文件名].[函数名]； |
 {{< /table >}}
- **函数配置**：配置函数的代码和运行环境。
{{< table "table-striped-columns" >}}
| **配置项**   | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| 上传代码包   | 支持ZIP包上传代码；ZIP文件大小不能超过20Mb, 且index.js必须在ZIP文件的根目录下，采用handler函数为入口函数； |
| 运行环境     | 目前MemFire Cloud云函数仅支持JavaScript语言，支持的运行环境包括：Node.js 12Node.js 14Node.js 16 |
| 执行超时时间 | 设置超时时间。执行超时时间默认为60秒，最长为86400秒。建议您将此值设置为 600 秒。如果函数在这个时间内未能成功执行，函数计算会返回超时错误，请设置大小合适的超时时间，避免函数执行超时。 |
| 访问方法     | 访问方法包括：GET、POST、PUT、DELETE、OPTIONS、HEAD、PATCH   |
| 函数描述     | 函数描述信息；                                               |
 {{< /table >}}


- **环境变量**：配置函数运行环境中的环境变量。可以配置多个环境变量，详见：[环境变量](/docs//guides/functions/variable)
- **函数生命周期回调**：配置函数实例的生命周期回调，**初始化（Initializer）函数**；
  - 在函数实例启动成功之后，运行请求处理程序（Handler）之前执行；
  - 云函数保证在一个实例生命周期内，成功且只成功执行一次Initializer回调；


{{< table "table-striped-columns" >}}
| **配置项**          | **说明**                                                     |
| ------------------- | ------------------------------------------------------------ |
| Initializer程序入口 | 在“运行环境”为 Node.js 时，当前值的格式为 [文件名].[函数名]；Initializer函数执行入口，默认为index.Initializer； |
| Initializer超时时间 | 执行 Initializer 回调程序的超时时间，开启Initializer回调时为必填项，默认为3，单位秒。最小1秒，最长5分钟，即300秒；初始化函数超过这个时间后会被终止执行； |
 {{< /table >}}


4、创建成功后，可以在函数列表中查看helloworld函数。

<img src="../../../img/helloworld-2.png">

### 步骤3：执行函数

1、在云函数页面，复制helloworld函数访问地址URL。

2、打开Postman工具，新建一个请求窗口，粘贴到Postman请求里。

3、点击“Send”按钮，即可获得返回结果。

<img src="../../../img/helloworld-3.png">



