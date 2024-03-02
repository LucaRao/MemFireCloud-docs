---
    weight: 117
    title: "like()"
    icon: "article"
    draft: false
    toc: true
---

like()用于查找所有在所述`列（column）`上的值与提供的 `模板（pattern）`相符的记录（区分大小写）。



## 案例教程
### 案例1 (使用select)

{{< tabs tabTotal="2" >}}
 
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
  .like('name', '%Alba%')
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
      模式（pattern）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

与之匹配的模式

  </div>
  
</li>

</ul>
