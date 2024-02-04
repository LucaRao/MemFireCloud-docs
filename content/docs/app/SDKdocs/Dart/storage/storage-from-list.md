---
    weight: 10
    title: "from.list()"
    icon: "article"
    draft: false
    toc: true
---

列出一个桶内的所有文件。


```dart
final List<FileObject> objects = await supabase
  .storage
  .from('avatars')
  .list();
```






## Notes

- 需要的政策权限。
  - `buckets`权限: 无 
  - `objects`的权限: `select`










## Examples

### 在一个桶中列出文件



```dart
final List<FileObject> objects = await supabase
  .storage
  .from('avatars')
  .list();
```