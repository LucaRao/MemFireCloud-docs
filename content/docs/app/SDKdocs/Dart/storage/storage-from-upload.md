---
    weight: 348
    title: "from.upload()"
    icon: "article"
    draft: false
    toc: true
---

将一个文件上传到一个现有的桶。


```dart
final avatarFile = File('path/to/file');
final String path = await supabase.storage.from('avatars').upload(
      'public/avatar1.png',
      avatarFile,
      fileOptions: const FileOptions(cacheControl: '3600', upsert: false),
    );
```






## Notes

- 需要的政策权限。
  - `buckets`权限: 无 
  - `objects`的权限: `insert`










## Examples

### 上传文件



```dart
final avatarFile = File('path/to/file');
final String path = await supabase.storage.from('avatars').upload(
      'public/avatar1.png',
      avatarFile,
      fileOptions: const FileOptions(cacheControl: '3600', upsert: false),
    );
```