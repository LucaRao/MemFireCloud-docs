---
    weight: 289
    title: "onAuthStateChange()"
    icon: "article"
    draft: false
    toc: true
---


onAuthStateChange()用于监听认证事件，接收每次认证事件发生时的通知。

* 认证事件类型： `SIGNED_IN（登录）`、`SIGNED_OUT（登出）`、`TOKEN_REFRESHED（令牌刷新）`、`USER_UPDATED（用户信息更新）`、`PASSWORD_RECOVERY（密码恢复）`
* 然而目前使用 `onAuthStateChange()` 方法在不同的标签页之间无效。
举例来说，在密码重置流程中，如果用户在一个标签页请求密码重置链接，当用户点击链接时，原始的标签页将无法收到`SIGNED_IN（登录）`和`PASSWORD_RECOVERY（密码恢复）`事件。





## 案例教程

### 案例1 （监听认证变化）

{{< tabs tabTotal="6" >}}

{{% tab tabName="使用方法" %}}



  ```ts
supabase.auth.onAuthStateChange((event, session) => {
  console.log(event, session)
})
  ```


{{% /tab %}}

{{< /tabs >}}


### 案例2 （监听密码恢复事件）

{{< tabs tabTotal="6" >}}

{{% tab tabName="使用方法" %}}



  ```ts
supabase.auth.onAuthStateChange((event, session) => {
  if (event == 'PASSWORD_RECOVERY') {
    console.log('PASSWORD_RECOVERY', session)

    // show screen to update user's password
    showPasswordResetScreen(true)
  }
})
  ```


{{% /tab %}}

{{< /tabs >}}

### 案例3 （监听登录）

{{< tabs tabTotal="6" >}}

{{% tab tabName="使用方法" %}}



  ```ts
supabase.auth.onAuthStateChange((event, session) => {
  if (event == 'SIGNED_IN') console.log('SIGNED_IN', session)
})
  ```


{{% /tab %}}

{{< /tabs >}}

### 案例4 （监听退出登录）

{{< tabs tabTotal="6" >}}

{{% tab tabName="使用方法" %}}



  ```ts
supabase.auth.onAuthStateChange((event, session) => {
  if (event == 'SIGNED_OUT') console.log('SIGNED_OUT', session)
})
  ```


{{% /tab %}}

{{< /tabs >}}


### 案例5 （监听令牌刷新）

{{< tabs tabTotal="6" >}}

{{% tab tabName="使用方法" %}}



  ```ts
supabase.auth.onAuthStateChange((event, session) => {
  if (event == 'TOKEN_REFRESHED') console.log('TOKEN_REFRESHED', session)
})
  ```


{{% /tab %}}

{{< /tabs >}}


### 案例6 （监听用户的更新信息）

{{< tabs tabTotal="6" >}}

{{% tab tabName="使用方法" %}}



  ```ts
supabase.auth.onAuthStateChange((event, session) => {
  if (event == 'USER_UPDATED') console.log('USER_UPDATED', session)
})
  ```


{{% /tab %}}

{{< /tabs >}}






## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      回调（callback）
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>函数类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

当发生认证事件时要调用的回调函数。

  </div>
  
</li>

</ul>