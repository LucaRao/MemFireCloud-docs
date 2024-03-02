---
    weight: 345
    title: "textSearch()"
    icon: "article"
    draft: false
    toc: true
---


textSearch()作用是找到所有在指定列上的 tsvector 值与给定的 to_tsquery 查询条件匹配的记录。







## 案例教程
### 案例1 (文本搜索)

{{< tabs tabTotal="7" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('quotes')
  .select('catchphrase')
  .textSearch('catchphrase', "'fat' & 'cat'",
    config: 'english'
  );
```


{{% /tab %}}

{{< /tabs >}}


### 案例2 (基本归一化)

{{< tabs tabTotal="7" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('quotes')
  .select('catchphrase')
  .textSearch('catchphrase', "'fat' & 'cat'",
    type: TextSearchType.plain,
    config: 'english'
  );
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



使用 PostgreSQL 的 `plainto_tsquery` 函数。



{{% /tab %}}


{{< /tabs >}}




### 案例3 (全面归一化)

{{< tabs tabTotal="7" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('quotes')
  .select('catchphrase')
  .textSearch('catchphrase', "'fat' & 'cat'",
    type: TextSearchType.phrase,
    config: 'english'
  );
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



使用 PostgreSQL 的 `plainto_tsquery` 函数。



{{% /tab %}}

{{< /tabs >}}


### 案例4 (Websearch)

{{< tabs tabTotal="7" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('quotes')
  .select('catchphrase')
  .textSearch('catchphrase', "'fat or cat'",
    type: TextSearchType.websearch,
    config: 'english'
  );
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



使用PostgreSQL的websearch_to_tsquery函数。 这个函数不会引发语法错误，这使得使用用户提供的原始输入进行搜索成为可能，并且可以使用 与高级运算符一起使用。

* `未加引号的文本`：不在引号内的文本将被转换为由&运算符分隔的术语，就像由plainto_tsquery处理一样。
* `带引号的文本`：带引号的文本将被转换为由<->运算符分隔的术语，就像由phraseto_tsquery处理的那样。
* `OR`:“or”字将被转换为 | 操作符。
* `—`：破折号将被转换为操作符！。



{{% /tab %}}


{{< /tabs >}}