---
    weight: 2378
    title: "maybeSingle()"
    icon: "article"
    draft: false
    toc: true
---


将`数据(data)`作为单个对象返回，而不是返回一个对象数组。



## 案例教程
### 案例1 

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
  .eq('name', 'Singapore')
  .maybeSingle()
  ```



{{% /tab %}}


{{% tab tabName="返回结果" %}}



  ```json
{
  "status": 200,
  "statusText": "OK"
}
  ```



{{% /tab %}}

{{< /tabs >}}
