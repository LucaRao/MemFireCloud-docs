---
    weight: 24
    title: "存储程序: rpc()"
    icon: "article"
    draft: false
    toc: true
---

你可以用"远程程序调用"的方式调用存储程序。

这是一种高级的说法，即你可以把一些逻辑放入数据库，然后从任何地方调用它。
这在逻辑很少更改的情况下特别有用，比如密码重置和更新等情况。




## 案例教程
### 案例1 (调用一个没有参数的存储程序)

{{< tabs tabTotal="3" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .rpc('hello_world');
```


{{% /tab %}}
{{% tab tabName="注意事项" %}}



这是一个调用存储过程的示例。



{{% /tab %}}

{{< /tabs >}}

### 案例2 (调用一个带参数的存储程序)

{{< tabs tabTotal="3" >}}

  
  
  
  
>

{{% tab tabName="使用方法" %}}



```dart
                                                                              
final data = await supabase
  .rpc('echo_city', params: { 'name': 'The Shire' });
```


{{% /tab %}}


{{< /tabs >}}
