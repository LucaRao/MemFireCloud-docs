---
    weight: 2544
    title: "rangeGt()"
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
  .select('name, id, population_range_millions')
  .rangeGt('population_range_millions', '[150, 250]');
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
  .rangeGt('population_range_millions', '[150, 250]');
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
  .rangeGt('population_range_millions', '[150, 250]');
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
  .rangeGt('population_range_millions', '[150, 250]');
```


{{% /tab %}}

{{< /tabs >}}