---
    weight: 23
    title: "range()"
    icon: "article"
    draft: false
    toc: true
---

将结果限制在指定范围内的行，包括在内。


```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .range(0,3);
```


















## Examples

### 使用 `select()`



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .range(0,3);
```