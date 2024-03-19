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
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(supabase_url, anon_key)
  ```

### 案例2  （创建认证客户端 （服务器端））

  ```ts
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(supabase_url, anon_key, {
  auth: {
    autoRefreshToken: false,
    persistSession: false,
    detectSessionInUrl: false
  }
})
  ```
