---
weight: 3
title: "数据库管理"
description: ""
icon: "article"
date: "2023-12-13T17:39:49+08:00"
lastmod: "2023-12-13T17:39:49+08:00"
draft: false
toc: true
---



新用户完成注册登录后，可以自助式的获得数据库资源。MemFire Cloud用户可以根据需要自助的进行数据库管理，包括创建数据库、删除、连接、备份等功能。
## 创建数据库
### 前置条件
创建数据库前置条件：  
*   用户名下需要有数据库账号，才可以创建数据库;  
*   用户有足够的资源额度；

### 操作步骤
1. 用户登录MemFire Cloud后，进入数据库管理页面；  
2. 单击“创建数据库”按钮，弹出“创建数据库”弹框；  
<!-- ![alt text](../_media/createdb.png)   -->
<div style="width:70%" >
<img src="../_media/createdb.png"  alt="图片名称" align=center>
</div>  

3. 输入要创建数据库名称，选择数据库账号，其中：      
    - 数据库名称由小写字母、大写字母、数字、下划线组成，以字母开头，字母或数字结尾，最多28个字符；   
    - 认证方式包括证书认证、密码认证两种。
        * 前者需要下载证书并进行配置，采用加密的方式连接数据库；  
        * 后者采用账号+密码的方式连接数据库；   
    - 创建数据库名称不可重复;  
5. 选择数据库认证方式；  
6. 单击“创建”按钮，则可以完成数据库创建操作，并在列表中查看新建的数据库信息。
<!-- <div style="width:80%" >
<img src="../_media/dblist.png"  alt="图片名称" align=center>
</div>       -->
 ![alt text](../_media/dblist.png)    

## 删除数据库
### 前置条件
删除数据库前提条件：  
*   该数据库没有关联的备份计划；  
*   该数据库处于“运行中”、“资源耗尽”、“异常”三种状态中任意一种；   

### 操作步骤
1. 选中要数据库列表中要删除的数据库所在行，单击数据库名称，进入数据库详情页面；   
2. 点击“释放按钮”，弹出“释放数据库”弹框；    
3. 单击“获取验证码”按钮，请在5min中内输入注册手机收到6位短信验证码； 
<div style="width:90%" >
<img src="../_media/deletedb2.png"  alt="图片名称" align=center>
</div>  
<!-- ![alt text](../_media/deletedb2.png)  -->

4. 单击“确定”按钮，完成数据库的删除操作，该数据库、数据库中所有数据表以及数据都将会被删除。  


## 查看连接信息
1. 用户登录MemFire Cloud后，进入数据库管理页面；  
2. 选中数据库列表中某个数据库所在列，单击“在线连接”按钮，弹出“连接信息”弹框；  
3. 若数据库为密码认证，则其连接信息如下所示：   
<div style="width:70%" >
<img src="../_media/connect1.png"  alt="图片名称" align=center>
</div>   
<!-- ![alt text](../_media/connect1.png)    -->

4. 当数据库为证书认证方式时，则提供证书下载链接，包括DER格式、PEM格式证书，默认勾选DER格式；        
① 采用JAVA使用JDBC连接访问MemFire Cloud数据库时，请下载PEM格式证书；    
② 其他方式连接访问MemFire Cloud数据库时，请下载DER格式证书；       
<!-- ![alt text](../_media/connect2.png)    -->
<div style="width:70%" >
<img src="../_media/connect2.png"  alt="图片名称" align=center>
</div> 

## 查看数据库详情
1. 用户登录MemFire Cloud后，进入数据库管理页面；  
2. 单击数据库列表中某个数据库名称，进入数据库详情页面。   
<div style="width:90%" >
<img src="../_media/dbdetail.png"  alt="图片名称" align=center>
</div> 
<!-- ![alt text](../_media/dbdetail.png)    -->
   
   - 数据库性能指标，最近1小时、最近6小时、最近12小时、最近1天、最近1周的读写OPS，平均延时；  
   - 数据库已用容量，数据表数目，备份计划数、 备份数量；                  |

---
