---
    weight: 1537
    title: "pgTAP:单元测试"
    description: "Unit testing in PostgreSQL."
    icon: "article"
    draft: false
    toc: true
---


`pgTAP`是PostgreSQL的一个单元测试扩展。

## 概述

让我们来介绍一些基本的概念：

- 单元测试：允许你测试一个系统的小部分（如数据库表！）。
- TAP：代表[Test Anything Protocol](http://testanything.org/)。它是一个框架，旨在简化测试期间的错误报告。

## 使用方法

### 启用扩展

{{< tabs tabTotal="2" >}}

{{% tab tabName="Dashboard" %}}



1. 进入仪表板中的**数据库**页面。
2. 点击侧边栏中的*扩展*。
3. 搜索 "pgtap "并启用该扩展。



{{% /tab %}}
{{% tab tabName="SQL" %}}



```sql
-- Enable the "pgtap" extension
create extension pgtap with schema extensions;

-- Disable the "pgtap" extension
drop extension if exists pgtap;
```

尽管SQL代码是`create extension`，但这相当于 "启用扩展"。
要禁用一个扩展，请调用`drop extension`。

程序语言会自动安装在`pg_catalog`中，所以你不需要指定模式。



{{% /tab %}}

{{< /tabs >}}

### 测试表

```sql
begin;
select plan( 1 );

select has_table( 'profiles' );

select * from finish();
rollback;
```

API：

- [`has_table()`](https://pgtap.org/documentation.html#has_table)：测试数据库中是否存在一个表。
- [`has_index()`](https://pgtap.org/documentation.html#has_index): 检查是否存在与命名表相关的命名索引。
- [`has_relation()`](https://pgtap.org/documentation.html#has_relation): 测试数据库中是否存在一个关系。

### 测试列

```sql
begin;
select plan( 2 );

select has_column( 'profiles', 'id' );  # test that the "id" column exists in the "profiles" table
select col_is_pk( 'profiles', 'id' );   # test that the "id" column is a primary key

select * from finish();
rollback;
```

API：

- [`has_column()`](https://pgtap.org/documentation.html#has_column): 测试一个列是否存在于给定的表、视图、物化视图或复合类型中。
- [`col_is_pk()`](https://pgtap.org/documentation.html#col_is_pk): 测试表中指定的列是否是该表的主键。

### 测试RLS策略

```sql
begin;
select plan( 1 );

select policies_are(
  'public',
  'profiles',
  ARRAY [
    'Profiles are public', # Test that there is a policy called  "Profiles are public" on the "profiles" table.
    'Profiles can only be updated by the owner'  # Test that there is a policy called  "Profiles can only be updated by the owner" on the "profiles" table.
  ]
);

select * from finish();
rollback;
```

API：

- [`policies_are()`](https://pgtap.org/documentation.html#policies_are)：测试指定表上的所有策略是否只是该表应该有的策略。
- [`policy_roles_are()`](https://pgtap.org/documentation.html#policy_roles_are): 测试策略适用的角色是否只是该策略上应该有的角色。
- [`policy_cmd_is()`](https://pgtap.org/documentation.html#policy_cmd_is): 测试策略适用的命令是否与函数参数中给出的命令相同。

你也可以使用`results_eq()`方法来测试策略是否返回正确的数据。

```sql
begin;
select plan( 1 );

select results_eq(
    'select * from profiles()',
    $$VALUES ( 1, 'Anna'), (2, 'Bruce'), (3, 'Caryn')$$
    'profiles() should return all users'
);


select * from finish();
rollback;
```

API:

- [`results_eq()`](https://pgtap.org/documentation.html#results_eq)
- [`results_ne()`](https://pgtap.org/documentation.html#results_ne)


### 测试函数

```sql
begin;
select plan( 1 );

select function_returns( 'hello_world', 'text' );                   # test if the function "hello_world" returns text
select function_returns( 'is_even', ARRAY['integer'], 'boolean' );  # test if the function "is_even" returns a boolean
select results_eq('select * from hello_world()', 'hello');          # test if the function "hello_world" returns "hello"

select * from finish();
rollback;
```

API:

- [`function_returns()`](https://pgtap.org/documentation.html#function_returns)：测试一个特定的函数是否返回一个特定的数据类型。
- [`is_definer()`](https://pgtap.org/documentation.html#is_definer): 测试一个函数是否是安全定义器(即一个 "setuid "函数)。

## 资源

- 官方 [`pgTAP` 文档](https://pgtap.org/)


