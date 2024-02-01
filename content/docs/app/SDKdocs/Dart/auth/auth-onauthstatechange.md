---
    weight: 69
    title: "onAuthStateChange()"
    icon: "article"
    draft: false
    toc: true
---

每次发生认证事件时都会收到通知。


```dart
final authSubscription = supabase.auth.onAuthStateChange.listen((data) {
  final AuthChangeEvent event = data.event;
  final Session? session = data.session;
});
```






## Notes

- 认证事件的类型: `AuthChangeEvent.passwordRecovery`, `AuthChangeEvent.signedIn`, `AuthChangeEvent.signedOut`, `AuthChangeEvent.tokenRefreshed`, `AuthChangeEvent.userUpdated`and `AuthChangeEvent.userDeleted`










## Examples

### 监听认证变化



```dart
final authSubscription = supabase.auth.onAuthStateChange.listen((data) {
  final AuthChangeEvent event = data.event;
  final Session? session = data.session;
});
```

### 监听一个特定的事件



```dart
final authSubscription = supabase.auth.onAuthStateChange.listen((data) {
  final AuthChangeEvent event = data.event;
  if (event == AuthChangeEvent.signedIn) {
    // handle signIn
  }
});
```

### 退订自动订阅



```dart
final authSubscription = supabase.auth.onAuthStateChange((event, session) {});

authSubscription.cancel();
```