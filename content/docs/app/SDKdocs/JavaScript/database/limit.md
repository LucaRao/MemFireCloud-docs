---
    weight: 219
    title: "limit()"
    icon: "article"
    draft: false
    toc: true
---

通过`count`限制查询结果。





## 案例教程

### 案例1 （使用select）

{{< tabs tabTotal="6" >}}
 

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
  .select('name')
  .limit(1)
  ```



{{% /tab %}}


{{% tab tabName="返回结果" %}}



  ```json
{
  "data": [
    {
      "name": "Afghanistan"
    }
  ],
  "status": 200,
  "statusText": "OK"
}
  ```



{{% /tab %}}

{{< /tabs >}}




### 案例2 （在外键表中）

{{< tabs tabTotal="6" >}}
 

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
  (1, 'United States');
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
  .limit(1, { foreignTable: 'cities' })
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
          "name": "Atlanta"
        }
      ]
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
      count
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>number类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

要返回的最大行数

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      选项（option）
    </span>
    <span className="method-list-item-label-badge required">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>object类型</code>
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
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

设置此选项以限制外键表的行数而不是当前的表

  </div>
  
</li>

</ul>

</li>

</ul>














