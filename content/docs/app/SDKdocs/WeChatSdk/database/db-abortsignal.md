---
    weight: 50
    title: "abortSignal()"
    icon: "article"
    draft: false
    toc: true
---

设置获取请求的AbortSignal。



## 案例教程
### 案例1 

{{< tabs tabTotal="3" >}}


{{% tab tabName="使用方法" %}}



  ```ts
const ac = new AbortController()
ac.abort()
                                                                              
const { data, error } = await supabase
  .from('very_big_table')
  .select()
  .abortSignal(ac.signal)
  ```



{{% /tab %}}


{{% tab tabName="返回结果" %}}



  ```json
  {
    "error": {
      "message": "FetchError: The user aborted a request.",
      "details": "",
      "hint": "",
      "code": ""
                                                                                    
    },
    "status": 400,
    "statusText": "Bad Request"
  }
  ```



{{% /tab %}}

{{% tab tabName="注意事项" %}}



你可以使用 [AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) 来中止请求。
请注意，对于被中止的请求，`状态 (status)` 和`状态文本 (statusText)` 并不具有实际意义，因为请求未能完成。



{{% /tab %}}



{{< /tabs >}}











## 参数说明


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      signal
    </span>
    <span className="method-list-item-label-badge required">
      [必要参数]
    </span>
    <span className="method-list-item-validation">
      <code>AbortSignal类型</code>
    </span>
  </h4>
  <div class="method-list-item-description">

用于获取请求的AbortSignal

  </div>
  
</li>

</ul>

