---
    weight: 1411
    title: "使用电子邮件登录"
    description: "使用MemFireCloud通过电子邮件对用户进行身份验证和授权。"
    icon: "article"
    draft: false
    toc: true
---

## 概述

为 MemFireCloud 应用程序设置电子邮件登录。

- 将电子邮件验证器添加到[MemFire Cloud项目](https://cloud.memfiredb.com)
- 将登录代码添加到应用程序 - [JavaScript](https://github.com/supabase/supabase-js) | [Flutter](https://github.com/supabase/supabase-flutter)

## 配置电子邮件设置

1. 对于 [网站 URL](https://app.supabase.com/project/_/auth/url-configuration), 输入应用程序的最终（托管）URL。
1. 对于 [身份验证服务商](https://app.supabase.com/project/_/auth/providers), **启用电子邮件提供程序**.

{{% alert context="info" %}}
对于自托管，可以使用提供的文件和环境变量更新项目配置。
有关详细信息，请参阅[自托管文档](/docs/app/hosting/static-start)。
{{% /alert %}}

## 将登录代码添加到客户端应用程序

{{< tabs tabTotal="4" >}}

{{% tab tabName="JavaScript" %}}



当用户登录时，使用其电子邮件地址和密码调用[signInWithPassword()](/docs/app/SDKdocs/JavaScript/auth/auth-signinwithpassword)：

```js
async function signInWithEmail() {
  const { data, error } = await supabase.auth.signInWithPassword({
    email: 'example@email.com',
    password: 'example-password',
  })
}
```



{{% /tab %}}
{{% tab tabName="Dart" %}}



当用户登录时，使用其电子邮件地址和密码调用[signInWithPassword()](/docs/app/SDKdocs/JavaScript/auth/auth-signinwithpassword)：

```dart
Future<void> signInWithEmail() async {
  final AuthResponse res = await supabase.auth.signInWithPassword(
    email: 'example@email.com',
    password: 'example-password'
  );
}
```



{{% /tab %}}

{{< /tabs >}}

{{< tabs tabTotal="4" >}}

{{% tab tabName="JavaScript" %}}



当用户注销时，调用[signOut()](/docs/app/SDKdocs/JavaScript/auth/auth-signOut)将其从浏览器会话和localStorage中删除：

```js
async function signOut() {
  const { error } = await supabase.auth.signOut()
}
```



{{% /tab %}}
{{% tab tabName="Dart" %}}



当用户注销时，调用[signOut()](/docs/app/SDKdocs/JavaScript/auth/auth-signOut)将其从浏览器会话和localStorage中删除：

```dart
Future<void> signOut() async {
   await supabase.auth.signOut();
}
```



{{% /tab %}}

{{< /tabs >}}

## 资料

- [MemFire Cloud](https://cloud.memfiredb.com)
- [JS客户端](https://github.com/supabase/supabase-js)
- [Flutter客户端](https://github.com/supabase/supabase-flutter)


