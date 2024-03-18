---
    weight: 1993
    title: "订阅数据库更改"
    icon: "article"
    draft: false
    toc: true
---
Supabase 允许您从客户端应用程序订阅数据库的实时更改。

您可以使用 [Postgres Changes](/docs/app/realtime/guides/postgres-changes) 扩展侦听数据库更改。
以下视频演示如何为表启用此功能。

## 例子

<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/2rUjcmgZDwQ"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>

## 设置

You'll first need to create a `supabase_realtime` publication and add your tables (that you want to subscribe to) to the publication:

```sql
begin;

-- remove the supabase_realtime publication
drop
  publication if exists supabase_realtime;

-- re-create the supabase_realtime publication with no tables
create publication supabase_realtime;

commit;

-- add a table called 'messages' to the publication
-- (update this to match your tables)
alter
  publication supabase_realtime add table messages;
```

## 流式插入

可以使用 INSERT 事件对所有新行进行流式处理。

```js
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_KEY)

const channel = supabase
  .channel('schema-db-changes')
  .on(
    'postgres_changes',
    {
      event: 'INSERT',
      schema: 'public',
    },
    (payload) => console.log(payload)
  )
  .subscribe()
```

## 流式处理更新

可以使用 UPDATE 事件流式传输所有更新的行。

```js
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_KEY)

const channel = supabase
  .channel('schema-db-changes')
  .on(
    'postgres_changes',
    {
      event: 'UPDATE',
      schema: 'public',
    },
    (payload) => console.log(payload)
  )
  .subscribe()
```

## 更多资源

- 详细了解[Postgres Changes](/docs/app/guides/realtime/postgres-changes)  扩展。
- 客户端库:
  - [JavaScript](/docs/app/sdkdocs/javascript/realtime/subscribe)
  - [Flutter](/docs/app/sdkdocs/dart/realtime/stream)
  - [Python](https://supabase.com/docs/app/sdkdocs/python/subscribe)
  - [C#](https://supabase.com/docs/app/sdkdocs/csharp/subscribe)