---
    weight: 23
    title: "Update 数据"
    icon: "article"
    draft: false
    toc: true
---

update()用于对表（table）或视图（view）执行 UPDATE 操作。


* `update()`应该始终与筛选器[Filters](/docs/app/SDKdocs/dartdatabase/using-filters)结合使用，以便定位您希望更新的项目。


## 案例教程
### 案例1 (更新数据)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
await supabase
  .from('cities')
  .update({ 'name': 'Middle Earth' })
  .match({ 'name': 'Auckland' });
```


{{% /tab %}}


{{< /tabs >}}


### 案例2 (更新JSON数据)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
await supabase
  .from('users')
  .update({
    'address': {
      'street': 'Melrose Place',
      'postcode': 90210
    }
  })
  .eq('address->postcode', 90210);
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



Postgres提供了一个 
[运算符的数量](https://www.postgresql.org/docs/current/functions-json.html) 
用于处理JSON数据。现在，它只能更新整个JSON文档。
但我们正在[研究更新单个键的想法](https://github.com/PostgREST/postgrest/issues/465)。




{{% /tab %}}


{{< /tabs >}}


### 案例3 (获取更新的行)

{{< tabs tabTotal="4" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final List<Map<String, dynamic>> data = await supabase
    .from('users')
    .update({
      'address': {'street': 'Melrose Place', 'postcode': 90210}
    })
    .eq('address->postcode', 90210)
    .select();
```


{{% /tab %}}

{{< /tabs >}}
