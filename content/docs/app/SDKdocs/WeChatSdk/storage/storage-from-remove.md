---
    weight: 2883
    title: "from.remove()"
    icon: "article"
    draft: false
    toc: true
---

from.remove()用于删除存储桶中的文件

需要RLS策略权限:
  - `buckets`表的权限: 无
  - `objects`表的权限：`delete`和`select`权限

请参考[存储指南](/docs/app/development_guide/storage/storage#access-control)中关于访问控制的工作方式。

## 案例教程

### 案例1 （删除文件）

{{< tabs tabTotal="1" >}}


{{% tab tabName="使用方法" %}}



  ```ts
                                                                                   
const { data, error } = await supabase
  .storage
  .from('avatars')
  .remove(['folder/avatar1.png'])
  ```



{{% /tab %}}

{{< /tabs >}}


## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      paths
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>string[]类型（字符串数组）</code>
    </span>
  </h4>
  <div class="method-list-item-description">

一个要删除的文件数组，包括路径和文件名。例如[`folder/image.png`]。

  </div>
  
</li>

</ul>





