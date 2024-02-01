---
    weight: 5
    title: "signUp()"
    icon: "article"
    draft: false
    toc: true
---

创建一个新的用户。


```dart
final AuthResponse res = await supabase.auth.signUp(
  email: 'example@email.com',
  password: 'example-password',
);
final Session? session = res.session;
final User? user = res.user;
```






## Notes

- 默认情况下，用户在登录前需要验证他们的电子邮件地址。要关闭这个功能，请在[你的项目](https://app.supabase.com/project/_/auth/settings)中禁用**确认电子邮件**。
- **确认电子邮件**决定了用户在注册后是否需要确认他们的电子邮件地址。
  - 如果**确认电子邮件**被启用，将返回一个`用户`，但`会话`为空。
  - 如果**确认电子邮件**被禁用，则同时返回一个`用户'和一个`会话`。
- 当用户确认他们的电子邮件地址时，他们默认会被重定向到[`SITE_URL`](https://supabase.com/docs/reference/auth/config#site_url)。你可以修改你的`SITE_URL`或在[你的项目](https://app.supabase.com/project/_/auth/settings)中添加额外的重定向URL。
- 如果为一个现有的确认用户调用signUp()。
    - 如果在[你的项目](https://app.supabase.com/project/_/auth/settings)中启用了**确认邮件**，将返回一个混淆的/假的用户对象。
    - 如果**确认电子邮件**被禁用，将返回错误信息 "用户已经注册"。










## Examples

### Sign up.



```dart
final AuthResponse res = await supabase.auth.signUp(
  email: 'example@email.com',
  password: 'example-password',
);
final Session? session = res.session;
final User? user = res.user;
```

### 与第三方供应商签约。

如果你使用Flutter，你可以使用`supabase_flutter`上的[signInWithOAuth()`](/docs/app/SDKdocs/dartauth/auth-signinwithoauth)方法与OAuth提供商签约。