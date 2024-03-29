---
    weight: 1455
    title: "Supabase Remix认证"
    description: 'Remix中加载程序和操作的身份验证助手。'
    icon: "article"
    draft: false
    toc: true
---

该子模块为在Remix应用程序中实现用户身份验证提供了方便的帮助。

## 安装Remix助手库

{{< tabs tabTotal="2" >}}
{{% tab tabName="npm" %}}

```sh
npm install @supabase/auth-helpers-remix
```

此库支持以下工具版本：

- Remix: `>=1.7.2`

{{% /tab %}}
{{% tab tabName="Yarn" %}}

```sh
yarn add @supabase/auth-helpers-remix
```

此库支持以下工具版本：

- Remix: `>=1.7.2`

{{% /tab %}}
{{< /tabs >}}

## 设置环境变量

在项目的`应用设置`->`API`中检索项目URL和匿名密钥以设置以下环境变量。对于本地开发，您可以将其设置为`.env`文件。[参见示例](https://github.com/supabase/auth-helpers/blob/main/examples/remix/.env.example).

```bash title=.env
SUPABASE_URL=YOUR_SUPABASE_URL
SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

## 加载器

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}


加载器函数在组件呈现之前立即在服务器上运行。它们响应路由上的所有GET请求。您可以通过调用 `createServerClient`函数并将`SUPABASE_URL`、`SUPABASE_ANON_KEY`以及`请求`和`响应`传递给它，来创建经过身份验证的超级数据库客户端。

```jsx
import { json } from '@remix-run/node' // change this import to whatever runtime you are using
import { createServerClient } from '@supabase/auth-helpers-remix'

export const loader = async ({ request }) => {
  const response = new Response()
  // an empty response is required for the auth helpers
  // to set cookies to manage auth

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )

  const { data } = await supabaseClient.from('test').select('*')

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { data },
    {
      headers: response.headers,
    }
  )
}
```

> Supabase将设置cookie头来管理用户的认证会话，因此，`response.headers`必须从`Loader`函数返回。

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

在组件被渲染之前，加载器函数立即在服务器上运行。你可以通过调用`createServerClient`函数并将你的`SUPABASE_URL`、`SUPABASE_ANON_KEY`以及`Request`和`Response`传递给它来创建一个经过认证的Supabase客户端。

```jsx
import { LoaderFunction, json } from '@remix-run/node' // change this import to whatever runtime you are using
import { createServerClient } from '@supabase/auth-helpers-remix'

export const loader: LoaderFunction = async ({ request }: { request: Request }) => {
  const response = new Response()
  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )

  const { data } = await supabaseClient.from('test').select('*')

  return json(
    { data },
    {
      headers: response.headers,
    }
  )
}
```

> Supabase将设置cookie头来管理用户的认证会话，因此，`response.headers`必须从`Loader`函数返回。

{{% /tab %}}
{{< /tabs >}}

## Action

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

动作函数在服务器上运行，并响应对路由的HTTP请求，而不是GET - POST、PUT、PATCH、DELETE等。你可以通过调用`createServerClient`函数并将你的`SUPABASE_URL`、`SUPABASE_ANON_KEY`以及`Request`和`Response`传递给它来创建一个经过认证的Supabase客户端。

```jsx
import { json } from '@remix-run/node' // change this import to whatever runtime you are using
import { createServerClient } from '@supabase/auth-helpers-remix'

export const action = async ({ request }) => {
  const response = new Response()

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )

  const { data } = await supabaseClient.from('test').select('*')

  return json(
    { data },
    {
      headers: response.headers,
    }
  )
}
```

> Supabase将设置cookie头来管理用户的认证会话，因此，`response.headers`必须从`Action`函数中返回。

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

动作函数在服务器上运行，并响应对路由的HTTP请求，而不是GET - POST、PUT、PATCH、DELETE等。你可以通过调用`createServerClient`函数并将你的`SUPABASE_URL`、`SUPABASE_ANON_KEY`以及`Request`和`Response`传递给它来创建一个经过认证的Supabase客户端。

```jsx
import { ActionFunction, json } from '@remix-run/node' // change this import to whatever runtime you are using
import { createServerClient } from '@supabase/auth-helpers-remix'

export const action: ActionFunction = async ({ request }: { request: Request }) => {
  const response = new Response()

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )

  const { data } = await supabaseClient.from('test').select('*')

  return json(
    { data },
    {
      headers: response.headers,
    }
  )
}
```

> Supabase将设置cookie头来管理用户的认证会话，因此，`response.headers`必须从`Action`函数中返回。

{{% /tab %}}
{{< /tabs >}}

## 会话和用户

你可以通过使用`getSession`函数检查用户的会话来确定用户是否被认证。

```jsx
const {
  data: { session },
} = await supabaseClient.auth.getSession()
```

The session contains a user property.

```jsx
const user = session?.user
```

> 这是访问登录用户的推荐方法。也有一个`getUser()`函数，但如果会话已经过期，它不会刷新。

## 客户端

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

为了在浏览器中使用Supabase客户端--在`useEffect`中获取数据或订阅实时事件--我们需要多做一些工作。Remix不包括使环境变量对浏览器可用的方法，所以我们需要从`root.jsx`路由中的`loader`函数将它们连接到`window`中。

```jsx title=app/root.jsx
export const loader = () => {
  const { SUPABASE_URL, SUPABASE_ANON_KEY } = process.env
  return json({
    env: {
      SUPABASE_URL,
      SUPABASE_ANON_KEY,
    },
  })
}
```

> 对于Node以外的环境，这些可能不会被存储在`process.env`中。

接下来，我们在组件中调用`useLoaderData`钩子来获取`env`对象。

```jsx title=app/root.jsx
const { env } = useLoaderData()
```

然后，添加一个`<script>`标签，将这些环境变量附加到`window`中。这应该被放在`app/root.jsx`中的`<Scripts />`组件之前。

```jsx title=app/root.jsx
<script
  dangerouslySetInnerHTML={{
    __html: `window.env = ${JSON.stringify(env)}`,
  }}
/>
```

Node的完整例子:

```jsx title=app/root.jsx
import { json } from '@remix-run/node' // change this import to whatever runtime you are using
import {
  Form,
  Links,
  LiveReload,
  Meta,
  Outlet,
  Scripts,
  ScrollRestoration,
  useLoaderData,
} from '@remix-run/react'
import { createBrowserClient, createServerClient } from '@supabase/auth-helpers-remix'

export const meta = () => ({
  charset: 'utf-8',
  title: 'New Remix App',
  viewport: 'width=device-width,initial-scale=1',
})

export const loader = () => {
  const { SUPABASE_URL, SUPABASE_ANON_KEY } = process.env
  return json({
    env: {
      SUPABASE_URL,
      SUPABASE_ANON_KEY,
    },
  })
}

export default function App() {
  const { env } = useLoaderData()

  return (
    <html lang="en">
      <head>
        <Meta />
        <Links />
      </head>
      <body>
        <Outlet />
        <ScrollRestoration />
        <script
          dangerouslySetInnerHTML={{
            __html: `window.env = ${JSON.stringify(env)}`,
          }}
        />
        <Scripts />
        <LiveReload />
      </body>
    </html>
  )
}
```

现在我们可以在我们的组件中调用`createBrowserClient`来获取客户端的数据，或者订阅实时事件--数据库中的变化。

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

为了在浏览器中使用Supabase客户端--在`useEffect`中获取数据或订阅实时事件--我们需要多做一些工作。Remix不包括使环境变量对浏览器可用的方法，所以我们需要从`root.tsx`路由中的`loader`函数将它们连接到`window`上。

```jsx title=app/root.tsx
export const loader = () => {
  const { SUPABASE_URL, SUPABASE_ANON_KEY } = process.env
  return json({
    env: {
      SUPABASE_URL,
      SUPABASE_ANON_KEY,
    },
  })
}
```

> 对于Node以外的环境，这些可能不会被存储在`process.env`中。

接下来，我们在组件中调用`useLoaderData`钩子来获取`env`对象。

```jsx title=app/root.tsx
const { env } = useLoaderData()
```

然后，添加一个`<script>`标签，将这些环境变量附加到`window`中。这应该被放在`app/root.tsx`中的`<Scripts />`组件之前。

```jsx title=app/root.tsx
<script
  dangerouslySetInnerHTML={{
    __html: `window.env = ${JSON.stringify(env)}`,
  }}
/>
```

Node的完整示例:

```jsx title=app/root.tsx
import { json, MetaFunction, LoaderFunction } from '@remix-run/node' // change this import to whatever runtime you are using
import {
  Form,
  Links,
  LiveReload,
  Meta,
  Outlet,
  Scripts,
  ScrollRestoration,
  useLoaderData,
} from '@remix-run/react'
import { createBrowserClient, createServerClient } from '@supabase/auth-helpers-remix'

export const meta: MetaFunction = () => ({
  charset: 'utf-8',
  title: 'New Remix App',
  viewport: 'width=device-width,initial-scale=1',
})

export const loader: LoaderFunction = () => {
  const { SUPABASE_URL, SUPABASE_ANON_KEY } = process.env
  return json({
    env: {
      SUPABASE_URL,
      SUPABASE_ANON_KEY,
    },
  })
}

export default function App() {
  const { env } = useLoaderData()

  return (
    <html lang="en">
      <head>
        <Meta />
        <Links />
      </head>
      <body>
        <Outlet />
        <ScrollRestoration />
        <script
          dangerouslySetInnerHTML={{
            __html: `window.env = ${JSON.stringify(env)}`,
          }}
        />
        <Scripts />
        <LiveReload />
      </body>
    </html>
  )
}
```

现在我们可以在我们的组件中调用`createBrowserClient`来获取客户端的数据，或者订阅实时事件--数据库中的变化。

{{% /tab %}}
{{< /tabs >}}

## 身份认证

现在，认证是基于cookie的，用户可以通过操作在服务器端签入和签出。

给出这个Remix `<Form />`组件。

```jsx
<Form method="post">
  <input type="text" name="email" />
  <input type="password" name="password" />
  <button type="submit">Go!</button>
</Form>
```

### 注册

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

任何[来自`supabase-js`的支持的认证策略](/docs/app/sdkdocs/javascript/auth-signup)都可以在服务器端工作。这就是你如何处理简单的 `email`和 `password`认证。

```jsx
export const action = async ({ request }) => {
  const { email, password } = Object.fromEntries(await request.formData())
  const response = new Response()

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )

  const { data, error } = await supabaseClient.auth.signUp({
    email,
    password,
  })

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { data, error },
    {
      headers: response.headers,
    }
  )
}
```

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

任何[来自`supabase-js`的支持的认证策略](/docs/app/sdkdocs/javascript/auth-signup)都可以在服务器端工作。这就是你如何处理简单的 `email`和 `password`认证。

```jsx
export const action: ActionFunction = async ({
  request
}: {
  request: Request;
}) => {
  const { email, password } = Object.fromEntries(await request.formData());
  const response = new Response();

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL!,
    process.env.SUPABASE_ANON_KEY!,
    { request, response }
  );

  const { data, error } = await supabaseClient.auth.signUp({
    email: String(registerEmail),
    password: String(registerPassword)
  });

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { data, error },
    {
      headers: response.headers
    }
  );
};
```

{{% /tab %}}
{{< /tabs >}}

### 登录

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

任何[来自`supabase-js`的支持的认证策略](/docs/app/sdkdocs/javascript/auth-signinwithpassword)都可以在服务器端工作。这就是你如何处理简单的 `email`和 `password`认证。

```jsx
export const action = async ({ request }) => {
  const { email, password } = Object.fromEntries(await request.formData())
  const response = new Response()

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )

  const { data, error } = await supabaseClient.auth.signInWithPassword({
    email: String(loginEmail),
    password: String(loginPassword),
  })

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { data, error },
    {
      headers: response.headers,
    }
  )
}
```

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

任何[来自`supabase-js`的支持的认证策略](/docs/app/sdkdocs/javascript/auth-signinwithpassword)都可以在服务器端工作。这就是你如何处理简单的 `email`和 `password`认证。

```jsx
export const action: ActionFunction = async ({
  request
}: {
  request: Request;
}) => {
  const { email, password } = Object.fromEntries(await request.formData());
  const response = new Response();

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL!,
    process.env.SUPABASE_ANON_KEY!,
    { request, response }
  );

  const { data, error } = await supabaseClient.auth.signInWithPassword({
    email: String(loginEmail),
    password: String(loginPassword)
  });

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { data, error },
    {
      headers: response.headers
    }
  );
};
```

{{% /tab %}}
{{< /tabs >}}

### 退出登录

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

```jsx
export const action = async ({ request }) => {
  const { email, password } = Object.fromEntries(await request.formData())
  const response = new Response()

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )

  const { error } = await supabaseClient.auth.signOut()

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { error },
    {
      headers: response.headers,
    }
  )
}
```

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

```jsx
export const action: ActionFunction = async ({
  request
}: {
  request: Request;
}) => {
  const { email, password } = Object.fromEntries(await request.formData());
  const response = new Response();

  const supabaseClient = createServerClient(
    process.env.SUPABASE_URL!,
    process.env.SUPABASE_ANON_KEY!,
    { request, response }
  );

  const { error } = await supabaseClient.auth.signOut();

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { error },
    {
      headers: response.headers
    }
  );
};
```

{{% /tab %}}
{{< /tabs >}}

## 订阅实时事件

```jsx
import { createBrowserClient } from '@supabase/auth-helpers-remix'
import { useState, useEffect } from 'react'

export default function SubscribeToRealtime() {
  const [data, setData] = useState([])

  useEffect(() => {
    const supabaseClient = createBrowserClient(
      window.env.SUPABASE_URL,
      window.env.SUPABASE_ANON_KEY
    )
    const channel = supabaseClient
      .channel('test')
      .on('postgres_changes', { event: 'INSERT', schema: 'public', table: 'test' }, (payload) => {
        setData((data) => [...data, payload.new])
      })
      .subscribe()

    return () => {
      supabaseClient.removeChannel(channel)
    }
  }, [session])

  return <pre>{JSON.stringify({ data }, null, 2)}</pre>
}
```

> 注意：`window.env`不是由Remix自动填充的。请看上面的 `客户端`说明来配置它。

在这个例子中，我们要监听`test`表的`INSERT`事件。只要Supabase的`test`表有新行，我们的用户界面就会自动更新新数据。

## 在实时事件中合并服务器和客户端状态

```jsx
import { json, LoaderFunction } from '@remix-run/node';
import { useLoaderData, useNavigate } from '@remix-run/react';
import {
  createServerClient,
  createBrowserClient
} from '@supabase/auth-helpers-remix';
import { useEffect } from 'react';
import { Database } from '../../db_types';

// this route demonstrates how to subscribe to realtime updates
// and synchronize data between server and client
export const loader: LoaderFunction = async ({
  request
}: {
  request: Request;
}) => {
  const response = new Response();
  const supabaseClient = createServerClient<Database>(
    process.env.SUPABASE_URL!,
    process.env.SUPABASE_ANON_KEY!,
    { request, response }
  );

  const {
    data: { session }
  } = await supabaseClient.auth.getSession();

  const { data, error } = await supabaseClient.from('test').select('*');

  if (error) {
    throw error;
  }

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { data, session },
    {
      headers: response.headers
    }
  );
};

export default function SubscribeToRealtime() {
  const { data, session } = useLoaderData();
  const navigate = useNavigate();

  useEffect(() => {
    // Note: window.env is not automatically populated by Remix
    // Check out the [example in this repo](../root.tsx) or
    // [Remix docs](https://remix.run/docs/en/v1/guides/envvars#browser-environment-variables) for more info
    const supabaseClient = createBrowserClient<Database>(
      window.env.SUPABASE_URL,
      window.env.SUPABASE_ANON_KEY
    );
    // make sure you have enabled `Replication` for your table to receive realtime events
    // https://supabase.com/docs/app/development_guide/database/replication
    const channel = supabaseClient
      .channel('test')
      .on(
        'postgres_changes',
        { event: '*', schema: 'public', table: 'test' },
        (payload: any) => {
          // you could manually merge the `payload` with `data` here
          // the `navigate` trick below causes all active loaders to be called again
          // this handles inserts, updates and deletes, keeping everything in sync
          // which feels more remix-y than manually merging state
          // https://sergiodxa.com/articles/automatic-revalidation-in-remix
          navigate('.', { replace: true });
        }
      )
      .subscribe();

    return () => {
      supabaseClient.removeChannel(channel);
    };
  }, [session]);

  return (
    <div style={{ fontFamily: 'system-ui, sans-serif', lineHeight: '1.4' }}>
      <pre>{JSON.stringify({ data }, null, 2)}</pre>
    </div>
  );
}
```

> 注意：`window.env`不是由Remix自动填充的。请看上面的 `客户端`说明来配置它。

## 使用TypeScript的用法

你可以将[用Supabase CLI生成的](/docs/app/sdkdocs/javascript/typescript-support#generating-types)类型传递给`createServerClient`或`createBrowserClient`函数以获得增强的类型安全和自动完成。

### 服务器端

```ts
import { createServerClient } from '@supabase/auth-helpers-remix'
import { Database } from '../../db_types'

export const loader = async ({ request }) => {
  const response = new Response()

  const supabaseClient = createServerClient<Database>(
    process.env.SUPABASE_URL,
    process.env.SUPABASE_ANON_KEY,
    { request, response }
  )
}
```

### 客户端

```ts
import { createBrowserClient } from '@supabase/auth-helpers-remix'
import { Database } from '../../db_types'

const supabaseClient = createBrowserClient<Database>(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
)
```

## 使用`provider_token`向OAuth APIs获取服务器端数据

当使用第三方认证供应商时，会话是由一个额外的`provider_token`字段发起的，该字段被持久地保存在认证cookie中，可以在会话对象中访问。`provider_token`可以用来代表登录的用户向OAuth提供商的API端点发出API请求。

```js
import { json, LoaderFunction, redirect } from '@remix-run/node'; // change this import to whatever runtime you are using
import { useLoaderData } from '@remix-run/react';
import { createServerClient, User } from '@supabase/auth-helpers-remix';
import { Database } from '../../db_types';

export const loader: LoaderFunction = async ({
  request
}: {
  request: Request;
}) => {
  const response = new Response();

  const supabaseClient = createServerClient<Database>(
    process.env.SUPABASE_URL!,
    process.env.SUPABASE_ANON_KEY!,
    { request, response }
  );

  const {
    data: { session }
  } = await supabaseClient.auth.getSession();

  if (!session) {
    // there is no session, therefore, we are redirecting
    // to the landing page. we still need to return
    // response.headers to attach the set-cookie header
    return redirect('/', {
      headers: response.headers
    });
  }

  // Retrieve provider_token & logged in user's third-party id from metadata
  const { provider_token, user } = session;
  const userId = user.user_metadata.user_name;

  const allRepos = await (
    await fetch(`https://api.github.com/search/repositories?q=user:${userId}`, {
      method: 'GET',
      headers: {
        Authorization: `token ${provider_token}`
      }
    })
  ).json();

  // in order for the set-cookie header to be set,
  // headers must be returned as part of the loader response
  return json(
    { user, allRepos },
    {
      headers: response.headers
    }
  );
};

export default function ProtectedPage() {
  // by fetching the user in the loader, we ensure it is available
  // for first SSR render - no flashing of incorrect state
  const { user, allRepos } = useLoaderData<{ user: User; allRepos: any }>();

  return <pre>{JSON.stringify({ user, allRepos }, null, 2)}</pre>;
}
```


