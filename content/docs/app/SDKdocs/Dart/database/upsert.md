---
    weight: 414
    title: "Upsert 数据"
    icon: "article"
    draft: false
    toc: true
---

upsert()用于对表（table）或视图（view）执行 UPSERT 操作。

在关系型数据库中，Upsert是一种结合了"插入（Insert）"和"更新（Update）"的操作,它允许我们在表或视图上执行插入或更新操作。
通常情况下，当我们想要向数据库中插入一行数据时，我们会使用INSERT语句。
但是，如果该行数据已经存在（通常通过主键来判断），我们可能希望更新该行数据而不是插入重复的数据。

Upsert通过传递列到`onConflict`方法，我们可以使用`.upsert()`来实现以下功能：

1. 如果不存在具有相应`onConflict`列的行，则执行等效于`.insert()`的插入操作。
2. 如果存在具有相应`onConflict`列的行，则根据`ignoreDuplicates`的设置执行另一种操作。

需要注意的是，为了使用upsert，必须在`values`中包含主键。主键是用于唯一标识表中每一行的一列或一组列，确保数据的唯一性和完整性。



## 案例教程
### 案例1 (Upsert数据)

{{< tabs tabTotal="5" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
await supabase
  .from('messages')
  .upsert({ 'id': 3, 'message': 'foo', 'username': 'supabot' });
```


{{% /tab %}}


{{< /tabs >}}


### 案例2 (将数据Upsert到带有约束的表中)

{{< tabs tabTotal="5" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
await supabase
  .from('users')
  .upsert({ 'username': 'supabot' }, { 'onConflict': 'username' });
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



运行以下代码将使 Supabase 进行数据的 UPSERT 操作到 "users" 表中。如果用户名 'supabot' 已经存在，onConflict 参数会告诉 Supabase 根据传递给 onConflict 的列来覆盖那一行的数据。




{{% /tab %}}


{{< /tabs >}}


### 案例3 (返回确切的行数)

{{< tabs tabTotal="5" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final res = await supabase.from('users').upsert(
  {'id': 3, 'message': 'foo', 'username': 'supabot'},
  options: const FetchOptions(count: CountOption.exact),
);

final data = res.data;
final count = res.count;
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



`count` 选项的允许取值为：exact、planned 和 estimated。



{{% /tab %}}

{{< /tabs >}}
