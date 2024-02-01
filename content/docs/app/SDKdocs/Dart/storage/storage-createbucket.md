---
    weight: 60
    title: "createBucket()"
    icon: "article"
    draft: false
    toc: true
---

创建一个新的存储桶


```dart
final String bucketId = await supabase
  .storage
  .createBucket('avatars');
```






## Notes

- 需要的政策权限。
  - `buckets`的权限。`insert`的权限 
  -  `objects`的权限: 没有










## Examples

### 创建桶



```dart
final String bucketId = await supabase
  .storage
  .createBucket('avatars');
```