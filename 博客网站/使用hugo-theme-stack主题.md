- [推荐配置](#推荐配置)
- [目录结构](#目录结构)

- [front matter](#front%20matter)
- [hugo.yaml](#hugo.yaml)
- [网页左侧上半部分的内容](#网页左侧上半部分的内容)
- [网页右侧的内容](#网页右侧的内容)
- [设置主题](#设置主题)
- [设置网站为中文网站](#设置网站为中文网站)
- [分页功能](#分页功能)
- [网站底部的版权声明](#网站底部的版权声明)
- [日期格式](#日期格式)
- [评论功能](#评论功能)
- [多语言](#多语言)
- [自定义模板](#自定义模板)

官网示例网站：[https://dev.stack.jimmycai.com/](https://dev.stack.jimmycai.com/)
md 文件都放在 content/post 目录下
`content/_index.md` 和 content/page 目录下的 `index.md` 会显示为网页左侧的菜单
## 推荐配置
```go
// hugo.yaml
baseURL: https://example.org/
title: Candvert
theme: 'hugo-theme-stack'
copyright: All rights reserved

defaultContentLanguage: 'zh-cn'
languageCode: 'zh-cn'

hasCJKLanguage: true

pagination:
  pagerSize: 5

params:
  mainSections: ["post"]
  favicon: '/a.jpg'
  rssFullContent: true

  dateFormat:
    published: 2006-01-02
    lastUpdated: 2006-01-02 15:04

  sidebar:
    emoji: h g
    subtitle: one two
    avatar:
      enabled: true
      local: true
	  # 该图片一定要放在 assets 目录下，而不是 static 目录下
      src: b.png

  widgets:
    homepage:
      - type: search
      - type: archives
        params:
          limit: 5
      - type: categories
        params:
          limit: 10
      - type: tag-cloud
        params:
          limit: 10
    page:
      - type: toc

  colorScheme:
    toggle: true
    default: auto

  footer:
    since: 2019
    customText: ''

menu:
  social:
  - identifier: github
    name: GitHub
    url: https://github.com/CaiJimmy/hugo-theme-stack
    params:
      icon: brand-github

  - identifier: twitter
    name: Twitter
    url: https://twitter.com
    params:
      icon: brand-twitter





// 普通 md 文件
---
title: "docker"
description: 'just a brief'

date: "2024-01-22"
lastmod: '2024-09-15T11:30:03'

categories: ["web", "rust"]
tags: ["css", "html", "js"]

image: '/a.png'
---




// content/_index.md
// icon 的值是主题的 assets/icons 目录下的文件名
---
menu:
    main:
        name: 主页
        weight: -100
        params:
            icon: home
---


// page/about/index.md
// title 会和 menu: main: name 冲突，只能设置一个
---
title: 关于
date: '2019-02-28'
menu:
    main: 
        weight: -90
        params:
            icon: user
---
Written in Go


// page/archives/index.md
---
title: 归档
date: 2019-05-28
layout: "archives"
slug: "archives"
menu:
    main:
        weight: -70
        params: 
            icon: archives
---


// page/search/index.md
---
title: 搜索
slug: "search"
layout: "search"
outputs:
    - html
    - json
menu:
    main:
        weight: -60
        params: 
            icon: search
---


// page/links/index.md
---
title: 其他链接
menu:
    main: 
        weight: -50
        params:
            icon: link
comments: false

links:
  - title: GitHub
    description: good site
    website: https://github.com
    image: /a.jpg
  - title: TypeScript
    description: programming language
    website: https://www.typescriptlang.org
    image: /a.jpg
---
```
## 目录结构
```go
// content/_index.md 和 content/page 目录下的 index.md 会显示为网页左侧的菜单
content/
├── post/
│       ├── Javascript.md
│       ├── Typescript.md
│       ├── rust/
│       │   └── learnRust.md
│   	└── lua/
│           └── learnLua.md
├── page/
│       ├── about
│       │   └── index.md
│       ├── archives/
│       │   └── index.md
│       ├── search/
│       │   └── index.md
│       └── links/
│           └── index.md
└── _index.md
```
## front matter
```go
title: about
description = 'just a brief'

date = "2024-01-22"
lastmod = '2024-09-15T11:30:03'

categories: ["web", "rust"]
tags: ["css", "html", "js"]

image: '/a.png'

toc: true
readingTime: true
comments: true



// 网页左侧的菜单，只在 _index.md 和 index.md 文件中设置
menu: 
    main:
        name: title
        weight: -90
        params:
            icon: icon-name
```
## hugo.yaml
```yaml
# 如果 defaultContentLanguage 是 zh-cn、ja、ko，则设置为 true
# 以使 .WordCount 和 .Summary 正确工作
hasCJKLanguage: true


# 设置 post 和 page 页面的链接，:slug 为文件名
permalinks:
  post: /p/:slug/
  page: /:slug/


params:
  # 设置 content 目录下那些文件夹中的文档可以显示，值默认为 ["post"]
  mainSections: ["post"]
  favicon: '/a.jpg'
  rssFullContent: true

  article:
    # 是否显示阅读时间
    readingTime: false
	# 每篇文章下方的许可证
    license:
      enabled: true
      default: Licensed under CC BY-NC-SA 4.0
```
## 网页左侧上半部分的内容
```yaml
title: Candvert


params:
  sidebar:
    emoji: h g
    subtitle: one two
    avatar:
      enabled: true
      local: true
	  # 该图片一定要放在 assets 目录下，而不是 static 目录下
      src: b.png


menu:
  social:
  - identifier: github
    name: GitHub
    url: https://github.com/CaiJimmy/hugo-theme-stack
    params:
      icon: brand-github

  - identifier: twitter
    name: Twitter
    url: https://twitter.com
    params:
      icon: brand-twitter
```
## 网页右侧的内容
```yaml
# limit 限制显示多少项，archives 的 limit 是限制显示多少年的内容
params:
  widgets:
    homepage:
      - type: search
      - type: archives
        params:
          limit: 5
      - type: categories
        params:
          limit: 10
      - type: tag-cloud
        params:
          limit: 10
    page:
      - type: toc
```
## 设置主题
```yaml
params:
  colorScheme:
    # 切换模式按钮
    toggle: true
	# light|dark|auto
    default: auto
```
## 设置网站为中文网站
```yaml
defaultContentLanguage: 'zh-cn'
languageCode: 'zh-cn'
```
## 分页功能
```yaml
pagination:
  pagerSize: 5
```
## 网站底部的版权声明
```yaml
copyright: All rights reserved

params:
  footer:
    since: 2019
    customText: ''
```
## 日期格式
```yaml
params:
  dateFormat:
    published: 2006-01-02
    lastUpdated: 2006-01-02 15:04
```
## 评论功能
```yaml
services:
  disqus:
    shortname: "shortname"
```
## 多语言
```yaml
languages:
  en:
    languageName: English
    title: Example Site
    weight: 1
    params:
      sidebar:
        subtitle: Example description
  zh-cn:
    languageName: 中文
    title: 演示站点
    weight: 2
    params:
      sidebar:
      subtitle: 演示说明
```
## 自定义模板
```go
// 主题中有两个为自定义 HTML 保留的空文件：
layouts/partials/head/custom.html
layouts/partials/footer/custom.html





// 比如更换网站使用的字体
// 创建 layouts/partials/head/custom.html
<style>
    /* Overwrite CSS variable */
    :root {
        --article-font-family: "Merriweather", var(--base-font-family);
    }
</style>
<script>
    (function () {
        const customFont = document.createElement('link');
        customFont.href = "https://fonts.googleapis.com/css2?family=Merriweather:wght@400;700&display=swap";
    
        customFont.type = "text/css";
        customFont.rel = "stylesheet";
    
        document.head.appendChild(customFont);
    }());
</script>
```