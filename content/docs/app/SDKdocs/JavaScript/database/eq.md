---
    weight: 136
    title: "eq()"
    icon: "article"
    draft: false
    toc: true
---

eq()用于匹配`列`值等于`指定值`的行。

## 案例教程

### 案例1
{{< tabs tabTotal="4" >}}
 
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
    .select()
    .eq('name', 'Albania')
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
                                                                                
  {
    "data": [
      {
        "id": 2,
        "name": "Albania"
      }
    ],
    "status": 200,
    "statusText": "OK"
  }
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



要检查`column`的值是否为NULL，你应该使用`.is()`来代替。



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
      <code>任意类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用来过滤的值

  </div>
  
</li>

</ul>
