---
    weight: 2650
    title: "from.update()"
    icon: "article"
    draft: false
    toc: true
---

用一个新的文件替换指定路径下的一个现有文件。


```dart
final avatarFile = File('path/to/local/file');
final String path = await supabase.storage.from('avatars').update(
      'public/avatar1.png',
      avatarFile,
      fileOptions: const FileOptions(cacheControl: '3600', upsert: false),
    );
```






## Notes

- 需要的政策权限。
  - `buckets`权限: 无 
  - `objects` 的权限: `update` and `select`










## Examples

### 更新文件



```dart
final avatarFile = File('path/to/local/file');
final String path = await supabase.storage.from('avatars').update(
      'public/avatar1.png',
      avatarFile,
      fileOptions: const FileOptions(cacheControl: '3600', upsert: false),
    );
```