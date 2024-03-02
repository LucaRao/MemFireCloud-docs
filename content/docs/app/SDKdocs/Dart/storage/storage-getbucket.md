---
    weight: 359
    title: "getBucket()"
    icon: "article"
    draft: false
    toc: true
---

检索现有存储桶的详细信息。


```dart
final Bucket bucket = await supabase
  .storage
  .getBucket('avatars');
```






## Notes

- 需要的政策权限。
  - `buckets`的权限。`select` 的权限 
  - `objects`权限: 无










## Examples

### 获取桶



```dart
final Bucket bucket = await supabase
  .storage
  .getBucket('avatars');
```