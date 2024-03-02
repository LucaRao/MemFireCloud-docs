---
    weight: 353
    title: "from.getPublicUrl()"
    icon: "article"
    draft: false
    toc: true
---

检索公共资源库中资产的URL


```dart
  final String publicUrl = supabase
    .storage
    .from('public-bucket')
    .getPublicUrl('avatar1.png');
```






## Notes

- 水桶需要被设置为公开，可以通过[updateBucket()](/docs/app/SDKdocs/JavaScript/storage/storag-updatebucket)或通过进入[app.supabase.com](https://app.supabase.com)的存储，点击水桶上的溢出菜单并选择 "Make public"
- 需要的策略权限。
  - `buckets`权限: 无 
  - `objects`权限: 无










## Examples

### 返回公共桶中资产的URL。



```dart
  final String publicUrl = supabase
    .storage
    .from('public-bucket')
    .getPublicUrl('avatar1.png');
```