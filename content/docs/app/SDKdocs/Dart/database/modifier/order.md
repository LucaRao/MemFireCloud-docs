---
    weight: 2572
    title: "order()"
    icon: "article"
    draft: false
    toc: true
---

用指定的列对结果进行排序。


```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .order('id',  ascending: false );
```


















## Examples

### 使用 `select()`



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .order('id',  ascending: false );
```

### 有嵌入式资源



```dart
final data = await supabase
  .from('countries')
  .select('name, cities(name)')
  .eq('name', 'United States')
  .order('name', foreignTable: 'cities');
```