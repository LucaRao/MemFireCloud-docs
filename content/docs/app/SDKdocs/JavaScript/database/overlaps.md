---
    weight: 228
    title: "overlaps()"
    icon: "article"
    draft: false
    toc: true
---


仅适用于数组列和范围列

仅匹配`列（column）`和`值（value）`有一个共同元素的行。


## 案例教程

### 案例1  （关于数组列）

{{< tabs tabTotal="7" >}}
 
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
  .from('issues')
  .select('title')
  .overlaps('tags', ['is:closed', 'severity:high'])
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



### 案例2  （关于范围列）

{{< tabs tabTotal="7" >}}
 
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
  .overlaps('during', '[2000-01-01 12:45, 2000-01-01 13:15)')
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

要进行过滤的数组或范围列

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

用于过滤的数组或范围值

  </div>
  
</li>

</ul>