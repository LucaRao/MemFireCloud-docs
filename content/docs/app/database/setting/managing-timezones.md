---
    weight: 1564
    title: "时区"
    description: "How to change your database timezone."
    icon: "article"
    draft: false
    toc: true
---

每个 Supabase 数据库都默认设置为 UTC 时区。我们强烈建议保持这种方式，即使你的用户在不同的地方。
这是因为，如果你采用 "我的数据库中的一切都在UTC时间 "的心理模式，那么计算不同时区的差异就会容易得多。

### 改变时区

{{< tabs tabTotal="3" >}}

  
  
  
  
{{% tab tabName="SQL" %}}



```sql
alter database postgres
set timezone to 'America/New_York';
```



{{% /tab %}}

{{< /tabs >}}

### 时区的完整列表

获取你的数据库所支持的时区的完整列表。这将返回以下列：

- `name`: 时区名称
- `abbrev`: 时区缩略语
- `utc_offset`: 与UTC的偏移（正数表示格林威治以东）。
- `is_dst`: 如果目前遵守夏令时，则为真

{{< tabs tabTotal="3" >}}

  
  
  
  
{{% tab tabName="SQL" %}}



```sql
select name, abbrev, utc_offset, is_dst
from pg_timezone_names()
order by name;
```



{{% /tab %}}

{{< /tabs >}}

### 搜索一个特定的时区

使用`ilike`（不区分大小写的搜索）来寻找特定的时区。

{{< tabs tabTotal="3" >}}

  
  
  
  
{{% tab tabName="SQL" %}}



```sql
select *
from pg_timezone_names()
where name ilike '%york%';
```



{{% /tab %}}

{{< /tabs >}}


