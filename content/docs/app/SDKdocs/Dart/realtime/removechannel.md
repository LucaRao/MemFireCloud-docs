---
    weight: 2623
    title: "removeChannel()"
    icon: "article"
    draft: false
    toc: true
---

取消订阅并从Realtime客户端删除Realtime频道。


```dart
final status = await supabase.removeChannel(channel);
```






## Notes

- 如果你在监听Postgres的变化，删除一个通道是保持你项目的Realtime服务以及你的数据库性能的一个好方法。Supabase会在客户端断开连接后的30秒内自动处理清理工作，但未使用的通道可能会因为更多的客户端同时订阅而导致性能下降。










## Examples

### 删除一个通道



```dart
final status = await supabase.removeChannel(channel);
```