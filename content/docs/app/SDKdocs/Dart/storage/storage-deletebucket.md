---
    weight: 2645
    title: "deleteBucket()"
    icon: "article"
    draft: false
    toc: true
---

删除一个现有的桶。一个桶不能在其内部存在对象的情况下被删除。你必须首先`empty()`该桶。


```dart
final String result = await supabase
  .storage
  .deleteBucket('avatars');
```






## Notes

- 需要的政策权限。
  - `buckets`的权限。`select` and `delete`。
  - `objects`的权限: 没有










## Examples

### 删除桶



```dart
final String result = await supabase
  .storage
  .deleteBucket('avatars');
```