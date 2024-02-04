---
    weight: 10
    title: "from.remove()"
    icon: "article"
    draft: false
    toc: true
---

删除同一桶内的文件


```dart
final List<FileObject> objects = await supabase
  .storage
  .from('avatars')
  .remove(['avatar1.png']);
```






## Notes

- 需要的政策权限。
  - `buckets`权限: 无 
  - `objects`的权限: `delete`和  `select`权限










## Examples

### 删除文件



```dart
final List<FileObject> objects = await supabase
  .storage
  .from('avatars')
  .remove(['avatar1.png']);
```