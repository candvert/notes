- [推荐配置](#推荐配置)
- [目录结构](#目录结构)

- [md文件的front matter](#md文件的front%20matter)
- [配置文件hugo.yaml中的一些选项](#配置文件hugo.yaml中的一些选项)
- [添加自定义css](#添加自定义css)
- [切换主题](#切换主题)
- [删除切换主题按钮](#删除切换主题按钮)
- [设置网站为中文网站](#设置网站为中文网站)
- [主页欢迎内容](#主页欢迎内容)
- [顶部导航栏右侧](#顶部导航栏右侧)
- [主页欢迎内容下面的图标](#主页欢迎内容下面的图标)
- [分页功能](#分页功能)
- [网站底部的版权声明](#网站底部的版权声明)
- [归档页面](#归档页面)
- [搜索页面](#搜索页面)
- [在每篇文章底部显示分享按钮](#在每篇文章底部显示分享按钮)
- [显示阅读时间](#显示阅读时间)
- [每篇文章显示目录](#每篇文章显示目录)
- [每篇文章显示BreadCrumb](#每篇文章显示BreadCrumb)
- [每篇文章的编辑链接](#每篇文章的编辑链接)
- [每篇文章底部添加上一个/下一个按钮](#每篇文章底部添加上一个/下一个按钮)
- [可以复制代码](#可以复制代码)
- [添加评论功能](#添加评论功能)
- [主页设置为Profile模式](#主页设置为Profile模式)

官方示例网站：[https://adityatelange.github.io/hugo-PaperMod/](https://adityatelange.github.io/hugo-PaperMod/)
使用该主题记得用 `hugo new site <project_name> --format yaml` 命令来生成项目
评价：该主题是 Star 数最多的，自定义选项较多，使用推荐配置即可
## 推荐配置
```go
// hugo.yaml 文件配置
baseURL: https://candvert.github.io/
title: Candvert博客
theme: ["hugo-PaperMod"]

defaultContentLanguage: "zh"
languageCode: "zh-cn"

copyright: "© [Candvert](https://github.com/candvert)"

pagination:
  disableAliases: false
  pagerSize: 5

params:
  defaultTheme: auto

  ShowReadingTime: true
  ShowBreadCrumbs: true
  ShowWordCount: true
  ShowPostNavLinks: true
  ShowCodeCopyButtons: true
  ShowPageNums: true
  ShowRssButtonInSectionTermList: true
  showtoc: true

  comments: true

  homeInfoParams:
    Title: "欢迎"
    Content: "开始阅读文章吧！"

  socialIcons:
    - name: bilibili
      title: View bilibili
      url: "https://bilibili.com/"
    - name: github
      title: View Source on Github
      url: "https://github.com/candvert"

  label:
    icon: /a.jpg
    iconHeight: 35

  assets:
    disableHLJS: true

markup:
  highlight:
    noClasses: false
    lineNos: true
    style: monokai

outputs:
  home:
    - HTML
    - RSS
    - JSON

services:
  disqus:
    shortname: candvert

menu:
  main:
    - name: 归档
      url: archives
      weight: 5
    - name: 搜索
      url: search/
      weight: 10
    - name: 标签
      url: tags/
      weight: 10
    - name: Github
      url: https://github.com/adityatelange/hugo-PaperMod/wiki/

caches:
  images:
    dir: :cacheDir/images




// 普通 md 文件配置
---
title: "Bun"
date: 2024-09-15
tags: ["Web"]
description: "bun简介"
---

// content/posts/_index.md
---
title: "index"
---

// content/archives.md
---
title: "Archive"
layout: "archives"
url: "/archives/"
summary: archives
---

// content/search.md
---
title: "Search"
layout: "search"
summary: "search"
placeholder: "placeholder text in search input box"
---

// assets/css/extended/custom.css
code {
	font-size: 1em !important;
}
```
## 目录结构
```go
// md 文件都放在 posts 目录下
// posts 目录下要有 _index.md 文件
content/
├── posts/
│       ├── _index.md
│       ├── rust/
│       │   └── learnRust.md
│   	└── lua/
│           └── learnLua.md
├── archives.md     // 实现归档页面
└── search.md       // 实现搜索页面



// _index.md 文件内容
---
title: "index"
---

// archives.md 文件内容
---
title: "Archive"
layout: "archives"
url: "/archives/"
summary: archives
---

// search.md 文件内容
---
title: "Search"
layout: "search"
summary: "search"
placeholder: "placeholder text in search input box"
---
```
## md文件的front matter
```go
// 标题
title: "My 1st post"

// 日期格式
date: 2024-09-15
date: 2024-09-15T11:30:03-02:00
// 2020-09-15 是日期部分
// T 是日期和时间的分隔符
// 11:30:03​​ 是时间部分
// -02:00 是时区偏移量，表示比协调世界时（UTC）晚2小时

// 将帖子固定/显示在列表顶部
weight: 1

// 作者
author: "Me"
// 多作者
author: ["Me", "You"]

// 标签
tags: ["first"]

// 显示阅读时间
ShowReadingTime: true

// 隐藏 2024年9月26日·1 分钟·2 字·Me 部分
hidemeta: true

// 摘要
description: "Desc Text."

// 隐藏摘要
hideSummary: true

// 显示 BreadCrumbs
ShowBreadCrumbs: true

// 显示单词数
ShowWordCount: true

// 永久的重定向链接，当访问 yoursite.com/first 时会重定向到该博客的当前 URL
aliases: ["/first"]

// 文档封面
cover:
    image: "/images/a.png"
    alt: "md file" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
	responsiveImages: false # optional
	linkFullImages: true # optional

// 显示编辑链接
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link

// 显示目录
showToc: true
// 目录默认关闭
TocOpen: false

// 显示如 Disqus 等的评论区，需要额外设置，详见“添加评论功能”部分
comments: true

// 禁用 highlight.js 代码高亮
disableHLJS: true

// 禁止分享
disableShare: false

// 当前文章的内容不参与搜索
searchHidden: true

// 添加上一篇/下一篇帖子建议
ShowPostNavLinks: true

// 当前文章是否为草稿
draft: false

// 归档页面显示 RSS 按钮
ShowRssButtonInSectionTermList: true

// 使用 Hugo 目录
UseHugoToc: true

// 搜索引擎 SEO 优化
canonicalURL: "https://canonical.url/to/page"
```
## 配置文件hugo.yaml中的一些选项
```yaml
# 这些选项也可以在front matter中设置，front matter设置的优先级更高
params:
  ShowReadingTime: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowWordCount: true
  ShowRssButtonInSectionTermList: true
  UseHugoToc: true
  comments: true
  hidemeta: false
  hideSummary: false
  showtoc: false
  tocopen: false

# 配置文件中独有的
params:
  ShowShareButtons: true
  ShowCodeCopyButtons: true
  disableSpecial1stPost: false
  disableScrollToTop: true
  ShowPageNums: true
  ShowAllPagesInArchive: true

# 顶部导航栏左侧的图标
params:
  label:
    icon: /a.png
    iconHeight: 35

# 使用 Hugo 的代码高亮器 chroma
params:
  assets:
    disableHLJS: true # 禁用 PaperMod 主题的 Highlight.js
markup:
  highlight:
    noClasses: false
    lineNos: true # 显示行号
	lineNoStart: 1 # 起始行号
    style: monokai

# 搜索页面的相关设置
params:
  fuseOpts:
    isCaseSensitive: false
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    limit: 10 # refer: https://www.fusejs.io/api/methods.html#search
    keys: ["title", "permalink", "summary", "content"]
```
## 添加自定义css
```go
// 创建 assets/css/extended/custom.css
code {
	font-size: 1em !important;
}
```
## 切换主题
```yml
params:
  # defaultTheme: light
  # defaultTheme: dark
  defaultTheme: auto # 跟随浏览器主题
```
## 删除切换主题按钮
```yml
params:
  disableThemeToggle: true
```
## 设置网站为中文网站
```yaml
defaultContentLanguage: "zh"
languageCode: "zh-cn"
```
## 主页欢迎内容
```yaml
params:
  homeInfoParams:
    Title: "Welcome"
    Content: "Welcome to demo page of Hugo's theme PaperMod!"
```
## 顶部导航栏右侧
```yaml
# weight 的作用是控制条目的位置
languages:
  zh:
    languageName: ":zh:"
    weight: 1
    taxonomies:
      category: categories
      tag: tags
      series: series
    menu:
      main:
        - name: 归档
          url: archives/
          weight: 5
        - name: 搜索
          url: search/
          weight: 10
        - name: 标签
          url: tags/
          weight: 10
        - name: Github
          url: https://github.com/adityatelange/hugo-PaperMod/wiki/



# 不设置多语言的话可以直接这样设置
menu:
  main:
    - name: 归档
      url: archives/
      weight: 5
    - name: 搜索
      url: search/
      weight: 10
    - name: 标签
      url: tags/
      weight: 10
```
## 主页欢迎内容下面的图标
```yaml
params:
  socialIcons:
    - name: github
      title: View Source on Github
      url: "https://github.com/adityatelange/hugo-PaperMod"
    - name: Discord
      title: Join discord community
      url: "https://discord.gg/ahpmTvhVmp"
    - name: X
      title: Share PaperMod on X/Twitter
      url: "https://x.com"
    - name: KoFi
      title: Buy me a Ko-Fi :)
      url: "https://ko-fi.com/adityatelange"
```
## 分页功能
```yaml
pagination:
  disableAliases: false
  pagerSize: 5

params:
  ShowPageNums: true # 显示一共有多少页
```
## 网站底部的版权声明
```yaml
copyright: "© [Candvert](https://github.com/candvert)"
```
## 归档页面
```go
// 需要创建 content/archives.md 文件
---
title: "Archive"
layout: "archives"
url: "/archives/"
summary: archives
---
```
## 搜索页面
```go
// 在配置文件中添加：
outputs:
  home:
    - HTML
    - RSS
    - JSON # necessary for search


// 创建 content/search.md 文件
---
title: "Search" # in any language you want
layout: "search" # necessary for search
# url: "/archive"
# description: "Description for Search"
summary: "search"
placeholder: "placeholder text in search input box"
---


// 要使某个 md 文档不被搜索，在front matter中添加
searchHidden: true
```
## 在每篇文章底部显示分享按钮
```yaml
params:
  ShowShareButtons: true
```
## 显示阅读时间
```yaml
params:
  ShowReadingTime: true
```
## 每篇文章显示目录
```yaml
params:
  ShowToc: true
  TocOpen: true # 默认为展开
```
## 每篇文章显示BreadCrumb
```yaml
params:
  ShowBreadCrumbs: true


# 对于每个 md 文档可在front matter中禁止
# ShowBreadCrumbs: false
```
## 每篇文章的编辑链接
```yaml
params:
  editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link


# 在front matter中单独设置
---
editPost:
  URL: "https://github.com/<path_to_repo>/content"
  Text: "Suggest Changes" # edit text
  appendFilePath: true # to append file path to Edit link
---
```
## 每篇文章底部添加上一个/下一个按钮
```yaml
params:
  ShowPostNavLinks: true
```
## 可以复制代码
```yaml
params:
  ShowCodeCopyButtons: true
```
## 添加评论功能

使用的是 Disqus
进入该页面
![](/images/hugo_01.png)

下拉到底部
![](/images/hugo_02.png)

复制这段代码
![](/images/hugo_03.png)

创建 layouts/partials/comments.html 文件，将复制的代码添加到文件里
```html
<div id="disqus_thread"></div>
<script>
    (function() {
    var d = document, s = d.createElement('script');
    s.src = 'https://candvert.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
```
在 config.yml 中添加
```yaml
params:
  comments: true

services:
  disqus:
    shortname: <your_shortname>
```
对于每个 md 文档可以在 front matter 中设置是否开启评论
```go
---
comments: false
---
```
## 主页设置为Profile模式
```yaml
params:
  profileMode:
    enabled: true
    title: "<Title>" # optional default will be site title
    subtitle: "This is subtitle"
    imageUrl: "/images/a.png" # optional
    imageTitle: "<title of image as alt>" # optional
    imageWidth: 120 # custom size
    imageHeight: 120 # custom size
    buttons:
      - name: Archive
        url: "/archive"
      - name: Github
        url: "https://github.com/"

  socialIcons: # optional
    - name: "<platform>"
      url: "<link>"
    - name: "<platform 2>"
      url: "<link2>"
```