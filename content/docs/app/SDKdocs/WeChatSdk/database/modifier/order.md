---
    weight: 2773
    title: "order()"
    icon: "article"
    draft: false
    toc: true
---

按列对查询结果进行排序。

* 你可以多次调用这个方法来按多列排序。

* 你可以对外部表进行排序，但这并不影响对当前表的排序。



## 案例教程 

### 案例1  （使用select）

{{< tabs tabTotal="7" >}}

  
  
  
  
>

{{% tab tabName="建表" %}}



```sql
create table
  countries (id int8 primary key, name text);

insert into
  countries (id, name)
values
  (1, 'Afghanistan'),
  (2, 'Albania'),
  (3, 'Algeria');

```



{{% /tab %}}

{{% tab tabName="使用方法" %}}



```ts
const { data, error } = await supabase
  .from('countries')
  .select('id', 'name')
  .order('id', { ascending: false })
```



{{% /tab %}}

{{% tab tabName="返回结果" %}}



```json
{
  "data": [
    {
      "id": 3,
      "name": "Algeria"
    },
    {
      "id": 2,
      "name": "Albania"
    },
    {
      "id": 1,
      "name": "Afghanistan"
    }
  ],
  "status": 200,
  "statusText": "OK"
}
```


{{% /tab %}}

{{< /tabs >}}




### 案例2  （在外部表）

{{< tabs tabTotal="7" >}}

  
  
  
  
>

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
  (1, 'United States'),
  (2, 'Vanuatu');
insert into
  cities (id, country_id, name)
values
  (1, 1, 'Atlanta'),
  (2, 1, 'New York City');
```



{{% /tab %}}

{{% tab tabName="使用方法" %}}



```ts
  const { data, error } = await supabase
    .from('countries')
    .select(`
      name,
      cities (
        name
      )
    `)
    .order('name', { foreignTable: 'cities', ascending: false })
```



{{% /tab %}}

{{% tab tabName="返回结果" %}}



```json
{
  "data": [
    {
      "name": "United States",
      "cities": [
        {
          "name": "New York City"
        },
        {
          "name": "Atlanta"
        }
      ]
    },
    {
      "name": "Vanuatu",
      "cities": []
    }
  ],
  "status": 200,
  "statusText": "OK"
}
```


{{% /tab %}}


{{% tab tabName="注意事项" %}}



对外部表进行排序不会影响父表的排序。



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

要排序的列

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      选项（option）
    </span>
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>[可选参数]</code>
    </span>
  </h4>
  <div class="method-list-item-description">

命名的参数

  </div>
  
<ul className="method-list-group">
  <h5 class="method-list-title method-list-title-isChild expanded">特性</h5>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      foreignTable
    </span>
    <span className="method-list-item-label-badge false">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

设置此选项以按外域表的列

  </div>
  
</li>






<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      ascending
    </span>
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>boolean类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

如果 "true"，结果将按升序排列。

  </div>
  
</li>





<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      nullsFirst
    </span>
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>boolean</code>
    </span>
  </h4>
  <div class="method-list-item-description">

如果 true，null首先出现。如果 false,则 null 出现在最后。

  </div>
  
</li>

</ul>

</li>

</ul>
