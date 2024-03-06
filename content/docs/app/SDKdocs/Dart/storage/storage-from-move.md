---
    weight: 2651
    title: "from.move()"
    icon: "article"
    draft: false
    toc: true
---

移动一个现有的文件，同时也可以选择重命名。


```dart
final String result = await supabase
  .storage
  .from('avatars')
  .move('public/avatar1.png', 'private/avatar2.png');
```






## Notes

- 需要的政策权限。
  - `buckets`权限: 无 
  - `objects`的权限: `update`和 `select`的权限










## Examples

### 移动文件



```dart
final String result = await supabase
  .storage
  .from('avatars')
  .move('public/avatar1.png', 'private/avatar2.png');
```