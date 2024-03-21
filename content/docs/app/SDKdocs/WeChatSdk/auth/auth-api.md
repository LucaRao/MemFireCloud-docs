---
    weight: 2391
    title: "概述"
    icon: "article"
    draft: false
    toc: true
---


认证方法可以通过 `supabase.auth` 命名空间访问。



## 案例教程
### 案例1  (创建认证客户端)


  ```ts
import { createClient } from 'supabase-wechat-stable-v2'

const supabase = createClient(supabase_url, anon_key)
  ```

### 案例2  （创建认证客户端 （服务器端））

  ```ts
import { createClient } from 'supabase-wechat-stable-v2'

const supabase = createClient(supabase_url, anon_key, {
  auth: {
    autoRefreshToken: false,
    persistSession: false,
    detectSessionInUrl: false
  }
})
  ```

## 入门指南
<div class="row flex-xl-wrap pb-4">
<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/auth/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">概述</p>
        <p class="para card-text mb-0"> 每个认证系统都有认证和授权两个部分</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/redirect-urls/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">重定向 URL</p>
        <p class="para card-text mb-0"> 使用无密码登录或第三方提供程序时，MemFire Cloud 客户端库方法提供 redirectTo 参数，以指定身份验证后将用户重定向到的位置</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/native-mobile-deep-linking/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">原生移动端深度链接</p>
        <p class="para card-text mb-0"> 在某些身份验证情况下，您需要处理链接回应用程序以完成用户登录</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/auth-captcha/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">启用Captcha保护</p>
        <p class="para card-text mb-0"> Supabase为您提供了在登录、注册和密码重置表单中添加captcha的选项</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/rate-limiting/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">速率限制</p>
        <p class="para card-text mb-0"> 速率限制、资源分配和滥用预防</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/authentication/auth-email/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">身份验证</p>
        <p class="para card-text mb-0"> 提供邮件、短信、微信等多种认证方式</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/mandates/row-level-security/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">授权</p>
        <p class="para card-text mb-0"> 行级别安全性</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/auth-helpers/auth-helpers/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">认证帮助程序</p>
        <p class="para card-text mb-0"> 用于使用Supabase的特定于框架的Auth实用程序的集合</p>
      </div>
    </div>
  </a>
</div>

<div id="list-item" class="col-md-4 col-12 py-2">
  <a class="text-decoration-none text-reset" href="/docs/app/development_guide/auth/auth-deep-dive/auth-deep-dive-jwts/">
  <div class="card h-100 features feature-full-bg rounded p-4 position-relative overflow-hidden border-1">
      <!-- <span class="h1 icon-color">
        <i class="material-icons align-middle">highlight</i>
      </span> -->
      <div class="card-body p-0 content">
        <p class="fs-5  card-title mb-1">深层探索</p>
        <p class="para card-text mb-0"> JWTS、行级安全、GoTrue、Google Oauth</p>
      </div>
    </div>
  </a>
</div>
</div>