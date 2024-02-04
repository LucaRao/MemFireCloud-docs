---
    weight: 22
    title: "verifyOtp()"
    icon: "article"
    draft: false
    toc: true
---



```dart
final AuthResponse res = await supabase.auth.verifyOTP(
  type: OtpType.sms,
  token: '111111',
  phone: '+13334445555',
);
final Session? session = res.session;
final User? user = res.user;
```






## Notes

- `verifyOtp`方法接受不同的验证类型。如果使用电话号码，类型可以是`sms`或`phone_change`。如果使用的是电子邮件地址，类型可以是下列之一。`signup`, `magiclink`, `recovery`, `invite` 或 `email_change`.
- 使用的验证类型应该根据在`verifyOtp`之前调用的相应的auth方法来确定，以注册/登录一个用户。










## Examples

### 验证短信一次性密码(OTP)



```dart
final AuthResponse res = await supabase.auth.verifyOTP(
  type: OtpType.sms,
  token: '111111',
  phone: '+13334445555',
);
final Session? session = res.session;
final User? user = res.user;
```

### 验证注册 一次性密码(OTP)



```dart
final AuthResponse res = await supabase.auth.verifyOTP(
  type: OtpType.signup,
  token: token,
  phone: '+13334445555',
);
final Session? session = res.session;
final User? user = res.user;
```