---
    weight: 335
    title: ".or()"
    icon: "article"
    draft: false
    toc: true
---

找到所有满足至少一个过滤器的行。


```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .or('id.eq.20,id.eq.30');
```






## Notes

- `.or()`希望你使用原始的[PostgREST语法](https://postgrest.org/en/stable/api.html#horizontal-filtering-rows)作为过滤器的名称和值。

  ```dart
  .or('id.in.(6,7),arraycol.cs.{"a","b"}')  // Use Postgres list () and 'in' for in_ filter. Array {} and 'cs' for contains.
  .or('id.in.(${mylist.join(',')}),arraycol.cs.{${mylistArray.join(',')}}')	// You can insert a Dart list for list or array column.
  .or('id.in.(${mylist.join(',')}),rangecol.cs.(${mylistRange.join(',')}]')	// You can insert a Dart list for list or range column.
  ```










## Examples

### 使用 `select()`



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .or('id.eq.20,id.eq.30');
```

### 使用 `or`与 `and`。



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .or('id.gt.20,and(name.eq.New Zealand,name.eq.France)');
```