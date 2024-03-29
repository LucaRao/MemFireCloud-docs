---
    weight: 1603
    title: "生成类型"
    description: "How to generate types for your API and Supabase libraries."
    icon: "article"
    draft: false
    toc: true
---

SubaseAPI是从数据库生成的，这意味着我们可以使用数据库来生成类型安全的API定义。

## 使用Suabase CLI生成类型

Suabase CLI是一个二进制Go应用程序，它提供了设置本地开发环境所需的一切。

你可以通过npm或其他支持的软件包管理器[安装CLI](https://www.npmjs.com/package/supabase)。CLI的最低要求版本是[v1.8.1](https://github.com/supabase/cli/releases)。


```bash
npm i supabase@">=1.8.1" --save-dev
```

用你的个人访问令牌登录:

```bash
npx supabase login
```

 为你的项目生成类型，产生`types/supabase.ts`文件:

```bash
npx supabase gen types typescript --project-id "$PROJECT_ID" --schema public > types/supabase.ts
```

在你生成了你的类型后，你可以在`src/index.ts`中使用它们.

```tsx
import { NextApiRequest, NextApiResponse } from 'next'
import { createClient } from '@supabase/supabase-js'
import { Database } from '../types/supabase'

const supabase = createClient<Database>(
  process.env.NEXT_PUBLIC_SUPABASE_URL,
  process.env.SUPABASE_SECRET_KEY
)

export default async (req: NextApiRequest, res: NextApiResponse) => {
  const allOnlineUsers = await supabase.from('users').select('*').eq('status', 'ONLINE')
  res.status(200).json(allOnlineUsers)
}
```

## 用GitHub动作自动更新类型

让你的类型定义与数据库保持同步的一个方法是设置一个GitHub动作，按计划运行。

将上面的脚本添加到你的`package.json`中，使用`npm run update-types`运行它。


```json
"update-types": "npx supabase gen types typescript --project-id \"$PROJECT_ID\" > types/supabase.ts"
```

创建一个文件 `.github/workflows/update-types.yml` ，用下面的片段来定义动作和环境变量。这个脚本将每天晚上提交新的类型变化到你的 repo。

```yaml
name: Update database types

on:
  schedule:
    # sets the action to run daily. You can modify this to run the action more or less frequently
    - cron: '0 0 * * *'

jobs:
  update:
    runs-on: ubuntu-latest
    env:
      SUPABASE_ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
      PROJECT_ID: <your-project-id>
    steps:
      - uses: actions/checkout@v2
        with:
          persist-credentials: false
          fetch-depth: 0
      - uses: actions/setup-node@v2.1.5
        with:
          node-version: 16
      - run: npm run update-types
      - name: check for file changes
        id: git_status
        run: |
          echo "::set-output name=status::$(git status -s)"
      - name: Commit files
        if: ${{contains(steps.git_status.outputs.status, ' ')}}
        run: |
          git add types/database/index.ts
          git config --local user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git config --local user.name "github-actions[bot]"
          git commit -m "Update database types" -a
      - name: Push changes
        if: ${{contains(steps.git_status.outputs.status, ' ')}}
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: ${{ github.ref }}
```

或者，你也可以使用社区支持的GitHub行动:[generate-supabase-db-types-github-action](https://github.com/lyqht/generate-supabase-db-types-github-action)

## 资源

- [用GitHub动作生成Supabase类型](https://blog.esteetey.dev/how-to-create-and-test-a-github-action-that-generates-types-from-supabase-database)


