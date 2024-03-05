---
    weight: 205
    title: "Insert 数据"
    icon: "article"
    draft: false
    toc: true
---

在表（table）或视图（view）执行 INSERT 操作。



## 案例教程

### 案例1 （创建一个记录）

{{< tabs tabTotal="10" >}}
 

{{% tab tabName="建表" %}}



  ```sql
create table
  countries (id int8 primary key, name text);
  ```



{{% /tab %}}

{{% tab tabName="使用方法" %}}



  ```ts
const { error } = await supabase
  .from('countries')
  .insert({ id: 1, name: 'Denmark' })
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
{
  "status": 201,
  "statusText": "Created"
}
  ```


{{% /tab %}}

{{< /tabs >}}



### 案例2 （创建一个记录并返回）

{{< tabs tabTotal="10" >}}
 

{{% tab tabName="建表" %}}



  ```sql
create table
  countries (id int8 primary key, name text);

  ```



{{% /tab %}}

{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase
  .from('countries')
  .insert({ id: 1, name: 'Denmark' })
  .select()
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
{
  "data": [
    {
      "id": 1,
      "name": "Denmark"
    }
  ],
  "status": 201,
  "statusText": "Created"
}
  ```


{{% /tab %}}

{{< /tabs >}}


### 案例3 （批量创建）

{{< tabs tabTotal="10" >}}
 

{{% tab tabName="建表" %}}



  ```sql
create table
  countries (id int8 primary key, name text);
  ```



{{% /tab %}}

{{% tab tabName="使用方法" %}}



  ```ts
const { error } = await supabase
  .from('countries')
  .insert([
    { id: 1, name: 'Nepal' },
    { id: 1, name: 'Vietnam' },
  ])
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
{
  "error": {
    "code": "23505",
    "details": "Key (id)=(1) already exists.",
    "hint": null,
    "message": "duplicate key value violates unique constraint \"countries_pkey\""
  },
  "status": 409,
  "statusText": "Conflict"
}
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



批量创建操作在单个事务中进行处理。如果其中任何一条插入失败，所有的行都不会被插入。
  


{{% /tab %}}

{{< /tabs >}}



## 参数说明


<ul className="method-list-group">
  
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

要插入的值。传递一个对象来插入单一行或一个数组来插入多行。

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
      count
    </span>
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>exact</code> | <code>planned</code> | <code>estimated</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用来计算插入行的计数算法。

exact:可以精确计算行数，但执行速度较慢。执行 COUNT(*) 操作。

planned:可以快速计算行数，但是结果可能略有偏差。使用了Postgres
的统计数据。

estimated:对于较小的数值使用精确计数，对于较大的数值使用计划计数。根据行数的大小决定使用精确计数或计划计数的算法。

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      defaultToNull
    </span>
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>boolean类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

将缺失的字段设置为null。否则使用列的默认值。

  </div>
  
</li>



</ul>

</li>

</ul>














