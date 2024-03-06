---
    weight: 2522
    title: "Insert 数据"
    icon: "article"
    draft: false
    toc: true
---

insert()用于在表(table)或视图(view)执行 INSERT 操作。

## 案例教程
### 案例1 (创建记录)

{{< tabs tabTotal="3" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                                                                                                            
await supabase
    .from('cities')
    .insert({'name': 'The Shire', 'country_id': 554});
```


{{% /tab %}}


{{< /tabs >}}


### 案例2 (批量创建)

{{< tabs tabTotal="3" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
await supabase.from('cities').insert([
  {'name': 'The Shire', 'country_id': 554},
  {'name': 'Rohan', 'country_id': 555},
]);
```


{{% /tab %}}

{{< /tabs >}}


### 案例3 (获取插入的记录)

{{< tabs tabTotal="3" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final List<Map<String, dynamic>> data =
        await supabase.from('cities').insert([
      {'name': 'The Shire', 'country_id': 554},
      {'name': 'Rohan', 'country_id': 555},
    ]).select();
```


{{% /tab %}}

{{< /tabs >}}
