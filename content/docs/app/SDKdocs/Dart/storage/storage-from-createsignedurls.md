---
    weight: 2654
    title: "from.createSignedUrls()"
    icon: "article"
    draft: false
    toc: true
---



```dart
  final List<String> signedUrls = await supabase
    .storage
    .from('avatars')
    .createSignedUrls(['folder/avatar1.png', 'folder/avatar2.png'], 60);
```






## Notes

- 需要RLS策略权限。
  - `buckets`表的权限: 没有
  - `objects`表的权限: `select`.
- 请参考[存储指南](/docs/app/storage/storage#access-control)中关于访问控制的工作方法










## Examples

### 创建签名的URL



```dart
  final List<String> signedUrls = await supabase
    .storage
    .from('avatars')
    .createSignedUrls(['folder/avatar1.png', 'folder/avatar2.png'], 60);
```