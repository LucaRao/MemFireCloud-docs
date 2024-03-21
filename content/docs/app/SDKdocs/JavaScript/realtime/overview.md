---
    weight: 2320
    title: "概述"
    icon: "article"
    draft: false
    toc: true
---

## 实时数据库

MemFire Cloud提供一个全球分布的实时服务器集群，实现了以下功能：

- 广播：以低延迟的方式从客户端向客户端发送短暂的信息。
- Presence：跟踪和同步客户端之间的共享状态。
- Postgres CDC：听取Postgres数据库的变化，并将其发送给授权客户。


<div class="row flex-xl-wrap pb-4">

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/realtime/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">概述</p>
        <p class="para card-text mb-0"> 实时 API</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/quickstart/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">快速入门</p>
        <p class="para card-text mb-0"> 学习如何使用实时的广播、Presence和Postgres CDC。</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/postgres-cdc/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">Postgres CDC</p>
        <p class="para card-text mb-0"> 实时的Postgres变更数据捕获（CDC）功能监听数据库的变更，并将其发送给客户端</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/rate-limits/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">实时速率限制</p>
        <p class="para card-text mb-0"> 速率限制可按项目配置，我们的集群支持数百万的并发连接</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/concepts/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">实时概念</p>
        <p class="para card-text mb-0"> 使用Supabase Realtime来构建具有协作/多人游戏功能的实时应用程序</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/usage/broadcast/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">用法</p>
        <p class="para card-text mb-0"> 广播和Presence</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/guides/client-side-throttling/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">指南</p>
        <p class="para card-text mb-0"> 限制消息、Postgres Changes、将 Realtime 与 Next.js 结合使用、订阅</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/realtime/deep-dive/architecture/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">深入了解</p>
        <p class="para card-text mb-0"> 实时架构、自带数据库、实时协议、实时配额</p>
      </div>
    </div>
  </a>
</div>

</div>