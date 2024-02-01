---
    weight: 82
    title: "neq()"
    icon: "article"
    draft: false
    toc: true
---

neq()用于匹配`列`值不等于`指定值`的行。




## 案例教程
### 案例1 (使用select)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .neq('name', 'The shire');
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
  .neq('name', 'San Francisco');
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
  .neq('name', 'Mordor');
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
  .neq('name', 'Lagos');
```


{{% /tab %}}

{{< /tabs >}}