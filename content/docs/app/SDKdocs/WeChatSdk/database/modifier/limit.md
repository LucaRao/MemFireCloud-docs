---
    weight: 306
    title: "limit()"
    icon: "article"
    draft: false
    toc: true
---


通过`count`限制查询结果。


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
  .select('name')
  .limit(1)
  ```


{{% /tab %}}


{{% tab tabName="返回结果" %}}

  ```json
{
  "data": [
    {
      "name": "Afghanistan"
    }
  ],
  "status": 200,
  "statusText": "OK"
}
  ```


{{% /tab %}}

{{< /tabs >}}



### 案例2 （在外键表中）

{{< tabs tabTotal="2" >}}
{{% tab tabName="建表" %}}


  ```sql
create table
  countries (id int8 primary key, name text);
create table
  cities (
    id int8 primary key,
    country_id int8 not null references countries,
    name text
  );

insert into
  countries (id, name)
values
  (1, 'United States');
insert into
  cities (id, country_id, name)
values
  (1, 1, 'Atlanta'),
  (2, 1, 'New York City');
  ```

{{% /tab %}}

{{% tab tabName="使用方法" %}}

  ```ts
const { data, error } = await supabase
  .from('countries')
  .select(`
    name,
    cities (
      name
    )
  `)
  .limit(1, { foreignTable: 'cities' })
  ```

{{% /tab %}}


{{% tab tabName="返回结果" %}}

  ```json
{
  "data": [
    {
      "name": "United States",
      "cities": [
        {
          "name": "Atlanta"
        }
      ]
    }
  ],
  "status": 200,
  "statusText": "OK"
}
  ```


{{% /tab %}}

{{< /tabs >}}