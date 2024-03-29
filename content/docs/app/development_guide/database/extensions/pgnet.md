---
    weight: 1536
    title: "pg_net: 异步网络"
    description: "pg_net: an async networking extension for PostgreSQL."
    icon: "article"
    draft: false
    toc: true
---

{{% alert context="info" %}}
pg_net的API还处于beta阶段。函数签名可能会改变。
{{% /alert %}}

[pg_net](https://github.com/supabase/pg_net/)是一个PostgreSQL扩展，为异步网络暴露了一个SQL接口，重点是可扩展性和用户体验。

它与`http`扩展的不同之处在于，它默认是异步的。这使得它在阻塞函数（如触发器）中很有用。

## 用法

### 启用扩展功能

{{< tabs tabTotal="2" >}}

{{% tab tabName="Dashboard" %}}



1. 进入仪表板中的**数据库**页面。
2. 点击侧边栏中的*扩展*.
3. 3. 搜索 "pg_net "并启用该扩展。



{{% /tab %}}
{{% tab tabName="SQL" %}}



```sql
-- Example: enable the "pg_net" extension
create schema if not exists net;
create extension pg_net with schema net;

-- Example: disable the "plv8" extension
drop extension if exists pg_net;
drop schema net;
```

尽管SQL代码是`create extension`，但这相当于 "启用扩展"。
要禁用一个扩展，请调用`drop extension`。

程序语言会自动安装在`pg_catalog`中，所以你不需要指定模式。



{{% /tab %}}

{{< /tabs >}}

## `http_get` {#http_get}

创建一个HTTP GET请求，返回该请求的ID。在事务提交之前，HTTP请求不会被启动。

### 签名

{{% alert context="info" %}}
这是一个Postgres安全定义函数。
{{% /alert %}}

```sql
net.http_get(
    -- url for the request
    url text,
    -- key/value pairs to be url encoded and appended to the `url`
    params jsonb default '{}'::jsonb,
    -- key/values to be included in request headers
    headers jsonb default '{}'::jsonb,
    -- WARNING: this is currently ignored, so there is no timeout
    -- the maximum number of milliseconds the request may take before being cancelled
    timeout_milliseconds int default 1000
)
    -- request_id reference
    returns bigint

    strict
    volatile
    parallel safe
    language plpgsql
```

### 使用方法

```sql
select net.http_get('https://news.ycombinator.com') as request_id;
request_id
----------
         1
(1 row)
```

在触发了`http_get`之后，使用[`http_get_result`](#http_get_result)来获取请求的结果。

## `http_post` {#http_post}

创建一个带有JSON主体的HTTP POST请求，返回请求的ID。HTTP请求在事务提交之前不会被启动。

主体的字符集编码与数据库的`server_encoding`设置一致。

### 签名

{{% alert context="info" %}}
这是一个Postgres安全定义函数。
{{% /alert %}}

```sql
net.http_post(
    -- url for the request
    url text,
    -- body of the POST request
    body jsonb default '{}'::jsonb,
    -- key/value pairs to be url encoded and appended to the `url`
    params jsonb default '{}'::jsonb,
    -- key/values to be included in request headers
    headers jsonb default '{"Content-Type": "application/json"}'::jsonb,
    -- WARNING: this is currently ignored, so there is no timeout
    -- the maximum number of milliseconds the request may take before being cancelled
    timeout_milliseconds int default 1000
)
    -- request_id reference
    returns bigint

    volatile
    parallel safe
    language plpgsql
```

### 使用方法

```sql
select
    net.http_post(
        url:='https://httpbin.org/post',
        body:='{"hello": "world"}'::jsonb
    ) as request_id;
request_id
----------
         1
(1 row)
```

在触发了`http_post`之后，使用[`http_get_result`](#http_get_result)来获得请求的结果。

## `http_collect_response` {#http_collect_response}

给出一个`request_id`参考，检索响应。

当`async:=false`被设置时，建议将[statement_timeout](https://www.postgresql.org/docs/13/runtime-config-client.html)设置为调用者愿意等待的最大时间，以防止响应填充缓慢。

### 签名

{{% alert context="info" %}}
这是一个Postgres安全定义函数。
{{% /alert %}}

```sql
net.http_collect_response(
    -- request_id reference
    request_id bigint,
    -- when `true`, return immediately. when `false` wait for the request to complete before returning
    async bool default true
)
    -- http response composite wrapped in a result type
    returns net.http_response_result

    strict
    volatile
    parallel safe
```

### 使用方法

{{% alert context="info" %}}
`net.http_collect_response`必须在与调用`net.http_<method>`不同的事务中。
{{% /alert %}}

```sql
select
    net.http_post(
        url:='https://httpbin.org/post',
        body:='{"hello": "world"}'::jsonb
    ) as request_id;
request_id
----------
         1
(1 row)

select * from net.http_collect_response(1, async:=false);
status  | message | response
--------+---------+----------
SUCCESS        ok   (
                      status_code := 200,
                      headers     := '{"date": ...}',
                      body        := '{"args": ...}'
                    )::net.http_response_result


select
    (response).body::json
from
    net.http_collect_response(request_id:=1);
                               body
-------------------------------------------------------------------
 {
   "args": {},
   "data": "{\"hello\": \"world\"}",
   "files": {},
   "form": {},
   "headers": {
     "Accept": "*/*",
     "Content-Length": "18",
     "Content-Type": "application/json",
     "Host": "httpbin.org",
     "User-Agent": "pg_net/0.2",
     "X-Amzn-Trace-Id": "Root=1-61031a5c-7e1afeae69bffa8614d8e48e"
   },
   "json": {
     "hello": "world"
   },
   "origin": "135.63.38.488",
   "url": "https://httpbin.org/post"
 }
(1 row)
```

其中，`response`是一个组合：

```sql
status_code integer
headers jsonb
body text
```

`net.http_response_result.status的可能值是`('PENDING', 'SUCCESS', 'ERROR')'。

## 资源

- 源代码: [github.com/supabase/pg_net](https://github.com/supabase/pg_net/)
- 官方文档: [supabase.github.io/pg_net](https://supabase.github.io/pg_net/)


