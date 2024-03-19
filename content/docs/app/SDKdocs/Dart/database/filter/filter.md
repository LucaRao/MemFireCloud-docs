---
    weight: 2554
    title: ".filter()"
    icon: "article"
    draft: false
    toc: true
---

找到所有`column`符合过滤器的记录。


```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .filter('name', 'in', '("Paris","Tokyo")');
```






## Notes

- `.filter()`希望你使用原始的[PostgREST语法](https://postgrest.org/en/stable/api.html#horizontal-filtering-rows)来表示过滤器的名称和值，所以它只能作为其他过滤器不工作时的一个转义。
  ```dart
    .filter('arraycol','cs','{"a","b"}') // Use Postgres array {} and 'cs' for contains.
    .filter('rangecol','cs','(1,2]') // Use Postgres range syntax for range column.
    .filter('id','in','(6,7)')  // Use Postgres list () and 'in' for in_ filter.
    .filter('id','cs','{${mylist.join(',')}}')  // You can insert a Dart array list.
  ```










## Examples

### 使用 `select()`



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .filter('name', 'in', '("Paris","Tokyo")');
```

### 使用 `update()`



```dart
final data = await supabase
  .from('cities')
  .update({ 'name': 'Mordor' })
  .filter('name', 'in', '("Paris","Tokyo")');
```

### 使用 `delete()`



```dart
final data = await supabase
  .from('cities')
  .delete()
  .filter('name', 'in', '("Paris","Tokyo")');
```

### 使用 `rpc()`



```dart
// Only valid if the Stored Procedure returns a table type.
final data = await supabase
  .rpc('echo_all_cities')
  .filter('name', 'in', '("Paris","Tokyo")');
```

### Filter embedded resources



```dart
final data = await supabase
  .from('cities')
  .select('name, countries ( name )')
  .filter('countries.name', 'in', '("France","Japan")');
```