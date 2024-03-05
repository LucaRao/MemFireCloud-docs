---
    weight: 401
    title: "signInWithPassword()"
    icon: "article"
    draft: false
    toc: true
---

使用电子邮件或电话号码与密码登录现有用户。


```dart
final AuthResponse res = await supabase.auth.signInWithPassword(
  email: 'example@email.com',
  password: 'example-password',
);
final Session? session = res.session;
final User? user = res.user;
```






## Notes

- 需要电子邮件和密码或电话号码和密码。










## Examples

### 用电子邮件和密码登录



```dart
final AuthResponse res = await supabase.auth.signInWithPassword(
  email: 'example@email.com',
  password: 'example-password',
);
final Session? session = res.session;
final User? user = res.user;
```

### 用电话和密码登录



```dart
final AuthResponse res = await supabase.auth.signInWithPassword(
  phone: '+13334445555',
  password: 'example-password',
);
final Session? session = res.session;
final User? user = res.user;
```