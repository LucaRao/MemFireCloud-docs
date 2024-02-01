---
    weight: 20
    title: "快速开始"
    description: "快速开始"
    icon: "article"
    draft: false
    toc: true
---

## 使用说明

配置静态网站托管时，您需要指定网站的默认首页index.html。

```js

 ├── index.html
 ├── error.html
 ├── example.txt
 └── subdir/
      └── index.html

```

## 准备工作

- 拥有MemFire Cloud账号;
- 已完成MemFire Cloud平台实名认证;
- 创建MemFire Cloud应用开发。


## 步骤1：写一个简单的HTML

下面我们创建一个简单的HTML文件，命名为index.html：

```js
<html><head><meta charset="utf-8" /></head><body>
    Hello MemFire Cloud!
  </body></html>

```

## 步骤2：打包压缩

打包压缩index.html文件，其中压缩包必须为ZIP格式。   
打包方式：先进入您的代码目录，在全选所有文件以后，单击鼠标右键，选择压缩为 ZIP 包，生成代码包。或者您也可以在代码包的根目录下执行```zip -rq -y code.zip ./```命令进行打包。   
Linux，Unix的系统环境下，使用zip命令打包，不要使用tar命令；     
备注说明：公测环境下，压缩包大小不超过20MB。

## 步骤3：上传压缩包，完成部署

1.登录MemFire Cloud平台，进入我的应用->某应用->静态托管页面。

<img src="/docs/img/static1.png">

2.点击上传文件，选中网站压缩包进行上传。

<img src="/docs/img/static2.png">

完成上传操作后，用户即可通过MemFire Cloud平台提供的**访问地址**进行访问托管的静态网站。


