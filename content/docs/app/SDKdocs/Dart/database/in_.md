---
    weight: 10
    title: "in_()"
    icon: "article"
    draft: false
    toc: true
---

in_()用于查找所有在指定`列（column）`上数值存在于指定值`列表（arry）`中的记录

`is_`和`in_`过滤方法的后缀是`_`，以避免与保留的关键字发生冲突。





## 案例教程
### 案例1 (使用select)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .in_('name', ['Rio de Janeiro', 'San Francisco']);
```


{{% /tab %}}

{{< /tabs >}}


### 案例2 (使用update)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
final data = await supabase
  .from('cities')
  .update({ 'name': 'Mordor' })
  .in_('name', ['Rio de Janeiro', 'San Francisco']);
```


{{% /tab %}}

{{< /tabs >}}




### 案例3 (使用delete)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
final data = await supabase
  .from('cities')
  .delete()
  .in_('name', ['Rio de Janeiro', 'San Francisco']);
```


{{% /tab %}}

{{< /tabs >}}


### 案例4 (使用rpc)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
// Only valid if the Stored Procedure returns a table type.
final data = await supabase
  .rpc('echo_all_cities')
  .in_('name', ['Rio de Janeiro', 'San Francisco']);
```


{{% /tab %}}

{{< /tabs >}}