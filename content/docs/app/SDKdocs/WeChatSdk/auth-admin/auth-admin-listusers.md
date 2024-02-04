---
    weight: 17
    title: "listUsers()"
    icon: "article"
    draft: false
    toc: true
---

listUsers()用于获取用户列表，默认每页返回50个用户。


## 案例教程

### 案例1 （获取用户的一页数据）

{{< tabs tabTotal="2" >}}


{{% tab tabName="使用方法" %}}



  ```ts
                                                                                   
const { data: { users }, error } = await supabase.auth.admin.listUsers()
  ```



{{% /tab %}}

{{< /tabs >}}

### 案例2 （用户的分页列表）

{{< tabs tabTotal="2" >}}


{{% tab tabName="使用方法" %}}



  ```ts
                                                                                   
const { data: { users }, error } = await supabase.auth.admin.listUsers({
  page: 1,
  perPage: 1000
})
  ```



{{% /tab %}}

{{< /tabs >}}







## 参数说明

<ul className="method-list-group">
  

<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      params
    </span>
    <span className="method-list-item-label-badge required">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>PageParams</code>
    </span>
  </h4>
<div class="method-list-item-description">

一个对象，支持 `page` 和 `perPage` 作为数字，用于更改分页结果。

  </div>
  
<ul className="method-list-group">
  <h5 class="method-list-title method-list-title-isChild expanded">特性</h5>

<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      page
    </span>
    <span className="method-list-item-label-badge required">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>数字类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

页数

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      perPage
    </span>
    <span className="method-list-item-label-badge required">
      [可选参数]
    </span>
    <span className="method-list-item-validation">
      <code>数字类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

每一页返回项目的个数

  </div>
  
</li>





</ul>

</li>

</ul>