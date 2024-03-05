---
    weight: 412
    title: "Delete 数据"
    icon: "article"
    draft: false
    toc: true
---

delete()用于在表（table）或视图（view）执行 DELETE 操作。

* `delete()` 应始终与[过滤器（filter）](/docs/app/SDKdocs/dartdatabase/using-filters)结合使用，以便定位要删除的项。
* 如果你在使用 `delete()` 时带有过滤器，并且启用了RLS（行级安全性），则只会删除通过  `SELECT` 策略可见的行。请注意，默认情况下没有行可见，因此你需要至少有一个 `SELECT`/`ALL` 策略来使行可见。




## 案例教程
### 案例1 (删除记录)

{{< tabs tabTotal="2" >}}

  
  


{{% tab tabName="使用方法" %}}



```dart
                                                                              
await supabase
  .from('cities')
  .delete()
  .match({ 'id': 666 });
```


{{% /tab %}}


{{< /tabs >}}

### 案例2 (找回已删除的记录)

{{< tabs tabTotal="2" >}}

  
  
  
  

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final List<Map<String,dynamic>> data = await supabase
  .from('cities')
  .delete()
  .match({ 'id': 666 })
  .select();
```


{{% /tab %}}


{{< /tabs >}}