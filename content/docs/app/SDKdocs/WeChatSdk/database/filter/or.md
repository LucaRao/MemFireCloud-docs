---
    weight: 2753
    title: "or()"
    icon: "article"
    draft: false
    toc: true
---

仅匹配满足至少一个过滤条件的行。

or() 期望您使用原始的 [PostgREST语法](https://postgrest.org/en/stable/api.html#operators) 来指定过滤器的名称和值。



```ts
.or('id.in.(5,6,7), arraycol.cs.{"a","b"}')  // Use `()` for `in` filter, `{}` for array values and `cs` for `contains()`.
.or('id.in.(5,6,7), arraycol.cd.{"a","b"}')  // Use `cd` for `containedBy()`
```

## 案例教程 

### 案例1  （和select一起使用）

{{< tabs tabTotal="8" >}}

  
  
  
  
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
  .or('id.eq.2,name.eq.Algeria')
```



{{% /tab %}}

{{% tab tabName="返回结果" %}}



```json
{
  "data": [
    {
      "name": "Albania"
    },
    {
      "name": "Algeria"
    }
  ],
  "status": 200,
  "statusText": "OK"
}
```


{{% /tab %}}

{{< /tabs >}}


### 案例2  （与and一起使用or）

{{< tabs tabTotal="8" >}}

  
  
  
  
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
  .or('id.gt.3,and(id.eq.1,name.eq.Afghanistan)')
```



{{% /tab %}}

{{< /tabs >}}


### 案例3  （在外部表上使用or）

{{< tabs tabTotal="8" >}}

  
  
  
  
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



```ts
const { data, error } = await supabase
  .from('countries')
  .select(`
    name,
    cities!inner (
      name
    )
  `)
  .or('country_id.eq.1,name.eq.Beijing', { foreignTable: 'cities' })
```



{{% /tab %}}

{{% tab tabName="返回结果" %}}



```json
{
  "data": [
    {
      "name": "Germany",
      "cities": [
        {
          "name": "Munich"
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
      过滤器（filters）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

使用的过滤器，遵循PostgREST的语法

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      外部表（foreignTable）
    </span>
    <span className="method-list-item-label-badge required">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>object类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

设置为过滤外域表而不是当前表

  </div>
  
<ul className="method-list-group">
  <h5 class="method-list-title method-list-title-isChild expanded">特性</h5>

<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      外部表（foreignTable）
    </span>
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>

  
</li>

</ul>

</li>

</ul>

