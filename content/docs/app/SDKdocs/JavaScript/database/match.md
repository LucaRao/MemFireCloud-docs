---
    weight: 95
    title: "match()"
    icon: "article"
    draft: false
    toc: true
---


仅匹配每个`查询(query)`键中的列与其关联值相等的行。相当于多个 `.eq()` 的简写。





## 案例教程 

### 案例1 

{{< tabs tabTotal="3" >}}

  
  
  
  defaultActiveId="demo1"
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
  .select('name')
  .match({ id: 2, name: 'Albania' })
```


{{% /tab %}}

{{% tab tabName="返回结果" %}}



```json
{
  "data": [
    {
      "name": "Albania"
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
      查询（query）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>Record类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用于筛选的对象，其中列名作为键映射到它们的筛选值。

  </div>
  
</li>

</ul>