---
    weight: 408
    title: "signOut()"
    icon: "article"
    draft: false
    toc: true
---

退出当前用户，如果有一个登录的用户。


```dart
await supabase.auth.signOut();
```






## Notes

- 为了使用`signOut()`方法，用户需要先退出。










## Examples

### Sign out



```dart
await supabase.auth.signOut();
```