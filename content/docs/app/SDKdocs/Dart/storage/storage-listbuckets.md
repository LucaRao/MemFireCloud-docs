---
    weight: 57
    title: "listBuckets()"
    icon: "article"
    draft: false
    toc: true
---

检索一个现有产品中所有存储桶的详细信息。


```dart
final List<Bucket> buckets = await supabase
  .storage
  .listBuckets();
```






## Notes

- 需要的政策权限。
  - `buckets`的权限。`select`的权限 
  - `objects`权限: 无










## Examples

### 列表中的桶



```dart
final List<Bucket> buckets = await supabase
  .storage
  .listBuckets();
```