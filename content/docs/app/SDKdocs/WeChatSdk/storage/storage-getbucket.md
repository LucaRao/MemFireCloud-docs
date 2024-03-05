---
    weight: 373
    title: "getBucket()"
    icon: "article"
    draft: false
    toc: true
---


getBucket()用于获取现有存储桶的详细信息。

需要RLS策略权限:
  - `buckets`表的权限: `select`
  - `objects`表的权限：无

请参考[存储指南](/docs/app/storage/storage#access-control)中关于访问控制的工作方式。





## 案例教程

### 案例1 （获取存储桶信息）

{{< tabs tabTotal="1" >}}


{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase                                      
  .storage
  .getBucket('avatars')
  ```



{{% /tab %}}

{{< /tabs >}}








## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      id
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

这是你创建存储桶的唯一标识符。

  </div>
  
</li>

</ul>