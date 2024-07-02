---
    weight: 1413
    title: "微信网站应用扫码登录"
    description: "微信扫码登录认证"
    icon: "article"
    draft: false
    toc: true
---


### 前言

为了满足国内用户日益增长的操作习惯需求，并进一步提升用户体验，MemFire Cloud认证服务已集成微信扫码登录功能。微信，作为国内广受欢迎的社交平台，其扫码登录功能以其便捷性和快速性赢得了广大用户的青睐。现在，通过MemFire Cloud，开发者可以轻松地为WEB应用添加微信扫码登录功能，为用户提供更加流畅、安全的登录体验。

### 前提条件

1. 您已成功注册MemFire Cloud平台的账号，并创建了一个应用。
2. 您已成功注册微信开放平台账号。
3. 您拥有一个正确的网站应用官网，用于在微信开放平台上创建网站应用时提交审核。


### 配置步骤

#### 1、在微信开放平台上“创建网站应用”

{{% alert context="info" %}}
首先，需要您在微信开放平台创建一个“网站应用”，待审核通过后即可获得该“网站应用”对应的AppID、AppSecret。
{{% /alert %}}


<img src="../../../../img/wechatqr1.png">


#### 2、在MemFire Cloud应用界面启用微信扫码认证

<img src="../../../../img/wechatqr2.png">

#### 3、在微信开放平台上对审核通过的网站应用配置其授权回调域

{{% alert context="info" %}}
在微信开放平台，进入到第1步审核通过的“网站应用”中进行“授权回调域”的配置，配置的值为上一步中微信扫码认证服务商的重定向URL的cpa09na5g6h7mlpvrehg.baseapi.memfiredb.com这一部分。此处注意，不要在“授权回调域”的前面加上“https://”，也不需要在其之后加上具体的路径。
{{% /alert %}}

   正确填写格式如下：
<img src="../../../../img/wechatqr3.png">

#### 4、在MemFire Cloud应用界面进行重定向URL配置

{{% alert context="info" %}}
将审核通过的网站应用界面里的“应用官网”填写到Memfire Cloud中 认证>URL配置>网站URL中即可，也可以配置“重定向URL列表”。这样，当完成微信扫码登录后，将会默认重定向到“网站URL”或是“重定向URL列表”的其中一个指定网址。
{{% /alert %}}

<img src="../../../../img/wechatqr4.png">

### SDK使用教程

#### 1、SDK安装

```js
npm install @supabase/supabase-js

```

#### 2、当您的用户进行登录时，调用signInWithOAuth()接口，将wechat_qr作为provider即可。

```js
<button onclick="signInWithWechatQr()">微信扫码登录</button>

async function signInWithWechatQr() {
    const { data, error } = await _supabase.auth.signInWithOAuth({
        provider: 'wechat_qr',
        // 如果不配置下面的options则登录成功后将会默认重定向到“网站URL”
        // 如果配置下面的options，则需要保持redirectTo参数的值与重定向URL列表中的值一致，登录成功后将会跳转到该网址
        options: {
            redirectTo: 'https://memfiredb.com/',
        },
    })
}



```

#### 3、当用户注销时，调用signOut()接口将其从浏览器会话和localStorage中删除：

```js
async function signout() {
  const { error } = await supabase.auth.signOut()
}

```
