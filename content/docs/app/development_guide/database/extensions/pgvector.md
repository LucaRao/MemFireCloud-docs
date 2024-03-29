---
    weight: 1535
    title: "pgvector: 嵌入向量和向量相似性"
    description: ""
    icon: "article"
    draft: false
    toc: true
---


[pgvector](https://github.com/pgvector/pgvector/) 是一款用于向量相似性搜索的 PostgreSQL 扩展。它还可以用于存储 [嵌入向量](https://supabase.com/blog/openai-embeddings-postgres-vector) 。

了解更多关于 Supabase 的 [AI & Vector](/docs/app/development_guide/ai/ai) 服务的信息。


## 概念

### 向量相似性

向量相似性是指衡量两个相关项之间相似程度的度量方式。例如，如果你有一组产品列表，你可以使用向量相似性来寻找相似的产品。为了实现这个目标，你需要使用数学模型将每个产品转换为由数字组成的"向量"。对于文本、图像和其他类型的数据，你可以使用类似的模型。一旦所有这些向量都存储在数据库中，你就可以使用向量相似性来查找相似的项。




## 使用方法

### 启用扩展

{{< tabs tabTotal="2" >}}


{{% tab tabName="控制台" %}}



1. 跳转控制台的 **数据库** 页面。
2. 点击侧栏中的 **扩展** 。
3. 搜索 "vector" 并启用扩展。



{{% /tab %}}
{{% tab tabName="SQL" %}}



```sql
 -- Example: enable the "vector" extension.
create extension vector
with
  schema extensions;

-- Example: disable the "vector" extension
drop
  extension if exists vector;
```

尽管 SQL 代码是 `create extension`，但它的等效操作是“启用扩展”。
要禁用扩展，您可以调用 `drop extension`。



{{% /tab %}}

{{< /tabs >}}


### 创建一个表来存储向量

```sql
create table posts (
  id serial primary key,
  title text not null,
  body text not null,
  embedding vector(1536)
);
```

### 存储一个向量
在这个示例中，我们将使用OpenAI API客户端生成一个向量，然后使用Supabase客户端将其存储在数据库中。

```js
const title = 'First post!'
const body = 'Hello world!'

// Generate a vector using OpenAI
const embeddingResponse = await openai.createEmbedding({
  model: 'text-embedding-ada-002',
  input: body,
})

const [{ embedding }] = embeddingResponse.data.data

// Store the vector in Postgres
const { data, error } = await supabase.from('posts').insert({
  title,
  body,
  embedding,
})
```

## 更多关于pgvector和Supabase资源的信息。

- [Supabase Clippy：用于Supabase文档的ChatGPT](https://supabase.com/blog/chatgpt-supabase-docs)
- [使用pgvector在Postgres中存储OpenAI嵌入](https://supabase.com/blog/openai-embeddings-postgres-vector)
- [使用Supabase Edge Runtime构建的ChatGPT插件模板](https://supabase.com/blog/building-chatgpt-plugins-template)
- [构建自定义ChatGPT风格文档搜索的模板](https://github.com/supabase-community/nextjs-openai-doc-search)


