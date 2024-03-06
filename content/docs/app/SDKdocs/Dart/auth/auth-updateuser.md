---
    weight: 2599
    title: "updateUser()"
    icon: "article"
    draft: false
    toc: true
---

更新用户数据，如果有一个登录的用户。


```dart
final UserResponse res = await supabase.auth.updateUser(
  UserAttributes(
    email: 'example@email.com',
  ),
);
final User? updatedUser = res.user;
```






## Notes

- 为了使用`updateUser()`方法，用户需要先登录。
- 默认情况下，电子邮件更新会向用户的当前和新的电子邮件发送一个确认链接。
要想只向用户的新邮箱发送确认链接，请在你的项目的[email auth provider settings](https://app.supabase.com/project/_/auth/settings)中禁用**安全的邮件变更**。










## Examples

### 更新已认证用户的电子邮件

向新的电子邮件地址发送一封 "确认电子邮件变更 "的电子邮件。

```dart
final UserResponse res = await supabase.auth.updateUser(
  UserAttributes(
    email: 'example@email.com',
  ),
);
final User? updatedUser = res.user;
```

### 更新一个已认证用户的密码



```dart
final UserResponse res = await supabase.auth.updateUser(
  UserAttributes(
    password: 'new password',
  ),
);
final User? updatedUser = res.user;
```

### 更新用户的元数据



```dart
final UserResponse res = await supabase.auth.updateUser(
  UserAttributes(
    data: { 'hello': 'world' },
  ),
);
final User? updatedUser = res.user;
```