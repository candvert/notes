- [md文件的front matter示例](#md文件的front%20matter示例)
- [md文件的front matter示例](#md文件的front%20matter示例)
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
- [主页设置为Profile模式](#主页设置为Profile模式)

官方示例网站：[https://adityatelange.github.io/hugo-PaperMod/](https://adityatelange.github.io/hugo-PaperMod/)
## md文件的front matter示例
```go
---
title: "My 1st post"
date: 2024-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["first"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
```

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


// 隐藏摘要
hideSummary: true


ShowBreadCrumbs: true


// 显示单词数
ShowWordCount: true

// 文档封面
cover:
    image: "/images/a.png"
    alt: "md file"
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
	responsiveImages: false # optional
	linkFullImages: true # optional
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
## posts目录下要有`_index.md`文件
## 顶部导航栏右侧
```yaml
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
```
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


# 在front matter中禁止
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
## 每篇文章添加上一个/下一个按钮
```yaml
params:
  ShowPostNavLinks: true
```
## 复制代码按钮
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
## 配置文件和front matter都可以设置的
```yaml
# front matter设置的优先级更高
# 如果front matter没有设置，则使用配置文件的设置
params:
  ShowReadingTime: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowWordCount: true
  ShowRssButtonInSectionTermList: true
  UseHugoToc: true
  comments: false
  hidemeta: false
  hideSummary: false
  showtoc: false
  tocopen: false



disableScrollToTop: false
disableSpecial1stPost: false
ShowCodeCopyButtons: false
ShowShareButtons: true
```
