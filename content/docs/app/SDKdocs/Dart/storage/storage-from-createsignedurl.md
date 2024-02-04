---
    weight: 10
    title: "from.createSignedUrl()"
    icon: "article"
    draft: false
    toc: true
---

创建签名的网址，下载文件而不需要权限。这个URL可以在设定的秒数内有效。


```dart
  final String signedUrl = await supabase
    .storage
    .from('avatars')
    .createSignedUrl('avatar1.png', 60);
```






## Notes

- 需要的政策权限。
  - `buckets`权限: 无 
  - `objects`的权限: `select`










## Examples

### 创建签名的URL



```dart
  final String signedUrl = await supabase
    .storage
    .from('avatars')
    .createSignedUrl('avatar1.png', 60);
```