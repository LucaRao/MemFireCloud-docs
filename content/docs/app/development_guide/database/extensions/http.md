---
    weight: 1532
    title: "http: RESTful客户端"
    description: "An HTTP Client for PostgreSQL Functions."
    icon: "article"
    draft: false
    toc: true
---

`http`扩展允许你在Postgres中调用RESTful端点。

## 快速演示

<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/rARgrELRCwY"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>

## 概述

让我们来介绍一些基本概念：

- REST：是REpresentational State Transfer的缩写。它是一种从外部服务请求数据的简单方法。
- RESTful APIs是接受HTTP "调用"的服务器。这些调用通常是：
  - `GET` - 只读访问一个资源。
  - `POST` - 创建一个新的资源。
  - `DELETE` - 移除一个资源。
  - `PUT` - 更新一个现有的资源或创建一个新的资源。

你可以使用`http`扩展来从Postgres进行这些网络请求。

## 用法

### 启用扩展功能

{{< tabs tabTotal="2" >}}


{{% tab tabName="Dashboard" %}}



1. 进入仪表板中的**数据库**页面。
2. 点击侧边栏中的*扩展*。
3. 搜索 "http "并启用该扩展。



{{% /tab %}}
{{% tab tabName="SQL" %}}



```sql
-- Example: enable the "http" extension
create extension http with schema extensions;

-- Example: disable the "http" extension
drop extension if exists http;
```
尽管SQL代码是`create extension`，但这相当于 "启用扩展"。
要禁用一个扩展，请调用`drop extension`。

好的做法是在一个单独的模式（如 `extensions`）中创建扩展，以保持你的数据库干净。



{{% /tab %}}

{{< /tabs >}}

### 可用的函数

虽然主要用法是简单的`http('http_request')`，但有5个封装函数用于特定功能：

- `http_get()`
- `http_post()`
- `http_put()`
- `http_delete()`
- `http_head()`

### 返回值

从`http`扩展中成功调用一个网络URL，会返回一个包含以下字段的记录:

- `status`: integer
- `content_type`: character varying
- `headers`: http_header[]
- `content`: character varying. 通常情况下，你希望使用`content::jsonb`的格式将其转换为`jsonb`。



## 示例

### 简单的“GET”示例

```sql
select
  "status", "content"::jsonb
from
  http_get('https://jsonplaceholder.typicode.com/todos/1');
```

### 简单的“POST”示例

```sql
select
  "status", "content"::jsonb
from
  http_post(
    'https://jsonplaceholder.typicode.com/posts',
    '{ "title": "foo", "body": "bar", "userId": 1 }',
    'application/json'
  );
```

## 资源

- 官方 [`http` GitHub库](https://github.com/pramsey/pgsql-http)


