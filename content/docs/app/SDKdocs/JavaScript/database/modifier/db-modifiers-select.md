---
    weight: 2372
    title: "select()"
    icon: "article"
    draft: false
    toc: true
---

对查询结果执行SELECT。

默认情况下，`.insert()`, `.update()`, `.upsert()`, 和 `.delete()`不会返回修改过的记录。通过调用这个方法，修改过的行会返回到`data`。

```ts
const { data, error } = await supabase
  .from('countries')
  .upsert({ id: 1, name: 'Algeria' })
  .select()
```


## 案例教程 

### 案例1 

{{< tabs tabTotal="3" >}}

  
  
  
  
>

{{% tab tabName="建表" %}}



```sql
create table
  countries (id int8 primary key, name text);

insert into
  countries (id, name)
values
  (1, 'Afghanistan');

```



{{% /tab %}}

{{% tab tabName="使用方法" %}}



```ts
const { data, error } = await supabase
  .from('countries')
  .upsert({ id: 1, name: 'Algeria' })
  .select()
```


{{% /tab %}}

{{% tab tabName="返回结果" %}}



```json
{
  "data": [
    {
      "id": 1,
      "name": "Algeria"
    }
  ],
  "status": 201,
  "statusText": "Created"
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
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>query类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

要检索的列，用逗号分隔

  </div>
  
</li>

</ul>
