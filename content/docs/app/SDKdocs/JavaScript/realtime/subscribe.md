---
    weight: 261
    title: "on().subscribe()"
    icon: "article"
    draft: false
    toc: true
---


on().subscribe()创建一个事件处理程序，用于监听变更。


* 默认情况下，广播（Broadcast）和在线状态（Presence）对所有项目都是启用的。
* 对于新项目，默认情况下禁用了监听数据库变更，原因是出于数据库性能和安全方面的考虑。你可以通过管理实时数据的[复制功能](/docs/app/api/api#managing-realtime)来启用它。
* 你可以通过将表的 `REPLICA IDENTITY` 设置为 `FULL`（例如，执行 `ALTER TABLE your_table REPLICA IDENTITY FULL`;），来接收更新和删除操作的"之前"的数据。
* 删除语句(delete statements)不适用行级安全（Row level security）。当启用行级安全并将复制标识（replica identity）设置为 full 时，只有主键会被发送到客户端。



## 案例教程

### 案例1 （监听广播消息）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('broadcast', { event: 'cursor-pos' }, payload => {
    console.log('Cursor position received!', payload)
  })
  .subscribe((status) => {
    if (status === 'SUBSCRIBED') {
      channel.send({
        type: 'broadcast',
        event: 'cursor-pos',
        payload: { x: Math.random(), y: Math.random() },
      })
    }
  })
  ```



{{% /tab %}}

{{< /tabs >}}


### 案例2 （监听在线状态同步）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
const channel = supabase.channel('any')
channel
  .on('presence', { event: 'sync' }, () => {
    console.log('Synced presence state: ', channel.presenceState())
  })
  .subscribe(async (status) => {
    if (status === 'SUBSCRIBED') {
      await channel.track({ online_at: new Date().toISOString() })
    }
  })
  ```



{{% /tab %}}

{{< /tabs >}}

### 案例3 （监听用户加入状态）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
const channel = supabase.channel('any')
channel
  .on('presence', { event: 'join' }, ({ newPresences }) => {
    console.log('Newly joined presences: ', newPresences)
  })
  .subscribe(async (status) => {
    if (status === 'SUBSCRIBED') {
      await channel.track({ online_at: new Date().toISOString() })
    }
  })
  ```



{{% /tab %}}

{{< /tabs >}}


### 案例4 （监听用户离开状态）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
const channel = supabase.channel('any')
channel
  .on('presence', { event: 'leave' }, ({ leftPresences }) => {
    console.log('Newly left presences: ', leftPresences)
  })
  .subscribe(async (status) => {
    if (status === 'SUBSCRIBED') {
      await channel.track({ online_at: new Date().toISOString() })
      await channel.untrack()
    }
  })
  ```



{{% /tab %}}

{{< /tabs >}}

### 案例5 （监听所有数据库变更）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('postgres_changes', { event: '*', schema: '*' }, payload => {
    console.log('Change received!', payload)
  })
  .subscribe()
  ```



{{% /tab %}}

{{< /tabs >}}


### 案例6 （监听特定表格）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('postgres_changes', { event: '*', schema: 'public', table: 'countries' }, payload => {
    console.log('Change received!', payload)
  })
  .subscribe()
  ```



{{% /tab %}}

{{< /tabs >}}

### 案例7 （监听插入操作）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('postgres_changes', { event: 'INSERT', schema: 'public', table: 'countries' }, payload => {
    console.log('Change received!', payload)
  })
  .subscribe()
  ```



{{% /tab %}}

{{< /tabs >}}


### 案例8 （监听更新操作）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('postgres_changes', { event: 'UPDATE', schema: 'public', table: 'countries' }, payload => {
    console.log('Change received!', payload)
  })
  .subscribe()
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



默认情况下，MemFire Cloud只会发送更新后的记录。如果你想要同时接收之前的值，你可以为你正在监听的表启用完整的复制（full replication）：

alter table "your_table" replica identity full;



{{% /tab %}}


{{< /tabs >}}


### 案例9 （监听删除操作）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('postgres_changes', { event: 'DELETE', schema: 'public', table: 'countries' }, payload => {
    console.log('Change received!', payload)
  })
  .subscribe()
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



默认情况下，MemFire Cloud不会发送已删除的记录。如果你想要接收已删除的记录，你可以为你正在监听的表启用完整的复制（full replication）：

alter table "your_table" replica identity full;



{{% /tab %}}


{{< /tabs >}}


### 案例10 （监听多个事件）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('postgres_changes', { event: 'INSERT', schema: 'public', table: 'countries' }, handleRecordInserted)
  .on('postgres_changes', { event: 'DELETE', schema: 'public', table: 'countries' }, handleRecordDeleted)
  .subscribe()
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



如果你想要监听同一张表的多个事件，你可以进行链式监听（chain listeners）。



{{% /tab %}}


{{< /tabs >}}


### 案例11 （监听行级别变更）

{{< tabs tabTotal="15" >}}


{{% tab tabName="使用方法" %}}



  ```ts
supabase
  .channel('any')
  .on('postgres_changes', { event: 'UPDATE', schema: 'public', table: 'countries', filter: 'id=eq.200' }, handleRecordUpdated)
  .subscribe()
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



你可以使用格式 `{table}:{col}=eq.{val}` 来监听单个行，其中 `{col}` 是列名，`{val}` 是你要匹配的值。



{{% /tab %}}

{{< /tabs >}}









## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      类型（type）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>类型为 "broadcast" 的字符串</code>
    </span>
  </h4>
  <div class="method-list-item-description">

可选值为 "broadcast"、"presence" 或 "postgres_changes"。

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      filter
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>object类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

针对实时功能（realtime功能）的自定义对象，用于详细说明要接收哪些有效载荷（payloads）。

  </div>
  
<ul className="method-list-group">
  <h5 class="method-list-title method-list-title-isChild expanded">特性</h5>

<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      event
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>string类型</code>
    </span>
  </h4>
  
</li>

</ul>

</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      callback
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>函数类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

当事件处理程序被触发时要调用的函数。

  </div>
  
</li>

</ul>