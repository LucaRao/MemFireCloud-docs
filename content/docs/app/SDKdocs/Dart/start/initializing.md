---
    weight: 394
    title: "初始化"
    icon: "article"
    draft: false
    toc: true
---




在使用 Supabase 的功能之前，需要先对 Supabase 进行初始化。这可以通过调用 Supabase 类的静态方法 initialize() 来实现。这个方法可能需要提供一些配置参数，以便正确地连接到 Supabase 服务。

一旦完成初始化，Supabase 客户端就是您与 Supabase 服务进行交互的主要接口。通过客户端，您可以访问 Supabase 提供的各种功能和服务。这是与 Supabase 生态系统内的其他组件交互的最简单且主要的方式，使您能够轻松地使用 Supabase 提供的各种功能。



## 案例教程

### 案例1 （Flutter项目）


{{< tabs tabTotal="2" >}}


{{% tab tabName="使用方法" %}}



  ```dart
                                                                              
Future<void> main() async {
  await Supabase.initialize(
    url: 'https://xyzcompany.supabase.co',
    anonKey: 'public-anon-key',
  );

  runApp(MyApp());
}

// Get a reference your Supabase client
final supabase = Supabase.instance.client;
  ```



{{% /tab %}}

{{< /tabs >}}



### 案例2 （其他Dart项目）

{{< tabs tabTotal="2" >}}


{{% tab tabName="使用方法" %}}



  ```dart
                                                                              
final supabase = SupabaseClient(
  'https://xyzcompany.supabase.co',
  'public-anon-key',
);
  ```



{{% /tab %}}

{{< /tabs >}}