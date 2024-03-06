---
    weight: 429
    title: "limit()"
    icon: "article"
    draft: false
    toc: true
---

用指定的计数来限制结果。


```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .limit(1);
```


















## Examples

### 使用 `select()`



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .limit(1);
```

### 有嵌入式资源



```dart
final data = await supabase
  .from('countries')
  .select('name, cities(name)')
  .eq('name', 'United States')
  .limit(1,  foreignTable: 'cities' );
```