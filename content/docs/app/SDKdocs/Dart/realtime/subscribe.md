---
    weight: 2622
    title: "on().subscribe()"
    icon: "article"
    draft: false
    toc: true
---

订阅你的数据库中的实时变化。


```dart
supabase.channel('*').on(
  RealtimeListenTypes.postgresChanges,
  ChannelFilter(event: '*', schema: '*'),
  (payload, [ref]) {
    print('Change received: ${payload.toString()}');
  },
).subscribe();
```






## Notes

- 为了提高数据库性能和安全性，新项目的实时性默认是禁用的。你可以通过[管理复制](/docs/app/development_guide/api/api#managing-realtime)打开它。
- 如果你想在更新和删除时接收 "以前的"数据，你需要将`REPLICA IDENTITY`设置为`FULL`，像这样。`ALTER TABLE your_table REPLICA IDENTITY FULL;`。










## Examples

### 监听所有的数据库变化



```dart
supabase.channel('*').on(
  RealtimeListenTypes.postgresChanges,
  ChannelFilter(event: '*', schema: '*'),
  (payload, [ref]) {
    print('Change received: ${payload.toString()}');
  },
).subscribe();
```

### 监听一个特定的表



```dart
supabase.channel('public:countries').on(
  RealtimeListenTypes.postgresChanges,
  ChannelFilter(event: '*', schema: 'public', table: 'countries'),
  (payload, [ref]) {
    print('Change received: ${payload.toString()}');
  },
).subscribe();
```

### 监听插入



```dart
supabase.channel('public:countries').on(
  RealtimeListenTypes.postgresChanges,
  ChannelFilter(event: 'INSERT', schema: 'public', table: 'countries'),
  (payload, [ref]) {
    print('Change received: ${payload.toString()}');
  },
).subscribe();
```

### 监听修改

默认情况下，Supabase 将只发送更新的记录。如果你想同时接收以前的值，你可以 
为你监听的表启用完全复制。

```sql
alter table "your_table" replica identity full;
```


```dart
supabase.channel('public:countries').on(
  RealtimeListenTypes.postgresChanges,
  ChannelFilter(event: 'UPDATE', schema: 'public', table: 'countries'),
  (payload, [ref]) {
    print('Change received: ${payload.toString()}');
  },
).subscribe();
```

### 监听删除

默认情况下，Supabase 不发送已删除的记录。如果你想接收删除的记录，你可以 
为你所监听的表启用完全复制功能。

```sql
alter table "your_table" replica identity full;
```


```dart
supabase.channel('public:countries').on(
  RealtimeListenTypes.postgresChanges,
  ChannelFilter(event: 'DELETE', schema: 'public', table: 'countries'),
  (payload, [ref]) {
    print('Change received: ${payload.toString()}');
  },
).subscribe();
```

### 监听多个事件

如果你想监听每个表的多个事件，你可以用链式监听器。

```dart
supabase.channel('public:countries').on(RealtimeListenTypes.postgresChanges,
    ChannelFilter(event: 'INSERT', schema: 'public', table: 'countries'),
    (payload, [ref]) {
  print('Change received: ${payload.toString()}');
}).on(RealtimeListenTypes.postgresChanges,
    ChannelFilter(event: 'DELETE', schema: 'public', table: 'countries'),
    (payload, [ref]) {
  print('Change received: ${payload.toString()}');
}).subscribe();
```

### 监听行级变化

你可以使用`{table}:{col}=eq.{val}`的格式来监听单个行，其中`{col}`是列名，`{val}`是你想要匹配的值。

```dart
supabase.channel('public:countries:id=eq.200').on(
    RealtimeListenTypes.postgresChanges,
    ChannelFilter(
      event: 'UPDATE',
      schema: 'public',
      table: 'countries',
      filter: 'id=eq.200',
    ), (payload, [ref]) {
  print('Change received: ${payload.toString()}');
}).subscribe();
```