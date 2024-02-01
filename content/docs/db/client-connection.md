---
weight: 7
title: "客户端工具"
description: ""
icon: "article"
date: "2023-12-13T17:39:49+08:00"
lastmod: "2023-12-13T17:39:49+08:00"
draft: false
toc: true
---





MemFireDB兼容兼容PostgreSQL 11.2版本, MemFire Cloud几乎兼容所有PostgreSQL的客户端，目前已确认支持的客户端工具如下:   
{{< table "table-striped" >}}
| 客户端名称                                                   | 运行环境                  | 安装、下载地址                                                |
| ------------------------------------------------------------ | ------------------------- | ------------------------------------------------------------ |
| psql                                                         | Linux环境                 | 命令：yum -y install postgresql11                            |
| DbGate                                                       | Windows、Linux、MacOS环境 | https://dbgate.org/                                          |
| dbeaver                                                      | Windows、MacOS环境        | https://dbeaver.io/files/7.1.0/                              |
| datagrip                                                     | Windows、Linux、MacOS环境 | https://www.jetbrains.com/datagrip/download/#section=windows |
| Navicat Premium                                              | Windows、Linux、MacOS环境 | 链接：https://pan.baidu.com/s/17r_oHwjeiC6Pqdq2c8yFLQ  提取码：s4l7   |
| beekeeper-studio                                             | Windows、Linux、MacOS环境 | https://www.beekeeperstudio.io/get                           |
| HeidiSQL                                                     | Windows环境               | https://www.heidisql.com/                                    |
{{< /table >}}

  
   MemFire Cloud提供密码认证、证书认证两种云数据库服务，接下来本文会举例介绍不同的环境下，如何安装、配置客户端来连接访问MemFire Cloud数据库。   

---

## Windows环境
### dbeaver使用SSL连接MemFireDB
以dbeaver7.1.0为例，介绍如何进行配置SSL连接MemFireDB。  

1. 下载软件。dbeaver7.1.0软件:[下载地址](https://github.com/dbeaver/dbeaver/releases/download/7.1.0/dbeaver-ce-7.1.0-win32.win32.x86_64.zip)    

2. 注册登录MemFire Cloud平台。 登录[MemFire Cloud](https://memfiredb.com/)平台完成新用户的注册。  

3. 创建数据库。   
进入数据库管理页面，点击“创建数据库”按钮，在”创建数据库“弹框中填写“数据库名称”，选择数据库账号，勾选“证书认证”，最后点击“确定”完成数据库创建操作。  
<div  style="width:1200px;display:block; background: rgba(22,184,248,.1); font-size:14px" >
<p style="line-height:50px; padding: 0 0 0 15px">
备注说明：   
① 创建数据库名称不能和已有数据库重复；   
② 如果没有数据库账号，则先创建数据库账号;  </p>
</div>   

4. 取连接信息。   
选中数据库列表中某个数据库所在列，点击“在线连接”按钮，弹出“连接信息”弹框；  
可以查看该数据库的访问IP以及端口信息，当数据库为证书认证方式时，则提供证书下载链接，包括DER格式、PEM格式证书；  
![avatar](../_media/w_dbeaver_ssl_1.png)  
① 采用JAVA使用JDBC连接访问MemFire Cloud数据库时，请下载PEM格式证书；  
② 其他方式连接访问MemFire Cloud数据库时，请下载DER格式证书；  
鉴于dbeaver使用jdbc连接数据库，选择”PEM“的证书，点击”下载证书“按钮，将证书下载到本地。  

5. 解压下载证书。   
解压文件中包括：“root.crt”，“memfiredb.crt”和“memfiredb.key”三个文件； 

6. 配置连接数据库。   
在dbeaver中点击“数据库”-“新建连接”，驱动类型选择PostgreSQL。在“常规”下面填写主机、端口、数据库、用户名和密码。如下图：  
<div style="width:600px" >
<img src="../_media/w_dbeaver_ssl_2.png"  alt="图片名称" align=center>
</div>
在“SSL”标签下勾选“使用SSL”，证书一栏中选择之前下载的证书文件，“SSL模式”一栏中选择“verify-ca”，如下图：  
<div style="width:600px" >
<img src="../_media/w_dbeaver_ssl_3.png"  alt="图片名称" align=center>
</div>

7. 点击“完成”创建数据库连接  
最后，数据库连接建立之后，可以继续操作数据库。  

---

###  dbeaver连接MemFireDB
以dbeaver7.1.0为例，介绍如何进行配置SSL连接MemFireDB。  

1. 下载软件。dbeaver7.1.0软件: [下载地址](https://github.com/dbeaver/dbeaver/releases/download/7.1.0/dbeaver-ce-7.1.0-win32.win32.x86_64.zip)      

2. 注册登录MemFire Cloud平台。登录[MemFire Cloud](https://memfiredb.com/)平台完成新用户的注册。  

3. 创建数据库。    
进入数据库管理页面，点击“创建数据库”按钮，在”创建数据库“弹框中填写“数据库名称”，选择数据库账号，勾选“密码认证”，最后点击“确定”完成数据库创建操作。  
<div  style="width:1200px;display:block; background: rgba(22,184,248,.1); font-size:14px" >
<p style="line-height:50px; padding: 0 0 0 15px">
备注说明：   
① 创建数据库名称不能和已有数据库重复；   
② 如果没有数据库账号，则先创建数据库账号;  </p>
</div>     

4. 获取连接信息。      
选中数据库列表中某个数据库所在列，点击“在线连接”按钮，弹出“连接信息”弹框；   
可以查看该数据库的访问IP以及端口信息，如下图：  
![avatar](../_media/w_dbeaver_password_1.png)  

5. 配置连接数据库。   
在dbeaver中点击“数据库”-“新建连接”，驱动类型选择PostgreSQL。在“常规”下面填写主机、端口、数据库、用户名和密码。如下图：  
<div style="width:500px" >
<img src="../_media/w_dbeaver_password_2.png"  alt="图片名称" align=center>
</div>

6. 点击“完成”创建数据库连接。数据库连接建立之后，可以继续操作数据库。

---
## linux 环境
### psql使用ssl连接MemFireDB

1. 注册登录MemFire Cloud平台。登录[MemFire Cloud](https://memfiredb.com/)平台完成新用户的注册;  

2. 创建数据库。   
进入数据库管理页面，点击“创建数据库”按钮，在”创建数据库“弹框中填写“数据库名称”，选择数据库账号，勾选“证书认证”，最后点击“确定”完成数据库创建操作。 
<div  style="width:1200px;display:block; background: rgba(22,184,248,.1); font-size:14px" >
<p style="line-height:50px; padding: 0 0 0 15px">
备注说明：   
① 创建数据库名称不能和已有数据库重复；   
② 如果没有数据库账号，则先创建数据库账号;  </p>
</div>    

3. 解压下载证书。   
下载证书，选中数据库列表中某个数据库所在列，点击“在线连接”按钮，弹出“连接信息”弹框； 当数据库为证书认证方式时，则提供证书下载链接，包括DER格式、PEM格式证书；  
① 采用JAVA使用JDBC连接访问MemFire Cloud数据库时，请下载PEM格式证书；  
② 其他方式连接访问MemFire Cloud数据库时，请下载DER格式证书；    

4. 获取连接信息。   
点击“在线连接”按钮，弹出“连接信息”弹框，可以查看该数据库的访问IP以及端口信息。如下图所示：  
![avatar](../_media/linux_psql_ssl_1.png)   

最后，大家按照代码示例并结合上面获取到的信息编写修改自己的程序即可访MemFireDB。   

```
psql "host=192.168.80.153 port=5433 dbname=db0f2985de09dd435ba7e98ddac663c918certdb user=test sslcert=/root/.postgresql/memfiredb.crt  sslkey=/root/.postgresql/memfiredb.key sslrootcert=/root/.postgresql/root.crt  sslmode=verify-ca"
```

---
### psql连接MemFireDB

1. 注册登录MemFire Cloud平台。登录[MemFire Cloud](https://memfiredb.com/)平台完成新用户的注册;  

2. 创建数据库。   
进入数据库管理页面，点击“创建数据库”按钮，在”创建数据库“弹框中填写“数据库名称”，选择数据库账号，勾选“密码认证”，最后点击“确定”完成数据库创建操作。  
<div  style="width:1200px;display:block; background: rgba(22,184,248,.1); font-size:14px" >
<p style="line-height:50px; padding: 0 0 0 15px">
备注说明：   
① 创建数据库名称不能和已有数据库重复；   
② 如果没有数据库账号，则先创建数据库账号;  </p>
</div>   

3. 获取连接信息。   
点击“在线连接”按钮，弹出“连接信息”弹框，可以查看该数据库的访问IP以及端口信息。如下图所示：  
![avatar](../_media/linux_psql_password_1.png)
最后，大家按照代码示例并结合上面获取到的信息编写修改自己的程序即可访MemFireDB数据库。  
```
psql "host=192.168.80.153 port=5433 dbname=db0f2985de09dd435ba7e98ddac663c918test user=test"
 ``` 

--- 

## mac环境

###  dbeaver使用SSL连接MemFireDB
以dbeaver7.1.0为例，介绍如何进行配置SSL连接MemFireDB。  

1. 下载软件。dbeaver7.1.0软件: [下载地址](https://dbeaver.io/files/7.1.0/dbeaver-ce-7.1.0-macosx.cocoa.x86_64.tar.gz)

2. 注册登录MemFire Cloud平台。登录[MemFire Cloud](https://memfiredb.com/)平台完成新用户的注册。  

3. 创建数据库。   
进入数据库管理页面，点击“创建数据库”按钮，在”创建数据库“弹框中填写“数据库名称”，选择数据库账号，勾选“证书认证”，最后点击“确定”完成数据库创建操作。  
<div  style="width:1200px;display:block; background: rgba(22,184,248,.1); font-size:14px" >
<p style="line-height:50px; padding: 0 0 0 15px">
备注说明：   
① 创建数据库名称不能和已有数据库重复；   
② 如果没有数据库账号，则先创建数据库账号;  </p>
</div>      

4. 获取连接信息。   
选中数据库列表中某个数据库所在列，点击“在线连接”按钮，弹出“连接信息”弹框；   
可以查看该数据库的访问IP以及端口信息，当数据库为证书认证方式时，则提供证书下载链接，包括DER格式、PEM格式证书；  
![avatar](../_media/mac_dbeaver_ssl_1.png)  
① 采用JAVA使用JDBC连接访问MemFire Cloud数据库时，请下载PEM格式证书；  
② 其他方式连接访问MemFire Cloud数据库时，请下载DER格式证书；  
鉴于dbeaver使用jdbc连接数据库，选择”PEM“的证书，点击”下载证书“按钮，将证书下载到本地。  

5. 解压下载证书。   
解压下载证书，解压文件中包括：“root.crt”，“memfiredb.crt”和“memfiredb.key”三个文件；  

6. 配置连接数据库。   
在dbeaver中点击“数据库”-“新建连接”，驱动类型选择PostgreSQL。在“常规”下面填写主机、端口、数据库、用户名和密码。如下图：  
<div style="width:600px" >
<img src="../_media/mac_dbeaver_ssl_2.png"  alt="图片名称" align=center>
</div>
在“SSL”标签下勾选“使用SSL”，证书一栏中选择之前下载的证书文件，“SSL模式”一栏中选择“verify-ca”，如下图：  
<div style="width:600px" >
<img src="../_media/mac_dbeaver_ssl_3.png"  alt="图片名称" align=center>
</div>

7. 点击“完成”创建数据库连接。数据库连接建立之后，可以继续操作数据库。  

---

### dbeaver连接MemFireDB

以dbeaver7.1.0为例，介绍如何进行配置连接MemFireDB。  
  
1. 下载软件。dbeaver7.1.0软件: [下载地址](https://github.com/dbeaver/dbeaver/releases/download/7.1.0/dbeaver-ce-7.1.0-win32.win32.x86_64.zip);  

2. 注册登录MemFire Cloud平台。登录[MemFire Cloud](https://memfiredb.com/)平台完成新用户的注册。  

3. 创建数据库。   
进入数据库管理页面，点击“创建数据库”按钮，在”创建数据库“弹框中填写“数据库名称”，选择数据库账号，勾选“密码认证”，最后点击“确定”完成数据库创建操作。  
<div  style="width:1200px;display:block; background: rgba(22,184,248,.1); font-size:14px" >
<p style="line-height:50px; padding: 0 0 0 15px">
备注说明：   
① 创建数据库名称不能和已有数据库重复；   
② 如果没有数据库账号，则先创建数据库账号;  </p>
</div>     

4. 获取连接信息。   
选中数据库列表中某个数据库所在列，点击“在线连接”按钮，弹出“连接信息”弹框； 可以查看该数据库的访问IP以及端口信息，如下图：  
![avatar](../_media/mac_dbeaver_password_1.png)

5. 配置连接数据库。   
在dbeaver中点击“数据库”-“新建连接”，驱动类型选择PostgreSQL。在“常规”下面填写主机、端口、数据库、用户名和密码。如下图：  
<div style="width:600px" >
<img src="../_media/mac_dbeaver_password_2.png"  alt="图片名称" align=center>
</div>

6. 点击“完成”创建数据库连接。数据库连接建立之后，可以继续操作数据库。  