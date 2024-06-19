---
    weight: 1409
    title: "Next.js 设置微信扫码登录身份验证"
    icon: "article"
    draft: false
    toc: true
---

## 第一种方式(推荐使用)

此示例将为您设置一个非常常见的情况：用户可以注册或登录，然后使用公共个人资料信息（包括个人资料图片）更新其帐户。

此示例演示了：

-   使用 Supabase Auth 进行微信扫码登录认证

    -   Next.js 的 Supabase Auth 助手。
    -   Supabase 为 React 预构建了 Auth UI。

-   使用 MemFire Cloud 存储的用户头像图像

-   受政策限制的公开资料。

-   使用 Next.js 的前端。

示例代码

```
git clone https://github.com/LucaRao/nextjs-user-management.git
```

## 第二种方式

### 1.安装 Supabase 依赖包。

安装 @supabase/supabase-js 包和帮助器 @supabase/ssr 包。

```
npm install @supabase/supabase-js @supabase/ssr
```

### 2.设置环境变量

在项目根目录中创建一个 .env.local 文件。

填写您的 `NEXT_PUBLIC_SUPABASE_URL` 和 `NEXT_PUBLIC_SUPABASE_ANON_KEY`：

```
NEXT_PUBLIC_SUPABASE_URL=<your_supabase_project_url>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<your_supabase_anon_key>
```

### 3.编写实用函数来创建 Supabase 客户端

要从 Next.js 应用程序访问 Supabase，您需要 2 种类型的 Supabase 客户端：

-   客户端组件客户端 - 从在浏览器中运行的客户端组件访问 Supabase。
-   服务器组件客户端 - 从仅在服务器上运行的服务器组件、服务器操作和路由处理程序访问 Supabase。

创建一个 utils/supabase 文件夹，其中包含每种类型客户端的文件。然后复制每种客户端类型的实用函数。

utils/supabase/client.ts

```
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

utils/supabase/server.ts

```
import { createServerClient, type CookieOptions } from '@supabase/ssr'
import { cookies } from 'next/headers'

export function createClient() {
  const cookieStore = cookies()

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return cookieStore.get(name)?.value
        },
        set(name: string, value: string, options: CookieOptions) {
          try {
            cookieStore.set({ name, value, ...options })
          } catch (error) {
            // The `set` method was called from a Server Component.
            // This can be ignored if you have middleware refreshing
            // user sessions.
          }
        },
        remove(name: string, options: CookieOptions) {
          try {
            cookieStore.set({ name, value: '', ...options })
          } catch (error) {
            // The `delete` method was called from a Server Component.
            // This can be ignored if you have middleware refreshing
            // user sessions.
          }
        },
      },
    }
  )
}
```

{{% alert context="info" %}}
“cookies”对象有什么作用？

cookies对象让Supabase客户端知道如何访问cookie，因此它可以读取和写入用户会话数据。为了使 @supabase/ssr 与框架无关，cookie 方法不是硬编码的。这些实用函数针对 Next.js 调整 @supabase/ssr 的 cookie 处理。

服务器客户端的 set 和 remove 方法需要错误处理程序，因为如果从服务器组件设置 cookie，Next.js 会引发错误。您可以安全地忽略此错误，因为您将在下一步中设置中间件以将刷新的 cookie 写入存储。

默认情况下，该 cookie 名为 sb-<project_ref>-auth-token。

  


我需要为每个route创建一个新客户端吗？

  


是的！创建 Supabase 客户端是轻量级的。在服务器上，它基本上配置了一个获取调用。您需要为服务器的每个请求重新配置 fetch 调用，因为您需要请求中的 cookie。在客户端，createBrowserClient 已经使用单例模式，因此无论您调用 createClient 函数多少次，您都只能创建一个实例。
{{% /alert %}}




### 4.连接中间件

在项目的根目录下创建 middleware.ts 文件。由于服务器组件无法写入 cookie，因此您需要中间件来刷新过期的身份验证令牌并存储它们。

中间件负责：

-   刷新身份验证令牌（通过调用supabase.auth.getUser）。
-   将刷新的身份验证令牌传递给服务器组件，这样它们就不会尝试自己刷新相同的令牌。这是通过 request.cookies.set 完成的。
-   将刷新的身份验证令牌传递给浏览器，以便它替换旧令牌。这是通过response.cookies.set 完成的。

复制您的应用程序的中间件代码。

middleware.ts

```
import { type NextRequest } from 'next/server'
import { updateSession } from '@/utils/supabase/middleware'

export async function middleware(request: NextRequest) {
  return await updateSession(request)
}

export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     * Feel free to modify this pattern to include more paths.
     */
    '/((?!_next/static|_next/image|favicon.ico|.*\.(?:svg|png|jpg|jpeg|gif|webp)$).*)',
  ],
}
```

utils/supabase/middleware.ts

```
import { createServerClient, type CookieOptions } from '@supabase/ssr'
import { NextResponse, type NextRequest } from 'next/server'

export async function updateSession(request: NextRequest) {
  let response = NextResponse.next({
    request: {
      headers: request.headers,
    },
  })

  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return request.cookies.get(name)?.value
        },
        set(name: string, value: string, options: CookieOptions) {
          request.cookies.set({
            name,
            value,
            ...options,
          })
          response = NextResponse.next({
            request: {
              headers: request.headers,
            },
          })
          response.cookies.set({
            name,
            value,
            ...options,
          })
        },
        remove(name: string, options: CookieOptions) {
          request.cookies.set({
            name,
            value: '',
            ...options,
          })
          response = NextResponse.next({
            request: {
              headers: request.headers,
            },
          })
          response.cookies.set({
            name,
            value: '',
            ...options,
          })
        },
      },
    }
  )

  await supabase.auth.getUser()

  return response
}
```

### 5.配置重定向

在MemFire Cloud应用界面进行重定向URL配置

<img src="../../../../img/redreacturl.png">

### 6.创建登录页面

为您的应用程序创建登录页面。使用服务器操作调用 MemFire Cloud 微信扫码登录api。

由于 api 是从 Action 中调用的，因此请使用 @/utils/supabase/server.ts 中定义的客户端。

app/login/page.tsx

```
import { login } from "./actions";

export default function LoginPage() {
  return (
    <div className="min-h-screen flex items-center justify-center">
      <form>
        <button
          className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          formAction={login}
        >
          微信登录
        </button>
      </form>
    </div>
  );
}
```

app/login/actions.tsx

对于 PKCE 流程，例如在服务器端身份验证中，您需要一个额外的步骤来处理代码交换。调用signInWithOAuth 时，提供指向回调路由的redirectTo URL。应将此重定向 URL 添加到您的重定向允许列表中。

在服务器中，您需要处理到 OAuth 提供程序的身份验证端点的重定向。 SignInWithOAuth 方法返回您可以重定向到的端点 URL。

这里的redirectTo需要与在MemFire Cloud应用界面进行重定向的URL中的某一个保持一致

```
'use server'

import { redirect } from 'next/navigation'

import { createClient } from '@/utils/supabase/server'

export async function login() {
  const supabase = createClient()
  const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'wechat_qr',
    options: {
        redirectTo: 'http://localhost:3000/private',
    },
    
})
if (data.url) {
  redirect(data.url) // use the redirect API for your server framework
}
}
```

app/error/page.tsx

```
export default function ErrorPage() {
    return <div className="min-h-screen flex items-center justify-center">
    <p>对不起，出错了</p>
  </div> 
  }
```

请注意，在调用 Supabase服务 之前会调用 cookie，Supabase服务 会选择从 Next.js 的缓存中提取调用。这对于经过身份验证的数据获取非常重要，以确保用户只能访问自己的数据。

### 7.更改Auth确认路径

在回调端点，处理代码交换以保存用户会话。

app\private\route.ts

```
import { cookies } from 'next/headers'
import { NextResponse } from 'next/server'
import { type CookieOptions, createServerClient } from '@supabase/ssr'

export async function GET(request: Request) {
  const { searchParams, origin } = new URL(request.url)
  const code = searchParams.get('code')
  // if "next" is in param, use it as the redirect URL
  const next = searchParams.get('next') ?? '/'

  if (code) {
    const cookieStore = cookies()
    const supabase = createServerClient(
      process.env.NEXT_PUBLIC_SUPABASE_URL!,
      process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
      {
        cookies: {
          get(name: string) {
            return cookieStore.get(name)?.value
          },
          set(name: string, value: string, options: CookieOptions) {
            cookieStore.set({ name, value, ...options })
          },
          remove(name: string, options: CookieOptions) {
            cookieStore.delete({ name, ...options })
          },
        },
      }
    )
    const { error } = await supabase.auth.exchangeCodeForSession(code)
    if (!error) {
      return NextResponse.redirect(`${origin}${next}`)
    }
  }

  // return the user to an error page with instructions
  return NextResponse.redirect(`${origin}/error`)
}
```

### 8.从服务器组件访问用户信息

服务器组件可以读取cookie，因此您可以获得身份验证状态和用户信息。

由于您是从服务器组件调用 Supabase服务，因此请使用在 @/utils/supabase/server.ts 中创建的客户端。

创建一个私人页面，用户只有登录后才能访问。该页面显示他们的电子邮件。

app\page.tsx

```
import { createClient } from "@/utils/supabase/server";
import { redirect } from "next/navigation";

export default async function Home() {
  const supabase = createClient()

    const { data, error } = await supabase.auth.getUser()
    if (error || !data?.user) {
      redirect('/login')
    }
  return (
    <main className="flex min-h-screen flex-col items-center  p-24">
      <p>Hello 你的用户名为：{data.user.user_metadata.full_name}</p>
      <p>你的用户id为：{data.user.id}</p>
      <form action="/signout" method="post">
          <button className="button block" type="submit">
            退出登录
          </button>
        </form>
    </main>
  );
}
```

### 9.退出登录

需要从服务器退出登录，清除用户会话。

app\signout\route.ts

```
import { type CookieOptions, createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'
import { type NextRequest, NextResponse } from 'next/server'

export async function POST(req: NextRequest) {
    const cookieStore = cookies()
    const supabase = createServerClient(
        process.env.NEXT_PUBLIC_SUPABASE_URL!,
        process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
        {
          cookies: {
            get(name: string) {
              return cookieStore.get(name)?.value
            },
            set(name: string, value: string, options: CookieOptions) {
              cookieStore.set({ name, value, ...options })
            },
            remove(name: string, options: CookieOptions) {
              cookieStore.delete({ name, ...options })
            },
          },
        }
      )

  // Check if we have a session
  const {
    data: { session },
  } = await supabase.auth.getSession()

  if (session) {
    await supabase.auth.signOut()
  }

  return NextResponse.redirect(new URL('/login', req.url), {
    status: 302,
  })
}
```