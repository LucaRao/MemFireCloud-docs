---
    weight: 29
    title: "overlaps()"
    icon: "article"
    draft: false
    toc: true
---


## 案例教程
### 案例1 (使用select)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('countries')
  .select('name, id, main_exports')
  .overlaps('main_exports', ['computers', 'minerals']);
```


{{% /tab %}}

{{< /tabs >}}


### 案例2 (使用update)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('countries')
  .update({ 'name': 'Mordor' })
  .overlaps('main_exports', ['computers', 'minerals']);
```


{{% /tab %}}

{{< /tabs >}}




### 案例3 (使用delete)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('countries')
  .delete()
  .overlaps('main_exports', ['computers', 'minerals']);
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
  .rpc('echo_all_countries')
  .overlaps('main_exports', ['computers', 'minerals']);
```


{{% /tab %}}

{{< /tabs >}}