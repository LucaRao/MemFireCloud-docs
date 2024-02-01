---
    weight: 17
    title: "refreshSession()"
    icon: "article"
    draft: false
    toc: true
---


refreshSession()用于获取新的会话，无论会话是否已过期，都将返回一个新的会话。该方法接受一个可选的当前会话作为参数。如果未传入当前会话，那么 refreshSession() 方法将尝试从 getSession() 方法中获取当前会话。
如果当前会话的刷新令牌无效，将抛出一个错误。

* 该方法将刷新并返回一个新的会话，无论当前会话是否已过期。



## 案例教程
### 案例1 （使用当前会话刷新会话）

{{< tabs tabTotal="2" >}}



{{% tab tabName="使用方法" %}}



  ```ts
const { data: { user, session }, error } = await supabase.auth.refreshSession()
  ```



{{% /tab %}}

{{< /tabs >}}


### 案例2 （使用传入的会话刷新会话）

{{< tabs tabTotal="2" >}}



{{% tab tabName="使用方法" %}}



  ```ts
const { data: { user, session }, error } = await supabase.auth.refreshSession({ refresh_token })
  ```



{{% /tab %}}

{{< /tabs >}}





## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      currentSession
    </span>
    <span className="method-list-item-label-badge false">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>object类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

当前会话的信息。如果传入了这个参数，则必须包含一个刷新令牌。

  </div>
  
<ul className="method-list-group">
  <h5 class="method-list-title method-list-title-isChild expanded">特性</h5>

<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      刷新令牌（refresh_token）
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

</ul>
