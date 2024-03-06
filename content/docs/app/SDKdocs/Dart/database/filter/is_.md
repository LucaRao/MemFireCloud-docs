---
    weight: 2540
    title: "is_()"
    icon: "article"
    draft: false
    toc: true
---


is_()用于检查是否完全相等(null, true, false),找到所有在所述`列（column）`上的值与指定的`值（value）`完全匹配的记录。

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
  .is_('name', null);
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
  .is_('name', null);
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
  .is_('name', null);
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
  .is_('name', null);
```


{{% /tab %}}

{{< /tabs >}}