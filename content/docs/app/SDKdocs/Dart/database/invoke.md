---
    weight: 70
    title: "invoke()"
    icon: "article"
    draft: false
    toc: true
---

调用一个Supabase函数。请参阅[指南](/docs/app/functions/functions)，了解关于编写函数的详细信息。


```dart
final res = await supabase.functions.invoke('hello', body: {'foo': 'baa'});
final data = res.data;
```






## Notes

- 需要一个授权标头。
- 调用参数通常符合[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)规范。










## Examples

### 基本调用。



```dart
final res = await supabase.functions.invoke('hello', body: {'foo': 'baa'});
final data = res.data;
```

### 指定响应类型。

默认情况下，`invoke()`将把响应解析为JSON。你可以用以下格式解析响应。`json`, `blob`, `text`, 和`arrayBuffer`.


```dart
final res = await supabase.functions.invoke(
  'hello',
  body: {'foo': 'baa'},
  responseType: ResponseType.text,
);
final data = res.data;
```

### 解析自定义头信息。

任何 `headers信息`都将被传递给该函数。一个常见的模式是将登录用户的JWT令牌作为授权标头传递。


```dart
final res = await supabase.functions.invoke(
  'hello',
  body: {'foo': 'baa'},
  headers: {
    'Authorization': 'Bearer ${supabase.auth.currentSession?.accessToken}'
  },
);
```