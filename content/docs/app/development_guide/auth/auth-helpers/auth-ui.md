---
    weight: 1452
    title: "身份验证UI"
    description: '一个预构建的、可定制的React组件，用于验证用户。'
    icon: "article"
    draft: false
    toc: true
---

Auth UI是用于验证用户的预构建React组件。
它支持定制主题和可扩展样式，以符合您的品牌和审美。

<video width="99%" muted playsInline controls="true">
  <source
    src="https://supabase.com/images/blog/lw5-one-more/auth-ui-demo.mp4"
    type="video/mp4"
    muted
    playsInline
  />
</video>

## 设置身份验证UI

安装最新版本的[supabase js](/docs/app/sdkdocs/javascript)和Auth UI包：

```bash
npm install @supabase/supabase-js @supabase/auth-ui-react
```

### 导入Auth组件

将 `@supabase/supabase-js` 中的 `supabaseClient` 作为属性传递给组件。

```js title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'

const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')

const App = () => <Auth supabaseClient={supabase} />
```

这将在没有任何样式的情况下渲染Auth组件。 我们建议使用预定义的主题之一来设置UI的样式。 导入要使用的主题并将其传递给`appearence.theme`属性。

```js lines=4,11 title=/src/index.js
import {
  Auth,
  // Import predefined theme
  ThemeSupa,
} from '@supabase/auth-ui-react'

const App = () => (
  <Auth
    supabaseClient={supabase}
    {/* Apply predefined theme */}
    appearance={{ theme: ThemeSupa }}
  />
)
```

## 自定义

有几种自定义身份验证UI的方法：

- 使用Auth UI附带的[预定义主题](#predefined-themes)之一
- 通过主题中的[覆盖变量标记](#override-themes)扩展主题
- [创建自己的主题](#create-theme)
- [使用您自己的CSS类](#custom-css-classes)
- [使用内联样式](#custom-inline-styles)
- [使用自己的标签](#custom-labels)

### 预定义主题

AuthUI提供了几个主题来自定义外观。每个预定义主题至少有两种变体，一种是`default`变体，另一种则是`dark`变体。您可以使用`theme`属性在这些主题之间切换。导入要使用的主题并将其传递给`appearence.theme`属性。

```js lines=2,10 title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth, ThemeSupa } from '@supabase/auth-ui-react'

const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')

const App = () => (
  <Auth
    supabaseClient={supabase}
    {/* Apply predefined theme */}
    appearance={{ theme: ThemeSupa }}
  />
)
```

{{% alert context="info" %}}

目前只有一个预定义的主题可用，但我们计划添加更多主题。

{{% /alert %}}

### 切换主题变体

Auth UI有两种主题变体：`default`和`dark`。您可以使用`theme`属性在这些主题之间切换。

```js lines=11 title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth, ThemeSupa } from '@supabase/auth-ui-react'

const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')

const App = () => (
  <Auth
    supabaseClient={supabase}
    appearance={{ theme: ThemeSupa }}
    {/* Set theme to dark */}
    theme="dark"
  />
)
```

如果不向`theme`传递值，它将使用`"default"`主题。您可以将`"dark"`传递给主题道具以切换到 `dark`主题。如果主题有其他变体，请使用此道具中变体的名称。

### 覆盖主题

可以使用变量令牌重写身份验证UI主题。参见[变量标记列表](https://github.com/supabase-community/auth-ui/blob/main/packages/react/common/theming/Themes.tsx).

```js lines=11-18 title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth, ThemeSupa } from '@supabase/auth-ui-react'

const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')

const App = () => (
  <Auth
    supabaseClient={supabase}
    appearance={{
      theme: ThemeSupa,
      variables: {
        default: {
          colors: {
            brand: 'red',
            brandAccent: 'darkred',
          },
        },
      },
    }}
  />
)
```

若您创建了自己的主题，则可能不需要覆盖任何主题。

### 创建自己的主题 {#create-theme}

您可以通过在外观中遵循相同的结构来创建自己的`appearance.theme`属性。
查看[主题内的标记](https://github.com/supabase-community/auth-ui/blob/main/packages/react/common/theming/Themes.tsx)列表.

```js title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'

const supabase = createClient(
  '<INSERT PROJECT URL>',
  '<INSERT PROJECT ANON API KEY>'
)

const customTheme = {
  default: {
    colors: {
      brand: 'hsl(153 60.0% 53.0%)',
      brandAccent: 'hsl(154 54.8% 45.1%)',
      brandButtonText: 'white',
      // ..
  },
  dark: {
    colors: {
      brandButtonText: 'white',
      defaultButtonBackground: '#2e2e2e',
      defaultButtonBackgroundHover: '#3e3e3e',
      //..
    },
  },
  // You can also add more theme variations with different names.
  evenDarker: {
    colors: {
      brandButtonText: 'white',
      defaultButtonBackground: '#1e1e1e',
      defaultButtonBackgroundHover: '#2e2e2e',
      //..
    },
  },
}

const App = () => (
  <Auth
    supabaseClient={supabase}
    theme="default" // can also be "dark" or "evenDarker"
    appearance={{ theme: customTheme}}
  />
)
```

您可以使用[`theme` prop](#switch-theme-variations)在主题的不同变体之间切换。

### 自定义CSS类 {#custom-css-classes}

您可以为以下元素使用自定义CSS类：
`"button"`, `"container"`, `"anchor"`, `"divider"`, `"label"`, `"input"`, `"loader"`, `"message"`.

```js title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'

const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')

const App = () => (
  <Auth
    supabaseClient={supabase}
    appearance={{
      className: {
        anchor: 'my-awesome-anchor',
        button: 'my-awesome-button',
        //..
      },
    }}
  />
)
```

### 自定义内联CSS {#custom-inline-styles}

您可以为以下元素使用自定义CSS内联样式：
`"button"`, `"container"`, `"anchor"`, `"divider"`, `"label"`, `"input"`, `"loader"`, `"message"`.

```js title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'

const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')

const App = () => (
  <Auth
    supabaseClient={supabase}
    appearance={{
      style: {
        button: { background: 'red', color: 'white' },
        anchor: { color: 'blue' },
        //..
      },
    }}
  />
)
```

### 自定义标签 {#custom-labels}

您可以将自定义标签与`localization.variables`一起使用。 参见[标签列表](https://github.com/supabase-community/auth-ui/blob/main/packages/react/common/lib/Localization/en.json) 

```js title=/src/index.js
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'

const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')

const App = () => (
  <Auth
    supabaseClient={supabase}
    //highlight-start
    localization={{
      variables: {
        sign_in: {
          email_label: 'Your email address',
          password_label: 'Your strong password',
        },
      },
    }}
    //highlight-end
  />
)
```


