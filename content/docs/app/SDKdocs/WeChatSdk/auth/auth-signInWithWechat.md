---
    weight: 2792
    title: "signInWithWechat()"
    icon: "article"
    draft: false
    toc: true
---

微信注册

## 使用 SDK 操作如下

```js
wx.login({
  success: async res => {
    const { data, error } = await supabase.auth.signInWithWechat({code:res.code})
  },
})
```


## 参数


{{< table "table-striped-columns" >}}
参数   | 类型 | 是否必填 | 说明
----- | -----| -----| -----
code | String |是 | 使用wx.login成功后返回的code
{{< /table >}}

## 返回参数

```js
//请求成功返回正常用户信息
data: { session: Session | null; user: User | null }; error: null 
//请求出错，返回错误信息
data: { session: null; user: null }; error: AuthError 

```

## 返回示例

### data结构如下：


  
```js
{
    "access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9xxxxxxxxxxxxxxxxxxxxxxxx",
    "token_type":"bearer",
    "expires_in":3600,
    "refresh_token":"xxxxxxxxxxx",
    "user":{
        "id":"xxxxxxxxxxxxx",
        "aud":"authenticated",
        "role":"authenticated",
        "email":"",
        "phone":"",
        "wechat_id":"xxxxxxxxxxxxx",
        "wechat_unionid":"",
        "last_sign_in_at":"2023-03-13T02:47:38.729272028Z",
        "app_metadata":{
            "provider":"wechat_mini",
            "providers":[
                "wechat_mini"
            ]
        },
        "user_metadata":{

        },
        "identities":[

        ],
        "created_at":"2023-03-13T02:47:38.681877Z",
        "updated_at":"2023-03-13T02:47:38.740396Z"
    }
}

```