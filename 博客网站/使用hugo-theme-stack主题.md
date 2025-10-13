- [hugo.yaml](#hugo.yaml)
- [设置网站为中文网站](#设置网站为中文网站)
- [分页功能](#分页功能)
- [网站底部的版权声明](#网站底部的版权声明)

官网示例网站：[https://dev.stack.jimmycai.com/](https://dev.stack.jimmycai.com/)
md 文件都放在 post 目录下
## 目录结构
```go
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


// content/_index.md 和 page 目录下的 index.md 会显示为网页左侧的菜单


// content/_index.md 内容
// icon 是主题的 assets/icons 目录下的文件名
---
menu:
    main:
        name: 主页
        weight: -100
        params:
            icon: home
---

// page/about/index.md 内容
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

// page/archives/index.md 内容
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

// page/search/index.md 内容
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

// page/links/index.md 内容
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
## front matter
```go
---
title: About
description: 'ok'
date: '2019-02-28'
aliases:
  - about-us
  - about-hugo
  - contact
license: CC BY-NC-ND
lastmod: '2020-10-09'
image: '/a.png'
toc: true
readingTime: true
comments: true
keywords: ['web', 'backend']
style: 


menu: 
    main:
        name: title (optional)
        weight: -90
        params:
            icon: icon-name
---
```
## hugo.yaml
```yaml
# 如果 DefaultContentLanguage 是 zh、ja、ko，则设置为 true
# 以使 .WordCount 和 .Summary 正确工作
hasCJKLanguage: true

params:
  description: 'ok'
  # 值默认为 ["post"]
  mainSections: ["post"]
  featuredImageField: '/a.png'
  rssFullContent: true
  favicon: '/a.png'
	
  article:
    toc: true
	readingTime: true
	license:
      enabled: true
      default: Licensed under CC BY-NC-SA 4.0
	enabled: true

related:
    includeNewer: true
    threshold: 60
    toLower: false
    indices:
        - name: tags
          weight: 100

        - name: categories
          weight: 200
```
## 网页左侧上半部分的内容
```yaml
title: Candvert

params:
  sidebar:
    emoji: 'e'
    subtitle: 'hi'
    avatar:
      enabled: true
      local: true
      src: b.png

menu:
  main: []
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
	default: 'light'
```
## 设置网站为中文网站
```yaml
defaultContentLanguage: 'zh-cn'
languageCode: 'zh-cn'
```
## 分页功能
```yaml
pagination:
  pagerSize: 3
```
## 网站底部的版权声明
```yaml
copyright: All rights reserved

params:
  footer:
    since: 2019
    customText: ''
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

```go
layouts/partials/head/custom.html
layouts/partials/footer/custom.html
```