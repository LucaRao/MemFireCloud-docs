---
    weight: 464
    title: "stream()"
    icon: "article"
    draft: false
    toc: true
---

通知被查询表的数据。


```dart
supabase.from('countries')
  .stream(primaryKey: ['id'])
  .listen((List<Map<String, dynamic>> data) {
  // Do something awesome with the data
});
```






## Notes

- `stream()`通过结合Postgrest和Realtime，将初始数据以及数据库上的任何进一步变化作为`List<Map<String, dynamic>`的`Stream`发出。
- 接受一个主键列的列表作为其参数。










## Examples

### 监听一个特定的表



```dart
supabase.from('countries')
  .stream(primaryKey: ['id'])
  .listen((List<Map<String, dynamic>> data) {
  // Do something awesome with the data
});
```

### 监听表格中的特定行数

你可以使用`{table}:{col}=eq.{val}`的格式来监听个别行，其中`{col}`是列名，`{val}`是你想要匹配的值。
这种语法与你在Realtime中过滤数据的方式相同


```dart
supabase.from('countries')
  .stream(primaryKey: ['id'])
  .eq('id', '120')
  .listen((List<Map<String, dynamic>> data) {
  // Do something awesome with the data
});
```

### 使用 `order()`



```dart
supabase.from('countries')
  .stream(primaryKey: ['id'])
  .order('name', ascending: true)
  .listen((List<Map<String, dynamic>> data) {
  // Do something awesome with the data
});
```

### 使用 `limit()`



```dart
supabase.from('countries')
  .stream(primaryKey: ['id'])
  .order('name', ascending: true)
  .limit(10)
  .listen((List<Map<String, dynamic>> data) {
  // Do something awesome with the data
});
```

### 使用 `stream()`与 `StreamBuilder`的关系

当在你的Flutter应用程序中使用`stream()`与`StreamBuilder`时，确保将你的流存储在一个变量中，以防止重建时重新获取。


```dart
final supabase = Supabase.instance.client;

class MyWidget extends StatefulWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  // Persist the stream in a local variable to prevent refetching upon rebuilds
  final _stream = supabase.from('countries').stream(primaryKey: ['id']);

  @override
  Widget build(BuildContext context) {
    return StreamBuilder(
      stream: _stream,
      builder: (context, snapshot) {
        // Return your widget with the data from the snapshot
      },
    );
  }
}
```