---
    weight: 1802
    title: "实时快速入门"
    description: ""
    icon: "article"
    draft: false
    toc: true
---


学习如何构建[multiplayer.dev](https://multiplayer.dev)，这是一个合作应用，它展示了使用[实时](/docs/app/development_guide/realtime/realtime)的广播、Presence和Postgres CDC。

<div className="video-container">
  <iframe
    src="https://www.youtube-nocookie.com/embed/BelYEMJ2N00"
    frameBorder="1"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</div>

## 安装`supabase-js` 客户端

```bash
npm install @supabase/supabase-js
```

## 光标位置

[广播](/docs/app/development_guide/realtime/realtime#broadcast)允许一个客户端发送消息，多个客户端接收消息。广播的消息是短暂的。它们不会被持久化到数据库中，而是直接通过实时服务器转发。这对于发送像光标位置这样的信息是很理想的，因为最小的延迟是很重要的，但持久化则不是。

在[multiplayer.dev](https://multiplayer.dev)中，客户端的光标位置被发送到房间里的其他客户端。然而，在这个例子中，光标位置将是随机生成的。

你需要从你的项目的API设置中获得公共的`anon`访问令牌。然后你就可以设置Supabase客户端，并开始发送一个客户端的光标位置到通道`room1`中的其他客户端。

```js
const { createClient } = require('@supabase/supabase-js')

const supabase = createClient('https://your-project-ref.supabase.co', 'anon-key', {
  realtime: {
    params: {
      eventsPerSecond: 10,
    },
  },
})

// Channel name can be any string.
// Create channels with the same name for both the broadcasting and receiving clients.
const channel = supabase.channel('room1')

// Subscribe registers your client with the server
channel.subscribe((status) => {
  if (status === 'SUBSCRIBED') {
    // now you can start broadcasting cursor positions
    setInterval(() => {
      channel.send({
        type: 'broadcast',
        event: 'cursor-pos',
        payload: { x: Math.random(), y: Math.random() },
      })
      console.log(status)
    }, 100)
  }
})
```

{{% alert context="info" %}}
JavaScript客户端有一个默认的速率限制，即每100毫秒一个实时事件，由`eventsPerSecond`配置。
{{% /alert %}}

另一个客户端可以订阅频道`room1`并接收游标位置。

```js
// Supabase client setup

// Listen to broadcast messages.
supabase
  .channel('room1')
  .on('broadcast', { event: 'cursor-pos' }, (payload) => console.log(payload))
  .subscribe((status) => {
    if (status === 'SUBSCRIBED') {
      // your callback function will now be called with the messages broadcast by the other client
    }
  })
```

{{% alert context="info" %}}
`type` 必须是 `broadcast`，`event` 必须与订阅该频道的客户相匹配。
{{% /alert %}}

## 往返延时

你也可以配置通道，使服务器必须返回一个确认函，表明它收到了 "广播 "信息。如果你想测量往返的延迟，这很有用。

```js
// Supabase client setup

const channel = supabase.channel('calc-latency', {
  config: {
    broadcast: { ack: true },
  },
})

channel.subscribe(async (status) => {
  if (status === 'SUBSCRIBED') {
    const begin = performance.now()

    await channel.send({
      type: 'broadcast',
      event: 'latency',
      payload: {},
    })

    const end = performance.now()

    console.log(`Latency is ${end - begin} milliseconds`)
  }
})
```

## 跟踪和显示哪些用户在线

[Presence](/docs/app/development_guide/realtime/realtime#presence)存储和同步各客户端的共享状态。每当共享状态发生变化时，就会触发`sync`事件。`join`事件在新客户加入频道时被触发，`leave`事件在客户离开时被触发。

每个客户端可以使用通道的`track`方法来存储共享状态中的对象。每个客户端只能跟踪一个对象，如果`track`被同一个客户端再次调用，那么新的对象就会覆盖共享状态中先前跟踪的对象。你可以使用一个客户端来跟踪和显示在线的用户。


```js
// Supabase client setup

const channel = supabase.channel('online-users', {
  config: {
    presence: {
      key: 'user1',
    },
  },
})

channel.on('presence', { event: 'sync' }, () => {
  console.log('Online users: ', channel.presenceState())
})

channel.on('presence', { event: 'join' }, ({ newPresences }) => {
  console.log('New users have joined: ', newPresences)
})

channel.on('presence', { event: 'leave' }, ({ leftPresences }) => {
  console.log('Users have left: ', newPresences)
})

channel.subscribe(async (status) => {
  if (status === 'SUBSCRIBED') {
    const status = await channel.track({ online_at: new Date().toISOString() })
    console.log(status)
  }
})
```

然后，您可以使用其他客户端将其他用户添加到频道的存在状态：

```js
// Supabase client setup

const channel = supabase.channel('online-users', {
  config: {
    presence: {
      key: 'user2',
    },
  },
})

// Presence event handlers setup

channel.subscribe(async (status) => {
  if (status === 'SUBSCRIBED') {
    const status = await channel.track({ online_at: new Date().toISOString() })
    console.log(status)
  }
})
```

如果一个频道在没有存在密钥的情况下被设置，服务器会生成一个随机的UUID。`type`必须是`presence`，`event`必须是`sync`，`join`，或`leave`。


## 插入和接收持久的信息

[Postgres Change Data Capture (CDC)](/docs/app/development_guide/realtime/realtime#postgres-cdc)使你的客户能够插入、更新或删除数据库记录，并将这些变化发送给客户。创建一个`messages`表来跟踪用户在特定房间创建的消息。

```sql
create table messages (
  id serial primary key,
  message text,
  user_id text,
  room_id text,
  created_at timestamptz default now()
)

alter table messages enable row level security;

create policy "anon_ins_policy"
ON messages
for insert
to anon
with check (true);

create policy "anon_sel_policy"
ON messages
for select
to anon
using (true);
```

如果它还不存在，创建一个`supabase_realtime`发布，并将`messages`表添加到该出发布中。

```sql
begin;
  -- remove the supabase_realtime publication
  drop publication if exists supabase_realtime;

  -- re-create the supabase_realtime publication with no tables and only for insert
  create publication supabase_realtime with (publish = 'insert');
commit;

-- add a table to the publication
alter publication supabase_realtime add table messages;
```

然后你可以让客户端监听特定房间的`messages`表的变化，并发送和接收持久化的信息。

```js
// Supabase client setup

const channel = supabase.channel('db-messages')

const roomId = 'room1'
const userId = 'user1'

channel.on(
  'postgres_changes',
  {
    event: 'INSERT',
    schema: 'public',
    table: 'messages',
    filter: `room_id=eq.${roomId}`,
  },
  (payload) => console.log(payload)
)

channel.subscribe(async (status) => {
  if (status === 'SUBSCRIBED') {
    const res = await supabase.from('messages').insert({
      room_id: roomId,
      user_id: userId,
      message: 'Welcome to Realtime!',
    })
    console.log(res)
  }
})
```


