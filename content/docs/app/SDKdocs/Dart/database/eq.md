---
    weight: 317
    title: "eq()"
    icon: "article"
    draft: false
    toc: true
---

eq()用于匹配`列`值等于`指定值`的行。

## 案例教程
### 案例1 (使用select)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .eq('name', 'The shire');
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
  .eq('name', 'San Francisco');
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
  .eq('name', 'Mordor');
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
  .eq('name', 'San Francisco');
```


{{% /tab %}}

{{< /tabs >}}