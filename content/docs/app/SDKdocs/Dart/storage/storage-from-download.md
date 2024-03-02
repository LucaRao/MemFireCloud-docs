---
    weight: 347
    title: "from.download()"
    icon: "article"
    draft: false
    toc: true
---

下载文件


```dart
final Uint8List file = await supabase
  .storage
  .from('avatars')
  .download('avatar1.png');
```






## Notes

- 需要的政策权限。
  - `buckets`权限: 无 
  - `objects`的权限: `select`










## Examples

### 下载文件



```dart
final Uint8List file = await supabase
  .storage
  .from('avatars')
  .download('avatar1.png');
```