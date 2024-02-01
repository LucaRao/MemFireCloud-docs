---
    weight: 65
    title: "single()"
    icon: "article"
    draft: false
    toc: true
---

只从结果中检索一条记录。结果必须是一行(例如,使用limit)，否则会导致错误。


```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .single();
```


















## Examples

### 使用 `select()`



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .single();
```