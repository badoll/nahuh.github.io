---
title: "Hugo 搭建"
date: 2023-04-24T17:34:53+08:00
tags:
  - hugo
---

# 页面管理
## page layout

## post meta
themes文件夹下面的layouts/partials找到post_meta.html
```xml
{{ $date := .Date.Format "02.01.2006" }}
{{ $lastmod := .Lastmod.Format "02.01.2006" }}
{{- if ne $lastmod $date -}}
{{- $scratch.Add "meta" (slice (printf "<span title='%s'>(updated %s)</span>" (.Lastmod) (.Lastmod | time.Format (default "January 2, 2006" site.Params.DateFormat)))) }}
{{- end }}
```

# 资源管理
page bundle 用于单page的资源管理，属于该页的资源都放在这个文件夹
branch bundle 适用于页面下有嵌套子页
https://blog.darkthread.net/blog/hugo-figures/