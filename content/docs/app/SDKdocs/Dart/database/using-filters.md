---
    weight: 315
    title: "使用过滤器"
    icon: "article"
    draft: false
    toc: true
---

过滤器允许你只返回符合某些条件的记录。

过滤器可以用于`select()`、`update()`和`delete()`查询。

如果一个数据库函数返回一个表的响应，你也可以应用过滤器。


## 案例教程
### 案例1 (使用过滤器)

{{< tabs tabTotal="12" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .eq('name', 'The Shire');  // Correct

final data = await supabase
  .from('cities')
  .eq('name', 'The Shire')  // Incorrect
  .select('name, country_id');
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



过滤器必须在`select()`, `update()`、`upsert()`、`delete()`和`rpc()`之后，并在[修改器](/docs/app/SDKdocs/dartdatabase/using-modifiers)之前应用。



{{% /tab %}}


{{< /tabs >}}




### 案例2 (链式过滤)

{{< tabs tabTotal="12" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('cities')
  .select('name, country_id')
  .gte('population', 1000)
  .lt('population', 10000)
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



过滤器必须在`select()`, `update()`、`upsert()`、`delete()`和`rpc()`之后，并在[修改器](/docs/app/SDKdocs/dartdatabase/using-modifiers)之前应用。



{{% /tab %}}


{{< /tabs >}}



### 案例3 (条件链式)

{{< tabs tabTotal="12" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final filterByName = null;
final filterPopLow = 1000;
final filterPopHigh = 10000;

var query = supabase
  .from('cities')
  .select('name, country_id');

if (filterByName != null)  { query = query.eq('name', filterByName); }
if (filterPopLow != null)  { query = query.gte('population', filterPopLow); }
if (filterPopHigh != null) { query = query.lt('population', filterPopHigh); }

final data = await query;
```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



过滤器可以一步步建立起来，然后执行。这个例子就可以很好地说明。



{{% /tab %}}


{{< /tabs >}}


### 案例4 (按JSON列中的值过滤)

{{< tabs tabTotal="12" >}}

  
  
  
  
{{% tab tabName="建表" %}}



```sql
                                                                              
create table
  users (
    id int8 primary key,
    name text,
    address jsonb
  );

insert into
  users (id, name, address)
values
  (1, 'Michael', '{ "postcode": 90210 }'),
  (2, 'Jane', null);

```


{{% /tab %}}


{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('users')
  .select()
  .eq('address->postcode', 90210);
```


{{% /tab %}}


{{% tab tabName="返回结果" %}}



```json
                                                                              
{
  "data": [
    {
      "id": 1,
      "name": "Michael",
      "address": {
        "postcode": 90210
      }
    }
  ],
  "status": 200,
  "statusText": "OK"
}

```


{{% /tab %}}

{{% tab tabName="注意事项" %}}



过滤器可以一步步建立起来，然后执行。这个例子就可以很好地说明。



{{% /tab %}}


{{< /tabs >}}

### 案例5 (过滤外键表)

{{< tabs tabTotal="12" >}}

  
  
  
  
{{% tab tabName="建表" %}}



```sql
                                                                              
create table
  countries (id int8 primary key, name text);
create table
  cities (
    id int8 primary key,
    country_id int8 not null references countries,
    name text
  );

insert into
  countries (id, name)
values
  (1, 'Germany'),
  (2, 'Indonesia');
insert into
  cities (id, country_id, name)
values
  (1, 2, 'Bali'),
  (2, 1, 'Munich');
```


{{% /tab %}}


{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .from('countries')
  .select('''
    name,
    cities!inner (
      name
    )
  ''')
  .eq('cities.name', 'Bali');
```


{{% /tab %}}


{{< /tabs >}}