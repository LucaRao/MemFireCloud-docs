---
    weight: 1562
    title: "复制"
    description: ""
    icon: "article"
    draft: false
    toc: true
---

复制是一种将数据从一个数据库复制到另一个数据库的技术。Supabase使用复制功能来提供一个实时的API。复制在以下方面很有用：

- 分散 "负载"。例如，如果你的数据库有大量的读数，你可能想把它分成两个数据库。
- 减少延时。例如，您可能希望伦敦有一个数据库为您的欧洲客户服务，而纽约有一个为美国服务。

复制是通过发布来完成的，这是一种选择将哪些变化发送到其他系统（通常是另一个Postgres数据库）的方法。发布可以在仪表板中管理，也可以用SQL来管理。

## 在仪表板中管理发布

1. 进入仪表板中的**数据库**页面。
2. 点击侧边栏中的**复制**。
3. 通过切换**插入**、**更新**和**删除**来控制哪些数据库事件被发送。
4. 通过选择**源**和切换每个表来控制哪些表被发送变化。

<video width="99%" muted playsInline controls="true">
  <source src="../../../../videos/api/api-realtime.mp4" type="video/mp4" muted playsInline />
</video>

## 创建一个发布

这个发布包含对所有表格的修改

```sql
create publication publication_name
for all tables;
```

## 创建发布以侦听各个表

```sql
create publication publication_name
for table table_one, table_two;
```

## 添加表到现有发布中

```sql
alter publication publication_name
add table table_name;
```

## 监听插入操作

```sql
create publication publication_name
for all tables
with (publish = 'insert');
```

## 监听更新操作

```sql
create publication publication_name
for all tables
with (publish = 'update');
```

## 监听删除操作

```sql
create publication publication_name
for all tables
with (publish = 'delete');
```

## 删除一个发布
```sql
drop publication if exists publication_name;
```

## 重新创建发布

如果要重新创建发布，最好在事务中执行，以确保操作成功。

```sql
begin;
  -- remove the realtime publication
  drop publication if exists publication_name;

  -- re-create the publication but don't enable it for any tables
  create publication publication_name;
commit;
```


