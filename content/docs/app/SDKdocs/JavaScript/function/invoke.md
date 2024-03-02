---
    weight: 166
    title: "invoke()"
    icon: "article"
    draft: false
    toc: true
---

应用使用MemFire Cloud的云函数功能时，需要结合supabase-wechat-stable-v2与memfire-cloud-functions-js 这两个SDK进行云函数的调用。

## 使用方法

在根目录下执行命令，安装依赖

```js
npm install supabase-wechat-stable-v2

```

添加一个js/ts文件与数据库建立连接，添加以下代码，并且将应用的URL和anon_key填写进去。

```js

import { createClient } from 'supabase-wechat-stable-v2'

const supabaseUrl = ''
const supabaseAnonKey = ''

export const supabase = createClient(supabaseUrl, supabaseAnonKey)


```
在需要使用云函数的页面调用，填写用户在MemFire Cloud创建的云函数生成的访问地址和访问方法。

```js
  const { data, error } = await functions.invoke(
      '访问地址',{method:'访问方法'}
    );
```


## 参数


<ul className="method-list-group">
  
<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      functionName
    </span>
    <span className="method-list-item-label-badge required">
      required
    </span>
    <span className="method-list-item-validation">
      <code>string</code>
    </span>
  </h4>
  <div class="method-list-item-description">

要调用的函数的名称

  </div>
  
</li>


<li className="method-list-item">
  <h4 className="method-list-item-label">
    <span className="method-list-item-label-name">
      invokeOptions
    </span>
    <span className="method-list-item-label-badge required">
      required
    </span>
    <span className="method-list-item-validation">
      <code>FunctionInvokeOptions</code>
    </span>
  </h4>
  <div class="method-list-item-description">

未提供说明。

  </div>
  
</li>

</ul>


## 注释

- 需要一个授权标头。
- 调用参数通常符合[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)规范。
- 当你向函数传递一个body时，我们会自动附加 `Blob`、`ArrayBuffer`、`File`、`FormData`和 `String`的Content-Type标头。如果它不符合这些类型，会默认为`json`，将其序列化并附加`Content-Type'头为`application/json`。用户还可以自定义 `Content-Type`。
- 响应会自动解析为`json`、`blob`或`form-data`，取决于你的函数发送的`Content-Type`头。默认情况下，响应被解析为`text`。










## 示例

### 基本调用


```js
  const { data, error } = await functions.invoke(
      '访问地址',{method:'访问方法'}
    );
```

### 错误处理

如果你的函数抛出一个错误，将返回`FunctionsHttpError`；如果Supabase Relay在处理你的函数时出现错误，将返回`FunctionsRelayError`；如果调用你的函数时出现网络错误，将返回`FunctionsFetchError`。


```js
import { FunctionsHttpError, FunctionsRelayError, FunctionsFetchError } from "supabase-wechat-stable-v2";

const { data, error } = await supabase.functions.invoke('访问地址', {
  headers: {
    "my-custom-header": 'my-custom-header-value'
  },
  body: { foo: 'bar' },
  
})

if (error instanceof FunctionsHttpError) {
  console.log('Function returned an error', error.message)
} else if (error instanceof FunctionsRelayError) {
  console.log('Relay error:', error.message)
} else if (error instanceof FunctionsFetchError) {
  console.log('Fetch error:', error.message)
}
```

### 传递自定义标头和传递queries参数。

你可以向你的函数传递自定义标头信息。注意：supabase-js会自动将`授权`头与登录用户的JWT一起传递。

如果想通过queries方式传递参数，建议在访问地址后面追加query参数。如下所示：


```js
const { data, error } = await supabase.functions.invoke('https://functions1.memfiredb.com/xxxxxxxxx/functionName?action=pay', {
  headers: {
    "my-custom-header": 'my-custom-header-value'
  },
  body: { foo: 'bar' },
  method:'访问方法'
},)
```