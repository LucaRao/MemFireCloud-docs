---
    weight: 106
    title: "Delete 数据"
    icon: "article"
    draft: false
    toc: true
---

delete()用于在表（table）或视图（view）执行 DELETE 操作。

* `delete()` 应始终与[过滤器（filter）](/docs/app/SDKdocs/JavaScript/database/using-filters)结合使用，以便定位要删除的项。
* 如果你在使用 `delete()` 时带有过滤器，并且启用了RLS（行级安全性），则只会删除通过  `SELECT` 策略可见的行。请注意，默认情况下没有行可见，因此你需要至少有一个 `SELECT`/`ALL` 策略来使行可见。


## 案例教程

### 案例1 （删除记录）

{{< tabs tabTotal="3" >}}
 

{{% tab tabName="建表" %}}



  ```sql
create table
  countries (id int8 primary key, name text);

insert into
  countries (id, name)
values
  (1, 'Spain');
  ```



{{% /tab %}}

{{% tab tabName="使用方法" %}}



  ```ts
const { error } = await supabase
  .from('countries')
  .delete()
  .eq('id', 1)
  ```



{{% /tab %}}
{{% tab tabName="返回结果" %}}



  ```json
{
  "status": 204,
  "statusText": "No Content"
}
  ```


{{% /tab %}}

{{< /tabs >}}





## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      选项（option）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
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
      optional
    </span>
    <span className="method-list-item-validation">
      <code>exact</code> | <code>planned</code> | <code>estimated</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用来计算更新行的计数算法。

exact:可以精确计算行数，但执行速度较慢。执行 "COUNT(*)"操作。

planned:可以快速计算行数，但是结果可能略有偏差。使用了Postgres的统计数据。

estimated:对于较小的数值使用精确计数，对于较大的数值使用计划计数。根据行数的大小决定使用精确计数或计划计数的算法。

  </div>
  
</li>

</ul>

</li>

</ul>
