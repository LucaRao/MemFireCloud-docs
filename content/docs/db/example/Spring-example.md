---
weight: 4104
title: "Spring示例"
description: ""
icon: "article"
date: "2023-12-13T17:39:49+08:00"
lastmod: "2023-12-13T17:39:49+08:00"
draft: false
toc: true
---




MemFire Cloud 提供Python、Java、spring、golang、nodejs、小程序开发示例，讲述如何编译执行程序，帮助用户如何采用多种语言来使用连接MemFire Cloud的云数据库。   

 **示例下载地址**    
Spring示例下载地址：https://gitee.com/memfiredb/mefiredb-example-spring  

1、下载证书【可选-如果创建数据库时未勾选证书认证则不需要】  
2、登录cloud.memfiredb.com 创建数据库并下载证书，证书类型选择“jdbc” 将下载的证书保存到合适的路径，本示例例中保存到/home/.memfiredb/中 查看数据库信息，包括服务器地址、数据库名、用户名、密码

3、修改配置文件中datasource【如果创建数据库时未勾选证书认证则url中不需要配置ssl相关内容】  
src/main/resources/application.properties  
```
# Data-source config.
# 请修改服务器地址、数据库名、用户名、密码
spring.datasource.platform=postgres
spring.datasource.url=jdbc:postgresql://192.168.80.5:5433/d0000005e2e1ead563d7e1b07a9a444cspring?ssl=true&sslmode=verify-ca&sslcert=/home/.memfiredb/memfiredb.crt&sslkey=/home/.memfiredb/memfiredb.key&sslrootcert=/home/.memfiredb/root.crt
spring.datasource.username=spring
spring.datasource.password=spring_123
```
4、编译  
```
$ mvn -DskipTests package
```  
5、运行
```  
$ mvn spring-boot:run  
```

