---
title: "Hugo 博客搭建"
date: 2023-04-24T17:34:53+08:00
categories:
  - blog
tags:
  - hugo
---

# hugo 目录结构

```
.
├── README.md
├── archetypes
│   └── default.md
├── assets
├── config.yaml
├── content
│   ├── archives.md
│   └── posts
├── data
├── layouts
│   └── partials
├── public
│   ├── 404.html
│   ├── archives
│   ├── assets
│   ├── categories
│   ├── index.html
│   ├── index.xml
│   ├── page
│   ├── posts
│   ├── sitemap.xml
│   └── tags
├── resources
│   └── _gen
├── static
└── themes
    └── PaperMod
```

## archetype
可在`default.md`中修改默认的post的meta模版，在使用 hugo new post 命令时可以自动生成 title、date、tags 等 meta 信息。
```/yaml
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
tags:
  - test
---
```

## content
存放页面的目录，content 下的一级子目录看作一个对应的 section 内容分类区 content section。比如，为博客设置一个 blog 目录，为文章设置一个 articles 目录，为教程设置一个 tutorials 目录等。

```
.
└── content
    └── about
    |   └── index.md       // <- https://example.com/about/
    ├── posts
    |   ├── _index.md      // <- https://example.com/posts/
    |   ├── index.md       // <- https://example.com/posts/
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost/
    |   ├── happy
    |   |   └── ness.md    // <- https://example.com/posts/happy/ness/
    |   └── secondpost.md  // <- https://example.com/posts/secondpost/
    └── quote
        ├── first.md       // <- https://example.com/quote/first/
        └── second.md      // <- https://example.com/quote/second/
```

# 页面管理
## 主题
当前使用的主题是[PaperMod](https://github.com/adityatelange/hugo-PaperMod)

### 如何添加文章更新时间
themes文件夹下面的layouts/partials找到post_meta.html  
layout的优先级：
```
1. layouts/partials/*<PARTIALNAME>.html
2. themes/<THEME>/layouts/partials/*<PARTIALNAME>.html
```
可以将themes中的`post_meta.html`文件copy一份到`layouts/partials/`下进行修改，添加下面几行即可
```xml
{{ $date := .Date.Format "02.01.2006" }}
{{ $lastmod := .Lastmod.Format "02.01.2006" }}
{{- if ne $lastmod $date -}}
{{- $scratch.Add "meta" (slice (printf "<span title='%s'>(updated %s)</span>" (.Lastmod) (.Lastmod | time.Format (default "January 2, 2006" site.Params.DateFormat)))) }}
{{- end }}
```

# 资源管理
如果想要分页面管理图片、视频等资源，可以使用[Page Bundle](https://gohugo.io/content-management/page-bundles/)的方式来管理页面
```
content/
└── posts/
    └── post-1/           <-- page bundle
        ├── index.md
        └── sunset.jpg    <-- page resource
```
Page Bundle 分为 Leaf Bundle 和 Branch Bundle。  
Leaf Bundle 用于单页的资源管理，属于该页的资源都放在这个文件夹。  
Branch Bundle 适用于页面下有嵌套子页，文件夹下还可以嵌套 Leaf Bundle 或 Branch Bundle。  