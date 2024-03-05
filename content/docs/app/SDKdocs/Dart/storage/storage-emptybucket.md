---
    weight: 456
    title: "emptyBucket()"
    icon: "article"
    draft: false
    toc: true
---

删除单个桶内的所有对象。


```dart
final String result = await supabase
  .storage
  .emptyBucket('avatars');
```






## Notes

- 需要的政策权限。
  - `buckets`的权限。 `select` 的权限 
  - `objects`的权限: `select`和`delete`的权限










## Examples

### 清空存储桶



```dart
final String result = await supabase
  .storage
  .emptyBucket('avatars');
```