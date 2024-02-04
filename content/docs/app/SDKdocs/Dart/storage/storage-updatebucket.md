---
    weight: 10
    title: "updateBucket()"
    icon: "article"
    draft: false
    toc: true
---

更新一个新的存储桶


```dart
final res = await supabase
  .storage
  .updateBucket('avatars', const BucketOptions(public: false));
```






## Notes

- 需要的政策权限。
  - `buckets`的权限。`update`的权限
  - `objects` 的权限: 没有










## Examples

### 更新桶



```dart
final res = await supabase
  .storage
  .updateBucket('avatars', const BucketOptions(public: false));
```