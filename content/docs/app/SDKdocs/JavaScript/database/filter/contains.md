---
    weight: 2342
    title: "contains()"
    icon: "article"
    draft: false
    toc: true
---


该方法仅用于在 jsonb、数组（array）和范围（range）列上进行过滤

contains()的作用是匹配包含指定元素的行。

换句话说，在指定列中，匹配出来的记录的值是给定`数组（array）`的子集。

也就是说，在指定列中，给定的`数组（array）`包含了匹配出记录的所有元素。



## 案例教程

### 案例1 （关于数组列）

{{< tabs tabTotal="10" >}}
 
{{% tab tabName="建表" %}}



  ```sql
create table
  issues (
    id int8 primary key,
    title text,
    tags text[]
  );

insert into
  issues (id, title, tags)
values
  (1, 'Cache invalidation is not working', array['is:open', 'severity:high', 'priority:low']),
  (2, 'Use better names', array['is:open', 'severity:low', 'priority:medium']);

  ```



{{% /tab %}}
{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase
  .from('users')
  .select()
  .contains('tags', ['is:open', 'severity:high', 'priority:low']);  
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
{
  "data": [
    {
      "title": "Cache invalidation is not working"
    }
  ],
  "status": 200,
  "statusText": "OK"
}

  ```



{{% /tab %}}


{{< /tabs >}}

### 案例2 （关于范围列）


{{< tabs tabTotal="10" >}}
 
{{% tab tabName="建表" %}}



  ```sql
create table
  reservations (
    id int8 primary key,
    room_name text,
    during tsrange
  );

insert into
  reservations (id, room_name, during)
values
  (1, 'Emerald', '[2000-01-01 13:00, 2000-01-01 15:00)'),
  (2, 'Topaz', '[2000-01-02 09:00, 2000-01-02 10:00)');

  ```



{{% /tab %}}
{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase
  .from('reservations')
  .select()
  .contains('during', '[2000-01-01 13:00, 2000-01-01 13:30)')
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
{
  "data": [
    {
      "id": 1,
      "room_name": "Emerald",
      "during": "[\"2000-01-01 13:00:00\",\"2000-01-01 15:00:00\")"
    }
  ],
  "status": 200,
  "statusText": "OK"
}
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



Postgres 支持多种[范围类型](https://www.postgresql.org/docs/current/rangetypes.html)。您可以使用范围值的字符串表示来过滤范围列。



{{% /tab %}}

{{< /tabs >}}






### 案例3 （关于jsonb列）

{{< tabs tabTotal="10" >}}
 
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
  (1, 'Michael', '{ "postcode": 90210, "street": "Melrose Place" }'),
  (2, 'Jane', '{}');
  ```



{{% /tab %}}
{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase
  .from('users')
  .select('name')
  .contains('address', { postcode: 90210 })
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
{
  "data": [
    {
      "name": "Michael"
    }
  ],
  "status": 200,
  "statusText": "OK"
}
  ```



{{% /tab %}}


{{< /tabs >}}









## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      列（column）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

要过滤的jsonb、数组或范围列

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      值（value）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>object类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用来过滤的jsonb、数组或范围值

  </div>
  
</li>

</ul>











