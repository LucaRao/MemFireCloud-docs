---
    weight: 410
    title: "Select 查询"
    icon: "article"
    draft: false
    toc: true
---

select()用于对表格或视图执行SELECT查询。

* 默认情况下，Supabase项目将返回最多1,000行。这个设置可以在项目API设置中更改。建议将其保持较低，以限制意外或恶意请求的有效负载大小。您可以使用range()查询来分页处理数据。
* `select()`可以与过滤器[Filter](/docs/app/SDKdocs/dartdatabase/using-filters)组合使用
* `select()`可以与修改器[Modifier](/docs/app/SDKdocs/dartdatabase/using-modifiers)组合使用
* 如果您正在使用Supabase平台，`apikey`是一个保留关键字，[应避免将其作为列名](https://github.com/supabase/supabase/issues/5465)。




## 案例教程
### 案例1 (获取您的数据)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('cities')
  .select('name');
```


{{% /tab %}}


{{< /tabs >}}


### 案例2 (选择特定的列)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('countries')
  .select('''
    name,
    cities (
      name
    )
  ''');
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



你可以从你的表中选择特定的字段。



{{% /tab %}}


{{< /tabs >}}


### 案例3 (查询外键表)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('countries')
  .select('''
    name,
    cities (
      name
    )
  ''');
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



如果您的数据库有关联关系，您也可以查询相关的表。



{{% /tab %}}


{{< /tabs >}}


### 案例4 (多次查询同一个外键表)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('messages')
  .select('*, users!inner(*)')
  .eq('users.username', 'Jane');
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}




有时您需要对同一个外部表进行两次查询。在这种情况下，您可以使用连接列的名称来标识您打算使用的连接。
为了方便起见，您还可以为每个列提供一个别名。例如，如果我们有一个产品商店，并且我们想同时获取供应商和购买者（都在 users 表中）的信息：



{{% /tab %}}


{{< /tabs >}}


### 案例5 (使用内连接进行筛选)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('messages')
  .select('*, users!inner(*)')
  .eq('users.username', 'Jane');
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



如果您想根据子表的值来筛选一个表，可以使用 `!inner()` 函数。例如，如果您想选择所有属于用户名为 "Jane" 的用户的消息表中的行：



{{% /tab %}}


{{< /tabs >}}



### 案例6 (使用计数选项进行查询)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final res = await supabase.from('cities').select(
      'name',
      const FetchOptions(
        count: CountOption.exact,
      ),
    );

final count = res.count;
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



你可以通过使用count选项来获得行的数量。
count选项的允许值是[exact](https://postgrest.org/en/stable/api.html#exact-count)，[planned](https://postgrest.org/en/stable/api.html#planned-count)和[estimated](https://postgrest.org/en/stable/api.html#estimated-count)。



{{% /tab %}}


{{< /tabs >}}




### 案例7 (查询JSON数据)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('users')
  .select('''
    id, name,
    address->street
  ''')
  .eq('address->postcode', 90210);
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



如果你有一个JSONB列内的数据，你可以对数据值应用选择 
和查询过滤器到数据值。Postgres提供了一个 
[操作数](https://www.postgresql.org/docs/current/functions-json.html) 
用于查询JSON数据。还可以看到 
[PostgREST docs](http://postgrest.org/en/v7.0.0/api.html#json-columns) 了解更多细节。



{{% /tab %}}


{{< /tabs >}}


### 案例8 (以CSV格式返回数据)

{{< tabs tabTotal="15" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('users')
  .select()
  .csv();
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



默认情况下，数据以JSON格式返回，但你也可以要求以逗号分隔值的形式返回。



{{% /tab %}}


{{< /tabs >}}
