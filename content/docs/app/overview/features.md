---
    weight: 102
    title: "功能"
    description: "MemFire Cloud features"
    icon: "article"
    draft: false
    toc: true
---

这是MemFire Cloud为每个项目提供的功能的非详尽列表。

## 数据库

### Postgres数据库

每个项目都是一个完整的Postgres数据库. [文档](/docs/app/database/database).

### 数据库扩展

每个数据库都有一整套Postgres扩展. [文档](/docs/app/database/extensions/extensions).

### 数据库函数

创建可以从浏览器调用的自定义数据库函数. [文档](/docs/app/database/functions).

### 数据库触发器

将触发器附加到表以处理数据库更改. [文档](/docs/app/auth/mandates/managing-user-data#using-triggers).


### 数据库备份

每天备份项目，可选择升级到时间点恢复.

### 搜索

使用Postgres全文搜索构建搜索功能. [文档](/docs/app/database/full-text-search).

### 密钥和加密

使用我们的Postgres扩展MemFire Cloud Vault加密敏感数据并存储机密.[链接](https://supabase.com/blog/supabase-vault)



<br />

## 身份验证

### 电子邮件和密码登录

为您的应用程序或网站建立电子邮件登录. [文档](/docs/app/auth/authentication/auth-email).

### 魔法链接

为应用程序或网站建立无密码登录.[文档](/docs/app/auth/authentication/auth-magic-link).

### 社交登录

提供社交登录-从苹果到GitHub，再到Slack. [文档](/docs/app/auth/authentication/auth-apple).


### 行级别安全性

使用Postgres策略控制每个用户可以访问的数据. [文档](/docs/app/auth/mandates/row-level-security).



<br />

## API和客户端库

### 自动生成的REST API

RESTful API是从数据库自动生成的，无需一行代码. [文档](/docs/app/api/api#rest-api-overview).

### 自动生成的GraphQL API

使用我们的自定义PostgresGraphQL扩展的快速GraphQL API. [文档](/docs/app/api/api#graphql-api-overview).

### 实时数据库变更

通过websockets接收数据库更改. [文档](/docs/app/realtime/postgres-cdc).

### 用户广播

通过websocket在连接的用户之间发送消息. [文档](/docs/app/realtime/realtime#broadcast).

### 用户状态

跨用户同步共享状态，包括联机状态和键入指示符. [文档](/docs/app/realtime/realtime#presence).

### 客户端库

官方客户端库[JavaScript](/docs/app/SDKdocs/JavaScript/start/installing)和非官方客户端库[Dart](/docs/reference/dart)，[由社区支持](https://github.com/supabase-community#client-libraries)。 

<br />

## 文件存储

### 大文件存储

MemFire Cloud 存储使存储和服务大文件变得简单. [文档](/docs/app/storage/storage).

### 存储CDN

使用MemFire Cloud CDN缓存大文件. [文档](/docs/app/storage/storage-cdn).

<br />



## 功能状态

Postgres和MemFire Cloud平台都已做好生产准备。我们在Postgres之上提供的一些工具仍在开发中。
{{< table "table-striped-columns" >}}
| 产品                   | 功能                | 阶段   |
| -------------------------- | ---------------------- | ------- |
| Database                   | Postgres               | `GA`    |
| Database                   | Triggers               | `GA`    |
| Database                   | Functions              | `GA`    |
| Database                   | Extensions             | `GA`    |
| Database                   | Full Text Search       | `GA`    |
| Database                   | Webhooks               | `alpha` |
| Database                   | Poipnt-in-Time Recovery | `alpha` |
| Database                   | Vault                  | `alpha` |
| Studio                     |                        | `GA`    |
| Realtime                   | Postgres CDC           | `GA`    |
| Realtime                   | Broadcast              | `beta`  |
| Realtime                   | Presence               | `beta`  |
| Storage                    | Backend (S3)           | `GA`    |
| Storage                    | API                    | `beta`  |
| Storage                    | CDN                    | `beta`  |
| Edge Functions             |                        | `beta`  |
| Auth                       | OAuth Providers        | `beta`  |
| Auth                       | Passwordless           | `beta`  |
| Auth                       | Next.js Auth Helpers   | `alpha` |
| Auth                       | SvelteKit Auth Helpers | `alpha` |
| Auth                       | Remix Auth Helpers     | `alpha` |
| Management API             |                        | `beta`  |
| CLI                        |                        | `beta`  |
| Client Library: JavaScript |                        | `GA`    |
| Client Library: Dart       |                        | `beta`  |

 {{< /table >}}
