---
    weight: 1505
    title: "数据库函数"
    description: "Creating and using Postgres functions."
    icon: "article"
    draft: false
    toc: true
---

Postgres内置了对[SQL函数](https://www.postgresql.org/docs/current/sql-createfunction.html)的支持。
这些函数存在于你的数据库中，它们可以[与API一起使用](/docs/app/sdkdocs/javascript/database/rpc/)。

## 快速演示
<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/MJZCCpCYEqk"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>

## 开始使用

Supabase 为创建数据库函数提供了几个选项。你可以使用仪表板或直接使用 SQL 创建它们。
我们在 Dashboard 中提供了一个 SQL 编辑器，或者你可以 [连接](/docs/app/development_guide/database/connecting-to-postgres/) 到数据库并自己运行SQL查询。

1. 进入 `SQL编辑器`栏。
2. 点击 `新查询`。
3. 输入创建或替换数据库功函数的SQL。
4. 点击 `运行`或cmd+enter (ctrl+enter)。

## 简单的函数

让我们创建一个基本的数据库函数，返回一个字符串 `hello world`.

```sql
create or replace function hello_world() -- 1
returns text -- 2
language sql -- 3
as $$  -- 4
  select 'hello world';  -- 5
$$; --6

```

<details>
<summary>显示/隐藏细节</summary>

最基本的是，一个函数有以下部分：

1. `create or replace function hello_world()`。函数声明，其中`hello_world`是函数的名称。你可以在创建一个新的函数时使用`create`，或者在替换一个现有函数时使用`replace`。或者你可以同时使用`create或replace`来处理这两种情况。
2. `returns text`: 函数返回的数据类型。如果它什么都不返回，你可以`returns void`。
3. `language sql': 在函数主体中使用的语言。这也可以是一种程序性语言：`plpgsql`, `plv8`, `plpython`等。
4. `as $$`: 函数包装器。任何包含在`$$`符号中的东西都将是函数主体的一部分。
5. `select 'hello world';`: 一个简单的函数体。如果函数体中的最后一条`select`语句后面没有语句，将被返回。
6. `$$;`: 函数封装器的结束符号。

</details>

<br />

函数创建后，我们有几种 "执行 "函数的方法--可以直接在数据库中使用SQL，也可以使用其中一个客户端库。

{{< tabs tabTotal="11" >}}

  
  
  
  
{{% tab tabName="SQL" %}}



```sql
select hello_world();
```



{{% /tab %}}
{{% tab tabName="JavaScript" %}}



```js
const { data, error } = await supabase.rpc('hello_world')
```

参考资料: [rpc()](/docs/app/sdkdocs/javascript/database/rpc/)



{{% /tab %}}
{{% tab tabName="Dart" %}}



```dart
final res = await supabase
  .rpc('hello_world')
  .execute();
```

Reference: [rpc()](/docs/app/sdkdocs/Dart/database/rpc/)



{{% /tab %}}

{{< /tabs >}}

## 返回数据集


数据库函数也可以从[数据表](/docs/app/development_guide/database/tables/)或视图中返回数据集。

例如，如果我们有一个数据库，里面有一些星球大战的数据。


{{< tabs tabTotal="11" >}}

  
  
  
{{% tab tabName="Data" %}}



<h4>Planets</h4>
{{< table "table-striped-columns" >}}
| id  | name     |
| --- | -------- |
| 1   | Tattoine |
| 2   | Alderaan |
| 3   | Kashyyyk |
 {{< /table >}}

<h4>People</h4>
{{< table "table-striped-columns" >}}
| id  | name             | planet_id |
| --- | ---------------- | --------- |
| 1   | Anakin Skywalker | 1         |
| 2   | Luke Skywalker   | 1         |
| 3   | Princess Leia    | 2         |
| 4   | Chewbacca        | 3         |
 {{< /table >}}


{{% /tab %}}
{{% tab tabName="SQL" %}}



```sql
create table planets (
  id serial primary key,
  name text
);

insert into planets (id, name)
values
  (1, 'Tattoine'),
  (2, 'Alderaan'),
  (3, 'Kashyyyk');

create table people (
  id serial primary key,
  name text,
  planet_id bigint references planets
);

insert into people (id, name, planet_id)
values
  (1, 'Anakin Skywalker', 1),
  (2, 'Luke Skywalker', 1),
  (3, 'Princess Leia', 2),
  (4, 'Chewbacca', 3);
```



{{% /tab %}}

{{< /tabs >}}

我们可以创建一个函数，返回所有的星球：

```sql
create or replace function get_planets()
returns setof planets
language sql
as $$
  select * from planets;
$$;
```

因为这个函数返回一个表集，我们也可以应用过滤器和选择器。例如，如果我们只想要第一个星球：

{{< tabs tabTotal="11" >}}

  
  
  
  
{{% tab tabName="SQL" %}}



```sql
select *
from get_planets()
where id = 1;
```



{{% /tab %}}
{{% tab tabName="JavaScript" %}}



```js
const { data, error } = supabase.rpc('get_planets').eq('id', 1)
```



{{% /tab %}}
{{% tab tabName="Dart" %}}



```dart
final res = await supabase
  .rpc('get_planets')
  .eq('id', 1)
  .execute();
```



{{% /tab %}}

{{< /tabs >}}

## 传递参数

让我们创建一个函数，在`planets`表中插入一个新的行星并返回新的ID。注意，这次我们使用的是`plpgsql`语言。

```sql
create or replace function add_planet(name text)
returns bigint
language plpgsql
as $$
declare
  new_row bigint;
begin
  insert into planets(name)
  values (add_planet.name)
  returning id into new_row;

  return new_row;
end;
$$;
```

再一次，你可以在数据库中使用`select`查询来执行这个函数，或者使用客户端库。

{{< tabs tabTotal="11" >}}

  
  
  
  
{{% tab tabName="SQL" %}}



```sql
select * from add_planet('Jakku');
```



{{% /tab %}}
{{% tab tabName="JavaScript" %}}



```js
const { data, error } = await supabase.rpc('add_planet', { name: 'Jakku' })
```



{{% /tab %}}
{{% tab tabName="Dart" %}}



```dart
final res = await supabase
  .rpc('add_planet', params: { 'name': 'Jakku' })
  .execute();
```



{{% /tab %}}

{{< /tabs >}}

## 建议

### 数据库函数vs边缘函数

对于数据密集型的操作，使用数据库函数，它在你的数据库中执行并可以使用REST和GraphQL API远程调用。

对于需要低延迟的使用情况，使用[边缘函数](/docs/app/development_guide/functions/overview/)，它是全局分布的，可以用Typescript编写。


### 安全性 `definer`vs `invoker`

Postgres允许你指定是作为调用函数的用户（`invoker`），还是作为函数的创建者（`definer`）来执行函数。比如说:

```sql
create function hello_world()
returns text
language plpgsql
security definer set search_path = public
as $$
begin
  select 'hello world';
end;
$$;
```

最好的做法是使用`security invoker`（这也是默认的）。如果你使用`security definer`，你必须设置`search_path`。
这限制了潜在的损害，如果你允许访问执行该函数的用户不应该有的模式。

### 函数的权限

默认情况下，数据库函数可以由任何角色执行。你可以通过改变默认权限来限制这一点，然后选择哪些角色可以执行功能。

```sql
ALTER DEFAULT PRIVILEGES REVOKE EXECUTE ON FUNCTIONS FROM PUBLIC;

-- Choose which roles can execute functions
GRANT EXECUTE ON FUNCTION hello_world TO authenticated;
GRANT EXECUTE ON FUNCTION hello_world TO service_role;
```

## 资源

- 官方客户端库: [JavaScript](/docs/app/sdkdocs/javascript/database/rpc/) and [Flutter](../../reference/dart/rpc)
- 社区客户端库: [github.com/supabase-community](https://github.com/supabase-community)
- PostgreSQL官方文档: [第9章 函数和运算符](https://www.postgresql.org/docs/current/functions.html)
- PostgreSQL参考资料: [创建函数](https://www.postgresql.org/docs/9.1/sql-createfunction.html)

## 深度挖掘

### 创建数据库函数

<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/MJZCCpCYEqk"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>

### 使用JavaScript调用数据库函数

<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/I6nnp9AINJk"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>

### 使用数据库函数来调用外部API

<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/rARgrELRCwY"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>


