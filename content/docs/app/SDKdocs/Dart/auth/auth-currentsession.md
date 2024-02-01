---
    weight: 65
    title: "currentSession"
    icon: "article"
    draft: false
    toc: true
---

如果有一个活动的会话，返回会话数据。


```dart
final Session? session = supabase.auth.currentSession;
```


















## Examples

### 获取会话数据



```dart
final Session? session = supabase.auth.currentSession;
```