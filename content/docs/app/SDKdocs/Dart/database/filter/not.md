---
    weight: 434
    title: ".not()"
    icon: "article"
    draft: false
    toc: true
---

找到所有不符合过滤器要求的行。


```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .not('name', 'eq', 'Paris');
```






## Notes

- `.not()`希望你使用原始的[PostgREST语法](https://postgrest.org/en/stable/api.html#horizontal-filtering-rows)作为过滤器的名称和值。

  ```dart
  .not('name','eq','Paris')
  .not('arraycol','cs','{"a","b"}') // Use Postgres array {} for array column and 'cs' for contains.
  .not('rangecol','cs','(1,2]') // Use Postgres range syntax for range column.
  .not('id','in','(6,7)')  // Use Postgres list () and 'in' for in_ filter.
  .not('id','in','(${mylist.join(',')})')  // You can insert a Dart list array.
  ```










## Examples

### 使用 `select()`



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .not('name', 'eq', 'Paris');
```

### 使用 `update()`



```dart
final data = await supabase
  .from('cities')
  .update({ 'name': 'Mordor' })
  .not('name', 'eq', 'Paris');
```

### 使用 `delete()`



```dart
final data = await supabase
  .from('cities')
  .delete()
  .not('name', 'eq', 'Paris');
```

### 使用 `rpc()`



```dart
// Only valid if the Stored Procedure returns a table type.
final data = await supabase
  .rpc('echo_all_cities)
  .not('name', 'eq', 'Paris');
```