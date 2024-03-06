---
    weight: 2379
    title: "csv()"
    icon: "article"
    draft: false
    toc: true
---


以 CSV 格式将`数据（data）`作为字符串返回。

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
  .csv()
  ```



{{% /tab %}}


{{% tab tabName="返回结果" %}}



  ```json
{
  "data": "id,name\n1,Afghanistan\n2,Albania\n3,Algeria",
  "status": 200,
  "statusText": "OK"
}
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



默认情况下，数据以 JSON 格式返回，但也可以选择返回 CSV 格式。



{{% /tab %}}

{{< /tabs >}}
