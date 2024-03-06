---
    weight: 338
    title: "is()"
    icon: "article"
    draft: false
    toc: true
---

仅匹配`列`值与`指定值`相等的行。



对于非boolean型列，这只与检查`column`的值是否为NULL有关。
`column`的值是NULL，通过设置`value`为`null`。

对于boolean型列，你也可以将`value`设置为`true`或`false`，它的行为与
它的行为与`.eq()`相同。


## 案例教程

### 案例1  (检查是否为 null)

{{< tabs tabTotal="4" >}}
 
{{% tab tabName="建表" %}}



  ```sql
                                                                              
create table
  countries (id int8 primary key, name text);

insert into
  countries (id, name)
values
  (1, 'null'),
  (2, null);
  ```



{{% /tab %}}
{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase
  .from('countries')
  .select()
  .is('name', null)
  ```



{{% /tab %}}

{{% tab tabName="返回结果" %}}



  ```ts
{
  "data": [
    {
      "id": 1,
      "name": "null"
    }
  ],
  "status": 200,
  "statusText": "OK"
}

  ```



{{% /tab %}}


{{% tab tabName="注意事项" %}}



使用`eq()`筛选器在过滤`null`时不起作用。相反，您需要使用`is()`。



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

要过滤的列

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
      <code>null或者boolean类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用来过滤的值

  </div>
  
</li>

</ul>
