---
    weight: 1414
    title: "使用Magic Link登录"
    description: "使用Supabase使用魔术链接对用户进行身份验证和授权。"
    icon: "article"
    draft: false
    toc: true
---

Magic Link是一种无密码登录的形式，用户单击发送到其电子邮件地址的链接即可登录其帐户。
Magic Link仅适用于电子邮件地址。默认情况下，用户只能每60秒请求一次Magic Link。

## 概述

为MemfireCloud应用程序提供Magic Link登录。

- 将登录代码添加到应用程序 - [JavaScript](https://github.com/supabase/supabase-js) | [Flutter](https://github.com/supabase/supabase-flutter)

## 将Magic Link添加到您的 MemfireCloud 项目中

1. 对于 网站 URL（`用户认证`-> `URL 配置`）, 输入应用程序的最终（托管）URL。
1. 对于 身份验证服务商（`用户认证`-> `服务商`）, **启用电子邮件提供程序**.

## 将登录代码添加到客户端应用程序

{{< tabs tabTotal="4" >}}


{{% tab tabName="JavaScript" %}}



当您的用户登录时，使用其电子邮件地址调用[signInWithOtp()](/docs/app/SDKdocs/JavaScript/auth/auth-signinwithotp):

```js
async function signInWithEmail() {
  const { data, error } = await supabase.auth.signInWithOtp({
    email: 'example@email.com',
  })
}
```



{{% /tab %}}
{{% tab tabName="Dart" %}}



当您的用户登录时，使用其电子邮件地址调用[signIn()](/docs/app/SDKdocs/Dart/auth/auth-signinwithotp):

```dart
Future<void> signInWithEmail() async {
  final AuthResponse res = await supabase.auth.signinwithotp(email: 'example@email.com');
}
```



{{% /tab %}}

{{< /tabs >}}

{{< tabs tabTotal="4" >}}

{{% tab tabName="JavaScript" %}}



当用户注销时，调用[signOut()](/docs/app/SDKdocs/JavaScript/auth/auth-signout)将其从浏览器会话和localStorage中删除:

```js
async function signOut() {
  const { error } = await supabase.auth.signOut()
}
```



{{% /tab %}}
{{% tab tabName="Dart" %}}



当用户注销时，调用[signOut()](/docs/app/SDKdocs/Dart/auth/auth-signout)将其从浏览器会话和localStorage中删除:

```dart
Future<void> signOut() async {
  await supabase.auth.signOut();
}
```



{{% /tab %}}

{{< /tabs >}}

## 资源

- [MemFire Cloud](https://cloud.memfiredb.com)
- [JS客户端](https://github.com/supabase/supabase-js)
- [Flutter客户端](https://github.com/supabase/supabase-flutter)


