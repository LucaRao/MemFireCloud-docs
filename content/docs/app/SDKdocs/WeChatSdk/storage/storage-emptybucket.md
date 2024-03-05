---
    weight: 369
    title: "emptyBucket()"
    icon: "article"
    draft: false
    toc: true
---

emptyBucket()用于移除单个桶内的所有对象。



需要RLS策略权限:
  - `buckets`表的权限: `select`
  - `objects`表的权限：`select`和`delet `

请参考[存储指南](/docs/app/storage/storage#access-control)中关于访问控制的工作方式。



## 案例教程

### 案例1 （清空一个存储桶）

{{< tabs tabTotal="1" >}}


{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase
  .storage
  .emptyBucket('avatars')
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
