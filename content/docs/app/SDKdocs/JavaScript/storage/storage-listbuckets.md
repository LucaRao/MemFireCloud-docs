---
    weight: 258
    title: "listBuckets()"
    icon: "article"
    draft: false
    toc: true
---

listBuckets()用于获取现有项目中所有存储桶的详细信息。

需要RLS策略权限:
  - `buckets`表的权限: `select`
  - `objects`表的权限：无

请参考[存储指南](/docs/app/development_guide/storage/storage#access-control)中关于访问控制的工作方式。


## 案例教程

### 案例1 （获取所有存储桶信息）

{{< tabs tabTotal="1" >}}


{{% tab tabName="使用方法" %}}



  ```ts
const { data, error } = await supabase                                      
  .storage
  .listBuckets()
  ```



{{% /tab %}}

{{< /tabs >}}
