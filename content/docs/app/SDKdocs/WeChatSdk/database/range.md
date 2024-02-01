---
    weight: 30
    title: "range()"
    icon: "article"
    draft: false
    toc: true
---

通过 `from`和 `to`来限制查询结果。





## 案例教程
### 案例1 

{{< tabs tabTotal="3" >}}
 

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
  .range(0, 1)
  ```



{{% /tab %}}


{{% tab tabName="返回结果" %}}



  ```json
{
  "data": [
    {
      "name": "Afghanistan"
    },
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
      from
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>number类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用于限制结果的起始索引

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      to
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>number类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

限制结果的最后一个索引

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

