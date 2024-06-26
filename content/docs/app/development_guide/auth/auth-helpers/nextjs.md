---
    weight: 1453
    title: "使用Next.js进行Supabase认证"
    description: 'Next的身份验证助手。js API路由、中间件和SSR。'
    icon: "article"
    draft: false
    toc: true
---

该子模块为在Next中实现用户身份验证提供了方便的帮助Next.js应用程序。

## 安装Next.js助手库

{{< tabs tabTotal="2" >}}
{{% tab tabName="npm" %}}

```sh
npm install @supabase/auth-helpers-nextjs
```

此库支持以下工具版本：

- Node.js: `^10.13.0 || >=12.0.0`
- Next.js: `>=10`
- 注：Next.js13除了新的`app`目录方法外，其他都受支持。我们正在努力增加对此的支持，您可以关注[此处](https://github.com/supabase/auth-helpers/issues/341).

此外，为可在所有基于React的框架中使用的组件和挂钩安装**React Auth Helpers**。

```sh
npm install @supabase/auth-helpers-react
```

{{% /tab %}}
{{% tab tabName="Yarn" %}}

```sh
yarn add @supabase/auth-helpers-nextjs
```

此库支持以下工具版本：

- Node.js: `^10.13.0 || >=12.0.0`
- Next.js: `>=10`
- 注：Next.js13除了新的`app`目录方法外，其他都受支持。我们正在努力增加对此的支持，您可以关注[此处](https://github.com/supabase/auth-helpers/issues/341).

此外，为可在所有基于React的框架中使用的组件和挂钩安装**React Auth Helpers**。

```sh
yarn add @supabase/auth-helpers-react
```

{{% /tab %}}
{{< /tabs >}}

## 设置环境变量
在项目的`应用设置`->`API`中检索项目URL和匿名密钥以设置以下环境变量。对于本地开发，可以在`.env.local`文件。参见[示例](https://github.com/supabase/auth-helpers/blob/main/examples/nextjs/.env.local.example)。

```bash title=.env.local
NEXT_PUBLIC_SUPABASE_URL=YOUR_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

## 基本设置

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

包装`pages/_app.js`组件与`SessionContextProvider`组件：

```jsx title=pages/_app.js
import { createBrowserSupabaseClient } from '@supabase/auth-helpers-nextjs'
import { SessionContextProvider } from '@supabase/auth-helpers-react'

function MyApp({ Component, pageProps }) {
  const router = useRouter()
  // Create a new supabase browser client on every first render.
  const [supabaseClient] = useState(() => createBrowserSupabaseClient())

  return (
    <SessionContextProvider
      supabaseClient={supabaseClient}
      initialSession={pageProps.initialSession}
    >
      <Component {...pageProps} />
    </SessionContextProvider>
  )
}
```

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

包装`pages/_app.js`组件与`SessionContextProvider`组件：

```tsx lines=2,8 title=pages/_app.tsx
import { createBrowserSupabaseClient } from '@supabase/auth-helpers-nextjs'
import { SessionContextProvider, Session } from '@supabase/auth-helpers-react'

function MyApp({
  Component,
  pageProps,
}: AppProps<{
  initialSession: Session
}>) {
  // Create a new supabase browser client on every first render.
  const [supabaseClient] = useState(() => createBrowserSupabaseClient())

  return (
    <SessionContextProvider
      supabaseClient={supabaseClient}
      initialSession={pageProps.initialSession}
    >
      <Component {...pageProps} />
    </SessionContextProvider>
  )
}
```

{{% /tab %}}
{{< /tabs >}}

现在，您可以通过检查 `useUser()`钩子返回的`user`对象是否已定义来确定用户是否已通过身份验证。


## 使用RLS获取客户端数据

为了使[行级别安全](/docs/app/development_guide/auth/auth-deep-dive/auth-row-level-security/)在客户端获取数据时正常工作，您需要确保使用`supabaseClient`钩子中的 `useSupabaseClient`，并且仅在 `useUser()`钩子中定义了客户端用户后才运行查询：


```jsx lines=10-17
import { Auth, ThemeSupa } from '@supabase/auth-ui-react'
import { useUser, useSupabaseClient } from '@supabase/auth-helpers-react'
import { useEffect, useState } from 'react'

const LoginPage = () => {
  const supabaseClient = useSupabaseClient()
  const user = useUser()
  const [data, setData] = useState()

  useEffect(() => {
    async function loadData() {
      const { data } = await supabaseClient.from('test').select('*')
      setData(data)
    }
    // Only run query once user is logged in.
    if (user) loadData()
  }, [user])

  if (!user)
    return (
      <Auth
        redirectTo="http://localhost:3000/"
        appearance={{ theme: ThemeSupa }}
        supabaseClient={supabaseClient}
        providers={['google', 'github']}
        socialLayout="horizontal"
      />
    )

  return (
    <>
      <button onClick={() => supabaseClient.auth.signOut()}>Sign out</button>
      <p>user:</p>
      <pre>{JSON.stringify(user, null, 2)}</pre>
      <p>client-side data fetching with RLS</p>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </>
  )
}

export default LoginPage
```

## 服务器端渲染 (SSR)

创建服务器超级客户端以检索登录用户的会话：

```jsx title=pages/profile.js
import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'

export default function Profile({ user }) {
  return <div>Hello {user.name}</div>
}

export const getServerSideProps = async (ctx) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient(ctx)
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }

  return {
    props: {
      initialSession: session,
      user: session.user,
    },
  }
}
```

## 使用RLS获取服务器端数据

您可以使用服务器超级数据库客户端运行[行级别安全性](/docs/app/development_guide/auth/auth-deep-dive/auth-row-level-security/)服务器端验证查询：

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

```jsx
import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'

export default function ProtectedPage({ user, data }) {
  return (
    <>
      <div>Protected content for {user.email}</div>
      <pre>{JSON.stringify(data, null, 2)}</pre>
      <pre>{JSON.stringify(user, null, 2)}</pre>
    </>
  )
}

export const getServerSideProps = async (ctx) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient(ctx)
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }

  // Run queries with RLS on the server
  const { data } = await supabase.from('users').select('*')

  return {
    props: {
      initialSession: session,
      user: session.user,
      data: data ?? [],
    },
  }
}
```

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

```tsx
import { User, createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'
import { GetServerSidePropsContext } from 'next'

export default function ProtectedPage({ user, data }: { user: User; data: any }) {
  return (
    <>
      <div>Protected content for {user.email}</div>
      <pre>{JSON.stringify(data, null, 2)}</pre>
      <pre>{JSON.stringify(user, null, 2)}</pre>
    </>
  )
}

export const getServerSideProps = async (ctx: GetServerSidePropsContext) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient(ctx)
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }

  // Run queries with RLS on the server
  const { data } = await supabase.from('users').select('*')

  return {
    props: {
      initialSession: session,
      user: session.user,
      data: data ?? [],
    },
  }
}
```

{{% /tab %}}
{{< /tabs >}}

## 使用 `provider token`将服务器端数据提取到OAuth API {#oauth-provider-token}

当使用第三方身份验证提供程序时，会话将使用附加的 `provider_token`字段启动，该字段保存在身份验证cookie中，可以在会话对象中访问。`provider_token`可用于代表登录用户向OAuth提供程序的API端点发出API请求。

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

```jsx
import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'

export default function ProtectedPage({ user, allRepos }) {
  return (
    <>
      <div>Protected content for {user.email}</div>
      <p>Data fetched with provider token:</p>
      <pre>{JSON.stringify(allRepos, null, 2)}</pre>
      <p>user:</p>
      <pre>{JSON.stringify(user, null, 2)}</pre>
    </>
  )
}

export const getServerSideProps = async (ctx) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient(ctx)
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }

  // Retrieve provider_token & logged in user's third-party id from metadata
  const { provider_token, user } = session
  const userId = user.user_metadata.user_name

  const allRepos = await (
    await fetch(`https://api.github.com/search/repositories?q=user:${userId}`, {
      method: 'GET',
      headers: {
        Authorization: `token ${provider_token}`,
      },
    })
  ).json()

  return { props: { user, allRepos } }
}
```

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

```tsx
import { User, createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'
import { GetServerSidePropsContext } from 'next'

export default function ProtectedPage({ user, allRepos }: { user: User; allRepos: any }) {
  return (
    <>
      <div>Protected content for {user.email}</div>
      <p>Data fetched with provider token:</p>
      <pre>{JSON.stringify(allRepos, null, 2)}</pre>
      <p>user:</p>
      <pre>{JSON.stringify(user, null, 2)}</pre>
    </>
  )
}

export const getServerSideProps = async (ctx: GetServerSidePropsContext) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient(ctx)
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }

  // Retrieve provider_token & logged in user's third-party id from metadata
  const { provider_token, user } = session
  const userId = user.user_metadata.user_name

  const allRepos = await (
    await fetch(`https://api.github.com/search/repositories?q=user:${userId}`, {
      method: 'GET',
      headers: {
        Authorization: `token ${provider_token}`,
      },
    })
  ).json()

  return { props: { user, allRepos } }
}
```

{{% /tab %}}
{{< /tabs >}}

## 保护API路由

创建服务器超级客户端以检索登录用户的会话：

{{< tabs tabTotal="2" >}}
{{% tab tabName="JavaScript" %}}

```jsx title=pages/api/protected-route.js
import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'

const ProtectedRoute = async (req, res) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient({ req, res })
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return res.status(401).json({
      error: 'not_authenticated',
      description: 'The user does not have an active session or is not authenticated',
    })

  // Run queries with RLS on the server
  const { data } = await supabase.from('test').select('*')
  res.json(data)
}

export default ProtectedRoute
```

{{% /tab %}}
{{% tab tabName="TypeScript" %}}

```tsx title=pages/api/protected-route.ts
import { NextApiHandler } from 'next'
import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'

const ProtectedRoute: NextApiHandler = async (req, res) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient({ req, res })
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return res.status(401).json({
      error: 'not_authenticated',
      description: 'The user does not have an active session or is not authenticated',
    })

  // Run queries with RLS on the server
  const { data } = await supabase.from('test').select('*')
  res.json(data)
}

export default ProtectedRoute
```

{{% /tab %}}
{{< /tabs >}}

## 使用Next.js中间件进行身份验证。

作为保护单个页面的替代方案，您可以使用[Next.js中间件](https://nextjs.org/docs/middleware)以保护整个目录或与配置对象匹配的目录。在以下示例中，所有对`/middleware protected/*`的请求都将检查用户是否已登录，如果成功，请求将被转发到目标路由，否则用户将被重定向：

```ts title=middleware.ts
import { createMiddlewareSupabaseClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export async function middleware(req: NextRequest) {
  // We need to create a response and hand it to the supabase client to be able to modify the response headers.
  const res = NextResponse.next()
  // Create authenticated Supabase Client.
  const supabase = createMiddlewareSupabaseClient({ req, res })
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  // Check auth condition
  if (session?.user.email?.endsWith('@gmail.com')) {
    // Authentication successful, forward request to protected route.
    return res
  }

  // Auth condition not met, redirect to home page.
  const redirectUrl = req.nextUrl.clone()
  redirectUrl.pathname = '/'
  redirectUrl.searchParams.set(`redirectedFrom`, req.nextUrl.pathname)
  return NextResponse.redirect(redirectUrl)
}

export const config = {
  matcher: '/middleware-protected',
}
```

## 迁移指南

### 迁移到v0.5.X

使这些助手更灵活，更易于维护，更易于为新版本的Next.js升级，我们将其分解为最有用的部分，即管理cookie，并在任何环境（客户端、服务器、中间件/边缘）中为您提供经过认证的超级js客户端。

因此，我们将`withApiAuth`、`withPageAuth` 和 `withMiddlewareAuth`高阶函数标记为已弃用，它们将在下一个**次要**版本（v0.6.X）中删除。

请按照以下步骤更新API路由、页面和中间件处理程序。谢谢

#### `withApiAuth`已弃用！

在`NextApiHandler`中使用`createServerSubaseClient`：

{{< tabs tabTotal="2" >}}
{{% tab tabName="Before" %}}

```tsx title=pages/api/protected-route.ts
import { withApiAuth } from '@supabase/auth-helpers-nextjs'

export default withApiAuth(async function ProtectedRoute(req, res, supabase) {
  // Run queries with RLS on the server
  const { data } = await supabase.from('test').select('*')
  res.json(data)
})
```

{{% /tab %}}

{{% tab tabName="After" %}}

```tsx title=pages/api/protected-route.ts
import { NextApiHandler } from 'next'
import { createServerSupabaseClient } from '@supabase/auth-helpers-nextjs'

const ProtectedRoute: NextApiHandler = async (req, res) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient({ req, res })
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return res.status(401).json({
      error: 'not_authenticated',
      description: 'The user does not have an active session or is not authenticated',
    })

  // Run queries with RLS on the server
  const { data } = await supabase.from('test').select('*')
  res.json(data)
}

export default ProtectedRoute
```

{{% /tab %}}
{{< /tabs >}}

#### `withPageAuth`已弃用！

在`getServerSideProps`中使用`createServerSubaseClient`：

{{< tabs tabTotal="2" >}}
{{% tab tabName="Before" %}}

```tsx title=pages/profile.tsx
import { withPageAuth, User } from '@supabase/auth-helpers-nextjs'

export default function Profile({ user }: { user: User }) {
  return <pre>{JSON.stringify(user, null, 2)}</pre>
}

export const getServerSideProps = withPageAuth({ redirectTo: '/' })
```

{{% /tab %}}
{{% tab tabName="After" %}}

```tsx title=pages/profile.js
import { createServerSupabaseClient, User } from '@supabase/auth-helpers-nextjs'
import { GetServerSidePropsContext } from 'next'

export default function Profile({ user }: { user: User }) {
  return <pre>{JSON.stringify(user, null, 2)}</pre>
}

export const getServerSideProps = async (ctx: GetServerSidePropsContext) => {
  // Create authenticated Supabase Client
  const supabase = createServerSupabaseClient(ctx)
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (!session)
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }

  return {
    props: {
      initialSession: session,
      user: session.user,
    },
  }
}
```

{{% /tab %}}
{{< /tabs >}}

#### `withMiddlewareAuth`已弃用！

{{< tabs tabTotal="2" >}}
{{% tab tabName="Before" %}}

```tsx title=middleware.ts
import { withMiddlewareAuth } from '@supabase/auth-helpers-nextjs'

export const middleware = withMiddlewareAuth({
  redirectTo: '/',
  authGuard: {
    isPermitted: async (user) => {
      return user.email?.endsWith('@gmail.com') ?? false
    },
    redirectTo: '/insufficient-permissions',
  },
})

export const config = {
  matcher: '/middleware-protected',
}
```

{{% /tab %}}
{{% tab tabName="After" %}}

```tsx title=middleware.ts
import { createMiddlewareSupabaseClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export async function middleware(req: NextRequest) {
  // We need to create a response and hand it to the supabase client to be able to modify the response headers.
  const res = NextResponse.next()
  // Create authenticated Supabase Client.
  const supabase = createMiddlewareSupabaseClient({ req, res })
  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  // Check auth condition
  if (session?.user.email?.endsWith('@gmail.com')) {
    // Authentication successful, forward request to protected route.
    return res
  }

  // Auth condition not met, redirect to home page.
  const redirectUrl = req.nextUrl.clone()
  redirectUrl.pathname = '/'
  redirectUrl.searchParams.set(`redirectedFrom`, req.nextUrl.pathname)
  return NextResponse.redirect(redirectUrl)
}

export const config = {
  matcher: '/middleware-protected',
}
```

{{% /tab %}}
{{< /tabs >}}

### 迁移到v0.4.X和supabase js v2

随着`supabase-js` v2的更新，不再需要 `auth`API路由，因此您可以继续删除 `/pages/api/`目录下的 `auth`目录。请参阅[v2迁移指南](/docs/app/sdkdocs/javascript/upgrade-guide)以了解supabase js中的全部更改。

`/api/auth/logout` api路由已删除，请改用 `signout` 方法：

```jsx
<button
  onClick={async () => {
    await supabaseClient.auth.signOut()
    router.push('/')
  }}
>
  Logout
</button>
```
已删除`supabaseClient` 和 `supabaseServerClient`，改用`createBrowserSupbaseClient`或`createServerSubaseClient`方法。这允许您向客户端提供CLI生成的类型：

```tsx
// client-side
import type { Database } from 'types_db'
const [supabaseClient] = useState(() => createBrowserSupabaseClient<Database>())

// server-side API route
import type { NextApiRequest, NextApiResponse } from 'next'
import type { Database } from 'types_db'

export default async (req: NextApiRequest, res: NextApiResponse) => {
  const supabaseServerClient = createServerSupabaseClient<Database>({
    req,
    res,
  })
  const {
    data: { user },
  } = await supabaseServerClient.auth.getUser()

  res.status(200).json({ name: user?.name ?? '' })
}
```

- `UserProvider` 已被 `SessionContextProvider`替换。确保包装您的 `pages/_app.js`与`SessionContextProvider`兼容。然后，在整个应用程序中，您可以使用 `useSessionContext`钩子获取 `session`，使用`useSubaseClient`钩子获取经过身份验证的`supbaseClient`。
- `useUser`钩子现在返回`user`对象或`null`。
- TypeScript的用法：您可以将[使用Suabase CLI](/docs/app/sdkdocs/javascript/typescript-support#generating-types)生成的类型传递给Supabase客户端，以获得增强的类型安全性和自动完成:

创建新的超级数据库客户端对象：

```tsx
import { Database } from '../database.types'

const [supabaseClient] = useState(() => createBrowserSupabaseClient<Database>())
```

从SessionContext中检索超级客户端对象：

```tsx
import { useSupabaseClient } from '@supabase/auth-helpers-react'
import { Database } from '../database.types'

const supabaseClient = useSupabaseClient<Database>()
```


