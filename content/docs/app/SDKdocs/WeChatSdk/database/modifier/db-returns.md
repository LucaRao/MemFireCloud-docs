---
    weight: 306
    title: "returns()"
    icon: "article"
    draft: false
    toc: true
---

覆盖返回`数据（data）`的类型。

## 案例教程
### 案例1  (覆盖成功响应的类型)

{{< tabs tabTotal="3" >}}
{{% tab tabName="使用方法" %}}
  ```ts
const { data } = await supabase
  .from('countries')
  .select()
  .returns<MyType>()
  ```

{{% /tab %}}


{{% tab tabName="返回结果" %}}

```json
let x: typeof data // MyType | null
```

{{% /tab %}}

{{< /tabs >}}