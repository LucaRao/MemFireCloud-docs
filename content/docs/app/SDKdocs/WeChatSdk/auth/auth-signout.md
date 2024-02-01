---
    weight: 83
    title: "signOut()"
    icon: "article"
    draft: false
    toc: true
---


在浏览器中，signOut() 方法将会从浏览器会话中移除已登录的用户，并让他们退出登陆。

* 清除所有本地存储项，然后触发一个 `SIGNED_OUT`事件。
* 为了使用 signOut() 方法，用户首先需要完成登录操作。



## 案例教程
### 案例1 

{{< tabs tabTotal="1" >}}



{{% tab tabName="使用方法" %}}



  ```ts
const { error } = await supabase.auth.signOut()
  ```



{{% /tab %}}

{{< /tabs >}}



