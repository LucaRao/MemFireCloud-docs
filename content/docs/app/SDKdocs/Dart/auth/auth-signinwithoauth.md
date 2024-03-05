---
    weight: 402
    title: "signInWithOAuth()"
    icon: "article"
    draft: false
    toc: true
---

使用第三方OAuth提供商对用户进行签名。


```dart
await supabase.auth.signInWithOAuth(Provider.github);
```






## Notes

- 这种方法用于使用第三方提供商进行登录。
- Supabase支持许多不同的[第三方提供商](https://supabase.com/docs/app/auth/auth#providers)。










## Examples

### 使用第三方供应商登录



```dart
await supabase.auth.signInWithOAuth(Provider.github);
```

### 使用 `redirectTo`

指定重定向链接，通过深层链接把用户带回来。
注意，对于Flutter Web，`redirectTo`应该是空的。


```dart
await supabase.auth.signInWithOAuth(
  Provider.github,
  redirectTo: kIsWeb ? null : 'io.supabase.flutter://reset-callback/',
);
```

### 使用 scopes

如果你需要OAuth提供商的额外数据，你可以在你的请求中包括一个空格分隔的范围列表，以获得OAuth提供商的令牌。
你可能还需要在提供者的OAuth应用设置中指定范围，这取决于提供者。


```dart
await supabase.auth.signInWithOAuth(
  Provider.github,
  scopes: 'repo gist notifications'
);
...
// after user comes back from signin flow

final Session? session = supabase.auth.currentSession;
final String? oAuthToken = session?.providerToken;
```