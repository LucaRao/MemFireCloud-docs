---
    weight: 38
    title: "removeAllChannels()"
    icon: "article"
    draft: false
    toc: true
---

取消订阅并从Realtime客户端删除所有Realtime频道。


```dart
final statuses = await supabase.removeAllChannels();
```






## Notes

- 如果你在监听Postgres的变化，移除通道是保持你项目的Realtime服务以及数据库性能的一个好方法。Supabase会在客户端断开连接后的30秒内自动处理清理工作，但未使用的通道可能会因为更多的客户端同时订阅而导致性能下降。










## Examples

### 删除所有通道



```dart
final statuses = await supabase.removeAllChannels();
```