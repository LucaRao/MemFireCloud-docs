---
    weight: 1539
    title: "uuid-ossp: 唯一标识符"
    description: "A UUID generator for PostgreSQL."
    icon: "article"
    draft: false
    toc: true
---

`uuid-ossp`扩展可用于生成`UID`。

## 概述

`UUID`是一个 `通用唯一标识符`，在实际应用中，它是唯一的。
这使得它们特别适合作为主键。它有时也被称为 `GUID`，代表 `全球唯一标识符`。

## 使用方法

### 启用扩展名

{{< tabs tabTotal="2" >}}


{{% tab tabName="Dashboard" %}}



1. 进入仪表板中的**数据库**页面。
2. 点击侧边栏中的*扩展*。
3. 搜索 `uuid-ossp`并启用该扩展。

**Note**:
目前 `uuid-ossp`扩展被默认启用，不能被禁用。



{{% /tab %}}
{{% tab tabName="SQL" %}}



```sql
-- Example: enable the "uuid-ossp" extension
create extension "uuid-ossp" with schema extensions;

-- Example: disable the "uuid-ossp" extension
drop extension if exists "uuid-ossp";
```

尽管SQL代码是`create extension`，但这相当于 "启用扩展"。
要禁用一个扩展，请调用`drop extension`。

程序语言会自动安装在`pg_catalog`中，所以你不需要指定模式。

**Note**:
目前 `uuid-ossp`扩展被默认启用，不能被禁用。



{{% /tab %}}

{{< /tabs >}}

### `Uuid`类型

一旦扩展被启用，你现在可以访问一个`uuid`类型。

### `uuid_generate_v1()`

根据计算机的MAC地址、当前时间戳和一个随机值的组合创建一个UUID值。

{{% alert context="info" %}}
UUIDv1泄露了可识别的细节，这可能使它不适合于某些安全敏感的应用.
{{% /alert %}}

### `uuid_generate_v4()`

创建完全基于随机数的UUID值。你也可以使用Postgres内置的[`gen_random_uuid()`](https://www.postgresql.org/docs/current/functions-uuid.html)函数来生成一个UUIDv4。

## 示例

### 在一个查询中

```sql
select uuid_generate_v4();
```

### 作为主键

在表中自动创建唯一的随机ID：

```sql
create table contacts (
  id uuid default uuid_generate_v4(),
  first_name text,
  last_name text,

  primary key (id)
);
```

## 资源

- [选择一个Postgres主键](https://supabase.com/blog/choosing-a-postgres-primary-key)
- [PostgreSQL `UID`数据类型的基础知识](https://www.postgresqltutorial.com/postgresql-uuid/)


