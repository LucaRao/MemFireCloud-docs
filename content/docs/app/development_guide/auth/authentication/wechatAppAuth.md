---
    weight: 1413
    title: "微信移动应用授权登录"
    description: "微信扫码登录认证"
    icon: "article"
    draft: false
    toc: true
---


### 前言

为了顺应国内用户的使用习惯，MemFire Cloud认证服务已集成微信移动应用授权登录功能。微信，作为国内广受欢迎的社交平台，其微信移动应用授权登录功能以其便捷性和快速性赢得了广大用户的青睐。现在，通过MemFire Cloud，开发者可以轻松地为app应用添加微信移动应用授权登录功能，为用户提供更加流畅、安全的登录体验。

### 前提条件

1. 您已成功注册MemFire Cloud平台的账号，并创建了一个应用。
2. 您已成功注册微信开放平台账号。


### 配置步骤

#### 1、在微信开放平台上“创建移动应用”

{{% alert context="info" %}}
首先，需要您在微信开放平台创建一个“移动应用”，待审核通过后即可获得该“移动应用”对应的AppID、AppSecret。
{{% /alert %}}


<img src="../../../../img/wechatapp1.png">


#### 2、在MemFire Cloud应用界面启用微信（移动应用）并填入相应appid信息 

<img src="../../../../img/wechatapp2.png">

### 使用方法

#### 引入包

```js
  fluwx: ^3.4.2
  gotrue_wechatlogin: ^2.7.2

```

fluwx是基于Flutter的微信SDK框架，并支持以下功能：
- 分享图片，文本，音乐，视频等。支持分享到会话，朋友圈以及收藏
- 微信支付
- 在微信登录时，获取Auth Code
- 拉起小程序
- 订阅消息
- 打开微信
- 从微信标签打开应用
gotrue_wechatlogin是MemFire Cloud基于gotrue开发用于微信认证。


### Flutter 使用教程 

#### 初始化fluwx

```dart
  //微信登录初始化
  initWXLogin() async {
    fluwx.registerWxApi(
        appId: 'wxxxxxx', //查看微信开放平台
        doOnAndroid: true,
        doOnIOS: true,
        universalLink: 'xxxxx' //查看微信开放平台
        );
  }
```

#### 调用fluwx的 sendWeChatAuth 方法， 请求code

<img src="../../../../img/wechatapp3.png">

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("登录")),
      body: Center(
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            Text(_userId ?? "还没登录"),
            FutureBuilder<bool>(
              future: fluwx.isWeChatInstalled,
              builder: (context, snapshot) {
                if (snapshot.connectionState == ConnectionState.waiting) {
                  return CircularProgressIndicator();  // 等待异步任务完成
                } else if (snapshot.hasError) {
                  return Text("错误提示Error: ${snapshot.error}");
                } else if (snapshot.hasData && snapshot.data!) {
                  // 如果安装了微信，显示微信登录按钮
                  return ElevatedButton(
                    onPressed: () async {
                      bool exist = await fluwx.isWeChatInstalled;
                      if (exist) {
                        fluwx.sendWeChatAuth(
                          scope: "snsapi_userinfo",
                          state: "wechat_sdk_demo_test",
                        );
                      }
                    },
                    child: const Text("微信登录"),
                  );
                } else {
                  // 如果没有安装微信
                  return const Text("没有安装微信，系统不显示按钮");
                }
              },
            ),
          ],
        ),
      ),
    );
  }
```

#### 微信登录结果监听获取到code 后，请求signInWithWechat登录

```dart
  // 微信登录结果监听方法
  void loginWxHandler() async {
    fluwx.weChatResponseEventHandler.listen((response) async {
      if (response is fluwx.WeChatAuthResponse) {
        if (response.code != null) {
          print('wxLogin--code:${response.code}');
          final String nonNullableCode = response.code ?? ""; // 如果response.code为空，则使用空字符串代替
          setState(() {
            _code = response.code ?? "";
          });
            final auth = GoTrueClient(
              url: "xxx",
              headers: {
                'Authorization': 'Bearer xxx',
                'apikey': 'xxxx',
              },
            );
          final res = await auth.signInWithWechat(code: nonNullableCode);
          setState(() {
            _userId__ = res.user?.id;
          });
          
          final session = res.session;
          final user = res.user;
        } else {
          print('wx------------e$response');
          // print('wxLogin--code:${event.code}');
        }
      }else{
        print('未安装微信');
      }
    });
  }
```

全部代码
main.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_application_dmo1/pages/account_page.dart';
import 'package:flutter_application_dmo1/pages/login_page.dart';
import 'package:flutter_application_dmo1/pages/splash_page.dart';
import 'package:fluwx/fluwx.dart' as fluwx;

Future<void> main() {
 
    fluwx.registerWxApi(
        appId: 'xxx', //查看微信开放平台
        doOnAndroid: true,
        doOnIOS: true
        );

  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Supabase Flutter',
      theme: ThemeData.dark().copyWith(
        primaryColor: Colors.green,
        textButtonTheme: TextButtonThemeData(
          style: TextButton.styleFrom(
            foregroundColor: Colors.green,
          ),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            foregroundColor: Colors.white,
            backgroundColor: Colors.green,
          ),
        ),
      ),
      initialRoute: '/',
      routes: <String, WidgetBuilder>{
        '/': (_) => const SplashPage(),
        '/login': (_) => const LoginPage(),
      },
    );
  }
}

```

login_page.dart

```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:gotrue_wechatlogin/gotrue.dart';
import 'package:flutter_application_dmo1/main.dart';
import 'package:fluwx/fluwx.dart' as fluwx;

class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  // ignore: library_private_types_in_public_api
  _LoginPageState createState() => _LoginPageState();
  
}

class _LoginPageState extends State<LoginPage> {
  String? _userId;
  String? _code;
  String? _userId__;
  @override
  void initState() {
    super.initState();
    loginWxHandler(); // 在 initState 中调用微信登录结果监听方法
  }

  late final StreamSubscription _authStateSubscription;

  // 微信登录结果监听方法
  void loginWxHandler() async {
    fluwx.weChatResponseEventHandler.listen((response) async {
      if (response is fluwx.WeChatAuthResponse) {
        if (response.code != null) {
          print('wxLogin--code:${response.code}');
          final String nonNullableCode = response.code ?? ""; // 如果response.code为空，则使用空字符串代替
          setState(() {
            _code = response.code ?? "";
          });
            final auth = GoTrueClient(
              url: "https://cpeov0k8c94v6pnsqqg0.baseapi.test1.nimbleyun.com",
              headers: {
                'Authorization': 'Bearer xxx',
                'apikey': 'xxx',
              },
            );
          final res = await auth.signInWithWechat(code: nonNullableCode);
          setState(() {
            _userId__ = res.user?.id;
          });
          
          final session = res.session;
          final user = res.user;
        } else {
          print('wx------------e$response');
          // print('wxLogin--code:${event.code}');
        }
      }else{
        print('未安装微信');
      }
    });
  }

  @override
  void dispose() {
    _authStateSubscription.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("登录")),
      body: Center(
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            Text(_code != null ? "code为:${_code} ;用户id为:${_userId__}": "还没登录"),
            FutureBuilder<bool>(
              future: fluwx.isWeChatInstalled,
              builder: (context, snapshot) {
                if (snapshot.connectionState == ConnectionState.waiting) {
                  return CircularProgressIndicator();  // 等待异步任务完成
                } else if (snapshot.hasError) {
                  return Text("错误提示Error: ${snapshot.error}");
                } else if (snapshot.hasData && snapshot.data!) {
                  // 如果安装了微信，显示微信登录按钮
                  return ElevatedButton(
                    onPressed: () async {
                      bool exist = await fluwx.isWeChatInstalled;
                      if (exist) {
                        fluwx.sendWeChatAuth(
                          scope: "snsapi_userinfo",
                          state: "wechat_sdk_demo_test",
                        );
                      }
                    },
                    child: const Text("微信登录"),
                  );
                } else {
                  // 如果没有安装微信
                  return const Text("没有安装微信，系统不显示按钮");
                }
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

移动端app微信登录示例：
[https://github.com/LucaRao/wechat_app_demo](https://github.com/LucaRao/wechat_app_demo)