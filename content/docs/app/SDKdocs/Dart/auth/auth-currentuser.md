---
    weight: 404
    title: "currentUser"
    icon: "article"
    draft: false
    toc: true
---

返回用户数据，如果有一个登录的用户。


```dart
final User? user = supabase.auth.currentUser;
```


















## Examples

### 获取登录的用户



```dart
final User? user = supabase.auth.currentUser;
```