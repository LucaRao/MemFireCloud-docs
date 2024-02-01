---
    weight: 10
    title: "行级别安全性"
    description: "使用Postgres行级安全保护您的数据。"
    icon: "article"
    draft: false
    toc: true
---

当您需要更精细的授权规则时，PostgreSQL的[行级安全（RLS）](https://www.postgresql.org/docs/current/ddl-rowsecurity.html)是很好的选择。


[策略](https://www.postgresql.org/docs/current/sql-createpolicy.html)是PostgreSQL的规则引擎。它们非常强大和灵活，允许你编写复杂的SQL规则，以满足你独特的业务需求。

<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/Ow_Uzedfohk"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>

## 策略

每个数据库表可以添加一个或多个策略（policy），每当表被访问（增，删，改、查等操作）时，都会执行（校验）设置的策略，就好比为每个sql语句添加了一个where的查询条件。以下是一个策略的例子：

```sql
create policy "Individuals can view their own todos."
    on todos for select
    using ( auth.uid() = user_id );
```

上面的策略在执行sql查询时被转化成以下语句(每当用户查询todos这张表时，只会查询包含自己ID的记录)：

```sql
select *
from todos
where auth.uid() = todos.user_id; -- Policy is implicitly added.
```

## 辅助函数

Supabase为您提供了一些简单的功能，您可以在策略中使用这些功能。

### `auth.uid()`

返回当前前端请求的用户id值。

### `auth.jwt()`

返回当前前端请求的用户jwt值。

## 示例

下面是一些示例，向您展示PostgreSQL的RLS的强大功能。

### 允许读取访问

```sql
-- 1. Create table
create table profiles (
  id uuid references auth.users,
  avatar_url text
);

-- 2. Enable RLS
alter table profiles
  enable row level security;

-- 3. Create Policy
create policy "Public profiles are viewable by everyone."
  on profiles for select using (
    true
  );
```

1. 在pulic模式（Postgre默认模式）中创建名为`profiles`的表。
2. 为该表开启行级别安全。
3. 创建一个策略，允许所有用户查询(select)该表的数据。

### 限制更新

```sql
-- 1. Create table
create table profiles (
  id uuid references auth.users,
  avatar_url text
);

-- 2. Enable RLS
alter table profiles
  enable row level security;

-- 3. Create Policy
create policy "Users can update their own profiles."
  on profiles for update using (
    auth.uid() = id
  );
```

1. 在pulic模式（Postgre默认模式）中创建名为`profiles`的表。
2. 为该表开启行级别安全。
3. 创建一个策略，仅允许当前登录用户更新(update)自己的数据。

### 仅限匿名或已认证的有权访问

您可以添加Postgres角色

```sql
create policy "Public profiles are viewable by everyone."
on profiles for select
to authenticated, anon
using (
  true
);
```

### 连接相关的策略

策略甚至可以包括表联接。此示例显示了如何查询`外部`表以构建更高级的规则。

```sql
create table teams (
  id serial primary key,
  name text
);

-- 2. Create many to many join
create table members (
  team_id bigint references teams,
  user_id uuid references auth.users
);

-- 3. Enable RLS
alter table teams
  enable row level security;

-- 4. Create Policy
create policy "Team members can update team details if they belong to the team."
  on teams
  for update using (
    auth.uid() in (
      select user_id from members
      where team_id = id
    )
  );
```
**注意：**如果还为_members_启用了RLS，则用户还必须具有对_members_1的读取（_select_）权限。否则，连接的查询将不会产生任何结果。

### 具有安全定义功能的策略

策略还可以使用`安全定义功能`。这在您希望限制对链接表的访问的多对多关系中非常有用。在上面的 `teams`和`members`示例之后，本示例显示了如何将安全定义功能与策略结合使用，以控制对 `member`表的访问。

```sql
-- 1. Follow example for 'Policies with joins' above

-- 2.  Enable RLS
alter table members
  enable row level security

-- 3.  Create security definer function
create or replace function get_teams_for_authenticated_user()
returns setof bigint
language sql
security definer
set search_path = public
stable
as $$
    select team_id
    from members
    where user_id = auth.uid()
$$;

-- 4. Create Policy
create policy "Team members can update team members if they belong to the team."
  on members
  for all using (
    team_id in (
      select get_teams_for_authenticated_user()
    )
  );

```

### 验证电子邮件域

Postgres有一个函数`right（string，n）`，它返回字符串中最右边的n个字符。 您可以使用它来匹配员工的电子邮件域。

```sql
-- 1. Create table
create table leaderboard (
  id uuid references auth.users,
  high_score bigint
);

-- 2. Enable RLS
alter table leaderboard
  enable row level security;

-- 3. Create Policy
create policy "Only Blizzard staff can update leaderboard"
  on leaderboard
  for update using (
    right(auth.jwt() ->> 'email', 13) = '@blizzard.com'
  );
```

### 记录的有效期

策略也可以用于实现TTL或实时功能，您可以在Instagram故事或Snapchat中看到。
在下面的示例中，只有在过去24小时内创建了`stories`表的行时，这些行才可用。

策略可以用于实现资源有效期限制TTL(time to live)功能，在微信中也有类似的功能，例如朋友圈仅三天可见

以下实例实现了此功能，表`stories`中只可以查询到过去24小时内的记录，超过24小时的记录将查看不到。

```sql
-- 1. Create table
create table if not exists stories (
  id uuid not null primary key DEFAULT uuid_generate_v4(),
  created_at timestamp with time zone default timezone('utc' :: text, now()) not null,
  content text not null
);

-- 2. Enable RLS
alter table stories
  enable row level security;

-- 3. Create Policy
create policy "Stories are live for a day"
  on stories
  for select using (
    created_at > (current_timestamp - interval '1 day')
  );
```

### 高级策略

Supabase 的高级策略功能允许您使用 SQL 的全部功能来创建复杂的规则。
在上述例子中，我们使用了两个表，即帖子表和评论表（`posts`和`comments`）。并且我们创建了一个评论策略，这个策略依赖于帖子策略。这意味着在执行评论策略之前，系统会检查帖子策略是否允许进行评论操作。这种依赖关系可以帮助您构建更复杂、更灵活的数据访问控制规则。

```sql
create table posts (
  id            serial    primary key,
  creator_id    uuid      not null     references auth.users(id),
  title         text      not null,
  body          text      not null,
  publish_date  date      not null     default now(),
  audience      uuid[]    null -- many to many table omitted for brevity
);

create table comments (
  id            serial    primary key,
  post_id       int       not null     references posts(id)  on delete cascade,
  user_id       uuid      not null     references auth.users(id),
  body          text      not null,
  comment_date  date      not null     default now()
);

create policy "Creator can see their own posts"
on posts
for select
using (
  auth.uid() = posts.creator_id
);

create policy "Logged in users can see the posts if they belong to the post 'audience'."
on posts
for select
using (
  auth.uid() = any (posts.audience)
);

create policy "Users can see all comments for posts they have access to."
on comments
for select
using (
  exists (
    select 1 from posts
    where posts.id = comments.post_id
  )
);
```

## 提示

### 为数据库表启用Realtime功能

实时服务器根据行级别安全（RLS）策略向授权用户广播数据库更改。
建议您对添加到发布中的表启用行级别安全性并设置行安全策略。
但是，您可以选择禁用表上的RLS，并将更改广播到所有连接的客户端。

```sql
/**
 * REALTIME SUBSCRIPTIONS
 * Realtime enables listening to any table in your public schema.
 */

begin;
  -- remove the realtime publication
  drop publication if exists supabase_realtime;

  -- re-create the publication but don't enable it for any tables
  create publication supabase_realtime;
commit;

-- add a table to the publication
alter publication supabase_realtime add table products;

-- add other tables to the publication
alter publication supabase_realtime add table posts;
```

### 你并不一定要使用策略

您也可以将授权规则放在中间件中，类似于使用任何其他`后端<->中间件<->前端`架构，在此架构中创建安全规则的方式。

策略是一种工具。在“serverless/Jamstack”设置的情况下，它们特别有效，因为您根本不必部署任何中间件。

然而，如果您想为应用程序使用另一种授权方法，这也可以。MemFireCloud只是“普通的Postgres”，所以如果您的应用程序 与Postgres一起工作，那么它也可以与MemFireCloud一起工作。

如果你打算使用这种方法，请确保为你的表启用RLS（行级安全性）。然后使用service_role密钥（对于我们的客户端库）或postgres角色 - 这两者都可以绕过RLS。
使用这种方法，你无需创建任何策略，仅启用RLS就足够了："

```sql
create table profiles (
  id serial primary key,
  email text
);

alter table profiles enable row level security;
```

### 切勿在客户端上使用服务密钥

MemFireCloud提供特殊的服务密钥（service_key），它可以绕过所有RLS。 而这个服务密钥不应在浏览器中使用或向客户公开，但它们对于管理任务非常有用。

{{% alert context="info" %}}
如果设置了用户令牌使用服务密钥初始化客户端，不会覆盖行级安全（RLS）。如果用户使用客户端登录，MemFireCloud将遵循该用户的行级安全策略。
{{% /alert %}}


### 测试相关的策略
MemFireCloud提供了一些辅助SQL过程，允许您在数据库中直接测试数据访问策略，而无需通过前端界面并模拟不同用户登录来进行测试。这些辅助过程可以帮助您更方便地验证和调试数据库的策略设置。

```sql
grant anon, authenticated to postgres;

create or replace procedure auth.login_as_user (user_email text)
    language plpgsql
    as $$
declare
    auth_user auth.users;
begin
    select
        * into auth_user
    from
        auth.users
    where
        email = user_email;
    execute format('set request.jwt.claim.sub=%L', (auth_user).id::text);
    execute format('set request.jwt.claim.role=%I', (auth_user).role);
    execute format('set request.jwt.claim.email=%L', (auth_user).email);
    execute format('set request.jwt.claims=%L', json_strip_nulls(json_build_object('app_metadata', (auth_user).raw_app_meta_data))::text);

    raise notice '%', format( 'set role %I; -- logging in as %L (%L)', (auth_user).role, (auth_user).id, (auth_user).email);
    execute format('set role %I', (auth_user).role);
end;
$$;

create or replace procedure auth.login_as_anon ()
    language plpgsql
    as $$
begin
    set request.jwt.claim.sub='';
    set request.jwt.claim.role='';
    set request.jwt.claim.email='';
    set request.jwt.claims='';
    set role anon;
end;
$$;

create or replace procedure auth.logout ()
    language plpgsql
    as $$
begin
    set request.jwt.claim.sub='';
    set request.jwt.claim.role='';
    set request.jwt.claim.email='';
    set request.jwt.claims='';
    set role postgres;
end;
$$;
```
要切换到指定用户（通过电子邮件），使用 `call auth.login_as_user('my@email.com')`;。您也可以切换到匿名角色，使用 `call auth.login_as_anon()`;。
完成后，使用 `call auth.logout()`; 将自己切换回 `postgres` 角色。

这些过程也可以用于编写针对策略的 pgTAP 单元测试。


查看psql使用此功能的交互示例。

该示例表明，在教程示例中，`public.profiles` 表确实可以由`postgres`角色和行的所有者进行更新，但无法通过`匿名`连接进行更新。

```sql

postgres=> select id, email from auth.users;

{{< table "table-striped-columns" >}}
                  id                  |       email
--------------------------------------+-------------------
 d4f0aa86-e6f6-41d1-bd32-391f077cf1b9 | user1@example.com
 15d6811a-16ee-4fa2-9b18-b63085688be4 | user2@example.com
 4e1010bb-eb37-4a4d-a05a-b0ee315c9d56 | user3@example.com
 {{< /table >}}
(3 rows)

postgres=> table public.profiles;
{{< table "table-striped-columns" >}}
                  id                  | updated_at | username | full_name | avatar_url | website
--------------------------------------+------------+----------+-----------+------------+---------
 d4f0aa86-e6f6-41d1-bd32-391f077cf1b9 |            | user1    | User 1    |            |
 15d6811a-16ee-4fa2-9b18-b63085688be4 |            | user2    | User 2    |            |
 4e1010bb-eb37-4a4d-a05a-b0ee315c9d56 |            | user3    | User 3    |            |
 {{< /table >}}
(3 rows)

postgres=> call auth.login_as_anon();
CALL
postgres=> update public.profiles set updated_at=now();
UPDATE 0 -- anon users cannot update any profile but see all of them
postgres=> table public.profiles;
{{< table "table-striped-columns" >}}
                  id                  | updated_at | username | full_name | avatar_url | website
--------------------------------------+------------+----------+-----------+------------+---------
 d4f0aa86-e6f6-41d1-bd32-391f077cf1b9 |            | user1    | User 1    |            |
 15d6811a-16ee-4fa2-9b18-b63085688be4 |            | user2    | User 2    |            |
 4e1010bb-eb37-4a4d-a05a-b0ee315c9d56 |            | user3    | User 3    |            |
 {{< /table >}}
(3 rows)

postgres=> call auth.logout();
CALL
postgres=> call auth.login_as_user('user1@example.com');
NOTICE:  set role authenticated; -- logging in as 'd4f0aa86-e6f6-41d1-bd32-391f077cf1b9' ('user1@example.com')
CALL
postgres=> update public.profiles set updated_at=now();
UPDATE 1 -- authenticated users can update their own profile and see all of them
postgres=> table public.profiles;
{{< table "table-striped-columns" >}}
                  id                  |          updated_at           | username | full_name | avatar_url | website
--------------------------------------+-------------------------------+----------+-----------+------------+---------
 15d6811a-16ee-4fa2-9b18-b63085688be4 |                               | user1    | User 1    |            |
 4e1010bb-eb37-4a4d-a05a-b0ee315c9d56 |                               | user2    | User 2    |            |
 d4f0aa86-e6f6-41d1-bd32-391f077cf1b9 | 2023-02-18 21:39:16.204612+00 | user3    | User 3    |            |
  {{< /table >}}
(3 rows)

postgres=> call auth.logout();
CALL
postgres=> update public.profiles set updated_at=now();
UPDATE 3 -- the 'postgres' role can update and see all profiles
postgres=> table public.profiles;
{{< table "table-striped-columns" >}}
                  id                  |          updated_at           | username | full_name | avatar_url | website
--------------------------------------+-------------------------------+----------+-----------+------------+---------
 15d6811a-16ee-4fa2-9b18-b63085688be4 | 2023-02-18 21:40:08.216324+00 | user1    | User 1    |            |
 4e1010bb-eb37-4a4d-a05a-b0ee315c9d56 | 2023-02-18 21:40:08.216324+00 | user2    | User 2    |            |
 d4f0aa86-e6f6-41d1-bd32-391f077cf1b9 | 2023-02-18 21:40:08.216324+00 | user3    | User 3    |            |
  {{< /table >}}
(3 rows)

```

## 已经弃用的功能

我们已经弃用了一些功能，以确保RLS策略具有更好的性能和可扩展性。

`auth.role()`

{{% alert context="info" %}}
`auth.role()` 函数已被弃用，推荐使用 Postgres 中原生支持的 `TO` 字段。
{{% /alert %}}

```sql
-- DEPRECATED
create policy "Public profiles are viewable by everyone."
on profiles for select using (
  auth.role() = 'authenticated' or auth.role() = 'anon'
);

-- RECOMMENDED
create policy "Public profiles are viewable by everyone."
on profiles for select
to authenticated, anon
using (
  true
);
```

`auth.email()`
{{% alert context="info" %}}
`auth.email()` 函数已被弃用，转而使用 `auth.jwt() ->> 'email'`。
{{% /alert %}}

返回提出请求的用户的电子邮件。


