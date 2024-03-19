---
    weight: 1606
    title: "使用cURL工具访问数据"
    description: "使用cURL工具访问数据."
    icon: "article"
    draft: false
    toc: true
---


除了API方式外，MemFire Cloud在线文档提供cURL命令来访问云数据库中的数据~

备注说明：cURL是一个命令行工具（客户端（Client）URL工具），通过指定的URL来上传或下载数据，并将数据展示出来。cURL功能非常强大，熟练的话可以取代Postman 这一类的图形界面工具。

## 前置条件

① 注册MemFire Cloud账号；

② 存在已创建好的应用；

③ 完成建表操作，且有写入数据；

## 操作步骤

### 1.使用API文档

在**我的应用**管理页面，点击具体应用，进入应用详情页面，点击左侧菜单栏“API文档”。选中所有数据表中的“employees”，右侧点击"Bash"栏，应用API key选择“公开(anno)”, 则会出现上图所示的该数据表的专属文档。

<img src="../../img/curl-1.png">

### 2.访问数据

打开Bash编译器，复制上图中的读取所有行数据命令，粘贴到编译器里，回车即可查询employees数据表的所有数据。

<img src="../../img/curl-2.png">


