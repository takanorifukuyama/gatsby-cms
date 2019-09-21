---
templateKey: blog-post
title: GatsbyCMSはじめました。
date: 2019-09-18T01:49:29.202Z
description: start up gatsby cms!!
featuredpost: false
featuredimage: /img/download.png
tags:
  - gatsby
  - react
image: /img/download.png
---
[こちらの記事](https://shibe97.com/blog/gatsby-netlify-cms/)を参考にして、とりあえずOGP設定とworkspaceを設定してみた！

あと、そのままだとコードが見づらいので

 `gatsby-remark-prismjs`

というプラグインを入れてみる。Themeは[Prismjs](https://prismjs.com/)から選べばいい。

- - -

ソースコード
ファイルの該当部分だけ載せます

<https://github.com/takanorifukuyama/gatsby-cms>

src/static/admin/config.yml

```yaml:title=src/static/admin/config.yml
...

collections:
  - name: "blog"
    label: "Blog"
    folder: "src/pages/blog"
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
    fields:
      - {label: "Template Key", name: "templateKey", widget: "hidden", default: "blog-post"}
      - {label: "Title", name: "title", widget: "string"}
      - {label: "Publish Date", name: "date", widget: "datetime"}
      - {label: "Description", name: "description", widget: "text"}
      - {label: "Featured Post", name: "featuredpost", widget: "boolean"}
      - {label: "Featured Image", name: "featuredimage", widget: image}
      - {label: "Body", name: "body", widget: "markdown"}
      - {label: "Tags", name: "tags", widget: "list"}
      # OGP
      - {label: "Image", name: "image", widget: "image"}

...
```

/static/templates/blog-post.js

```javascript
...

const BlogPost = ({ data }) => {
  const { markdownRemark: post } = data

  return (
    <Layout>
      <BlogPostTemplate
        content={post.html}
        contentComponent={HTMLContent}
        description={post.frontmatter.description}
        helmet={
          <Meta post={post} />
        }
        tags={post.frontmatter.tags}
        title={post.frontmatter.title}
      />
    </Layout>
  )
}

// 追加
const Meta = ({ post }) => {
  const origin = 'https://thisistakanori.netlify.com';

  return (
    <Helmet
      title={`${post.frontmatter.title} | Blog`}
      meta={[
        { name: 'description', content: post.frontmatter.description },
        { property: 'og:title', content: post.frontmatter.title },
        { property: 'og:description', content: post.frontmatter.description },
        { property: 'og:image', content: `${origin}${post.frontmatter.image}` },
      ]}
    />
  );
};

...
```
---
シンタックスハイライトの設定
```
module.export = {
  ....
  plugins:[
    {
      resolve: 'gatsby-transformer-remark',
      options: {
        plugins: [
          {
            resolve: 'gatsby-remark-prismjs',
            options: {
              classPrefix: "language-",
              inlineCodeMarker: null,
              aliases: {},
              showLineNumbers: true,
              noInlineHighlight: false,
            },
          },
        ],
      }
    }
  ],
}
