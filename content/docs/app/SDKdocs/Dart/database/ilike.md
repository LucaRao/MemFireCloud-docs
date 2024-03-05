---
    weight: 424
    title: "ilike()"
    icon: "article"
    draft: false
    toc: true
---

ilike()用于查找所有在所述`列（column）`上的值与提供的 `模板（pattern）`相符的记录（不区分大小写）。





## 案例教程
### 案例1 (使用select)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .ilike('name', '%la%');
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
  .ilike('name', '%la%');
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
  .ilike('name', '%la%');
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
  .ilike('name', '%la%');
```


{{% /tab %}}

{{< /tabs >}}