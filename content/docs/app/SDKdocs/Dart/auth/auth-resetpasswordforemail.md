---
    weight: 306
    title: "resetPasswordForEmail()"
    icon: "article"
    draft: false
    toc: true
---

向电子邮件地址发送重置请求。


`redirectTo` is used to open the app via deeplink when user opens the password reset email. 
```dart
await supabase.auth.resetPasswordForEmail(
  'sample@email.com',
  redirectTo: kIsWeb ? null : 'io.supabase.flutter://reset-callback/',
);
```






## Notes

向一个电子邮件地址发送一个密码重置请求。当用户点击邮件中的重置链接时，他们会被重定向到你的应用程序。提示用户输入新的密码并调用auth.updateUser()。

```dart
await supabase.auth.resetPasswordForEmail(
  'sample@email.com',
  redirectTo: kIsWeb ? null : 'io.supabase.flutter://reset-callback/',
);
```










## Examples

### 重置Flutter的密码



`redirectTo` is used to open the app via deeplink when user opens the password reset email. 
```dart
await supabase.auth.resetPasswordForEmail(
  'sample@email.com',
  redirectTo: kIsWeb ? null : 'io.supabase.flutter://reset-callback/',
);
```