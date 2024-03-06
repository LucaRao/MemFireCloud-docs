---
    weight: 2337
    title: "lte()"
    icon: "article"
    draft: false
    toc: true
---

lte()用于查找所有在所述`列（column）`上的值小于或等于指定`值（value）`的记录。


## 案例教程

### 案例1 (使用select)

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
  .select()
  .lte('id', 2)
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json

{
  "data": [
    {
      "id": 1,
      "name": "Afghanistan"
    },
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











