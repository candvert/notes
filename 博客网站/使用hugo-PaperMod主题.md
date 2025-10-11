- [文件结构示例](#文件结构示例)
- [配置文件config.yml示例](#配置文件config.yml示例)
- [md文件的front matter示例](#md文件的front%20matter示例)
- [切换主题](#切换主题)
- [删除切换主题按钮](#删除切换主题按钮)
## 文件结构示例
```go
.(site root)
├── configTaxo.yml
├── config.yml
├── content
│   ├── archives.fr.md
│   ├── archives.md
│   ├── posts
│   │   ├── emoji-support.md
│   │   ├── markdown-syntax.fa.md
│   │   ├── markdown-syntax.fr.md
│   │   ├── markdown-syntax.md
│   │   ├── math-typesetting.md
│   │   ├── papermod
│   │   │   ├── _index.md
│   │   │   ├── papermod-faq.md
│   │   │   ├── papermod-features
│   │   │   │   ├── images
│   │   │   │   │   ├── homeinfo.jpg
│   │   │   │   │   ├── profile.jpg
│   │   │   │   │   └── regular.jpg
│   │   │   │   └── index.md
│   │   │   ├── papermod-icons.md
│   │   │   ├── papermod-installation.md
│   │   │   └── papermod-variables.md
│   │   ├── placeholder-text.md
│   │   └── rich-content.md
│   ├── search.fr.md
│   ├── search.md
│   └── tags
├── LICENSE
├── README.md
├── resources
│   └── _gen
│       ├── assets
│       └── images
├── static
│   ├── android-chrome-192x192.png
│   ├── android-chrome-512x512.png
│   ├── apple-touch-icon.png
│   ├── favicon-16x16.png
│   ├── favicon-32x32.png
│   ├── favicon.ico
│   └── papermod-cover.png
└── themes
    └── hugo-PaperMod
```

```yaml
# If your site is in 'https', then make sure your base url isn't written using 'http' otherwise your sitemap would
# contain http (as opposeed to https) URLs. This would affect Google indexing of your URLs.
baseURL: "https://adityatelange.github.io/hugo-PaperMod/"
title: PaperMod
copyright: "© [PaperMod Contributors](https://github.com/adityatelange/hugo-PaperMod/graphs/contributors)"
theme: [hugo-PaperMod]

enableInlineShortcodes: true
enableRobotsTXT: true
buildDrafts: false
buildFuture: false
buildExpired: false
enableEmoji: true
pygmentsUseClasses: true
mainsections: ["posts", "papermod"]

minify:
  disableXML: true
  # minifyOutput: true

pagination:
  disableAliases: false
  pagerSize: 5

languages:
  en:
    languageName: "English"
    weight: 1
    taxonomies:
      category: categories
      tag: tags
      series: series
    menu:
      main:
        - name: Archive
          url: archives
          weight: 5
        - name: Search
          url: search/
          weight: 10
        - name: Tags
          url: tags/
          weight: 10
        - name: WiKi
          url: https://github.com/adityatelange/hugo-PaperMod/wiki/

  fr:
    languageName: ":fr:"
    weight: 2
    title: PaperModL2
    taxonomies:
      category: FRcategories
      tag: FRtags
      series: FRseries
    menu:
      main:
        - name: Archive
          url: archives/
          weight: 5
        - name: FRTags
          url: frtags
          weight: 10
        - name: FRCategories
          url: frcategories
          weight: 10
        - name: FRSeries
          url: frseries
          weight: 10
        - name: NullLink
          url: "#"

    # custom params for each language should be under [langcode].parms - hugo v0.120.0
    params:
      languageAltTitle: French
      profileMode:
        enabled: true
        title: PaperMod
        imageUrl: "https://raw.githubusercontent.com/googlefonts/noto-emoji/master/svg/emoji_u1f9d1_1f3fb_200d_1f4bb.svg"
        imageTitle: ProfileMode image
        # imageWidth: 120
        # imageHeight: 120
        subtitle: "☄️ Fast | ☁️ Fluent | 🌙 Smooth | 📱 Responsive"
        buttons:
          - name: Blog
            url: posts
          - name: Profile Mode
            url: https://github.com/adityatelange/hugo-PaperMod/wiki/Features#profile-mode

  fa:
    languagedirection: rtl
    weight: 3
    title: PaperMod RTL
    taxonomies:
      category: FAcategories
      tag: FAtags
      series: FAseries
    menu:
      main:
        - name: FATags
          url: fatags
          weight: 10
    # custom params for each language should be under [langcode].parms - hugo v0.120.0
    params:
      homeInfoParams:
        Title: "Hi there \U0001F44B"
        Content: Welcome to RTL layout

outputs:
  home:
    - HTML
    - RSS
    - JSON

params:
  env: production # to enable google analytics, opengraph, twitter-cards and schema.
  description: "Theme PaperMod - https://github.com/adityatelange/hugo-PaperMod"
  author: Theme PaperMod
  # author: ["Me", "You"] # multiple authors

  defaultTheme: auto
  # disableThemeToggle: true
  ShowShareButtons: true
  ShowReadingTime: true
  # disableSpecial1stPost: true
  displayFullLangName: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowCodeCopyButtons: true
  ShowRssButtonInSectionTermList: true
  ShowAllPagesInArchive: true
  ShowPageNums: true
  ShowToc: true
  # comments: false
  images: ["images/papermod-cover.png"]

  profileMode:
    enabled: false
    title: PaperMod
    imageUrl: "#"
    imageTitle: my image
    # imageWidth: 120
    # imageHeight: 120
    buttons:
      - name: Archives
        url: archives
      - name: Tags
        url: tags

  homeInfoParams:
    Title: "PaperMod's Demo"
    Content: >
      👋 Welcome to demo page of Hugo's theme PaperMod!

      - **PaperMod**  is designed to be clean and simple but fast and responsive theme with useful feature-set that enhances UX.

      - Feel free to show your support by giving us a star 🌟 on GitHub and sharing with your friends and social media .

      - PaperMod is based on theme [Paper](https://github.com/nanxiaobei/hugo-paper/tree/4330c8b12aa48bfdecbcad6ad66145f679a430b3).

  socialIcons:
    - name: github
      title: View Source on Github
      url: "https://github.com/adityatelange/hugo-PaperMod"
    - name: Discord
      title: Join discord community
      url: "https://discord.gg/ahpmTvhVmp"
    - name: X
      title: Share PaperMod on X/Twitter
      url: "https://x.com/intent/tweet/?text=Checkout%20Hugo%20PaperMod%20%E2%9C%A8%0AA%20fast,%20clean,%20responsive%20Hugo%20theme.&url=https://github.com/adityatelange/hugo-PaperMod&hashtags=Hugo,PaperMod"
    - name: KoFi
      title: Buy me a Ko-Fi :)
      url: "https://ko-fi.com/adityatelange"

  editPost:
    URL: "https://github.com/adityatelange/hugo-PaperMod/tree/exampleSite/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link

  # label:
  # iconSVG: '<svg xmlns="http://www.w3.org/2000/svg" height="25" viewBox="0 -960 960 960" fill="currentColor"><path d="M320-240h320v-80H320v80Zm0-160h320v-80H320v80ZM240-80q-33 0-56.5-23.5T160-160v-640q0-33 23.5-56.5T240-880h320l240 240v480q0 33-23.5 56.5T720-80H240Zm280-520v-200H240v640h480v-440H520ZM240-800v200-200 640-640Z"/></svg>'
  # text: "Home"
  # icon: icon.png
  # iconHeight: 35

  # analytics:
  #     google:
  #         SiteVerificationTag: "XYZabc"

  assets:
    disableHLJS: true
  #     favicon: "<link / abs url>"
  #     favicon16x16: "<link / abs url>"
  #     favicon32x32: "<link / abs url>"
  #     apple_touch_icon: "<link / abs url>"
  #     safari_pinned_tab: "<link / abs url>"

  # cover:
  #     hidden: true # hide everywhere but not in structured data
  #     hiddenInList: true # hide on list pages and home
  #     hiddenInSingle: true # hide on single page

  # fuseOpts:
  #     isCaseSensitive: false
  #     shouldSort: true
  #     location: 0
  #     distance: 1000
  #     threshold: 0.4
  #     minMatchCharLength: 0
  #     keys: ["title", "permalink", "summary", "content"]

markup:
  goldmark:
    renderer:
      unsafe: true
  highlight:
    noClasses: false
    # anchorLineNos: true
    # codeFences: true
    # guessSyntax: true
    # lineNos: true
    # style: monokai

# privacy:
#   vimeo:
#     disabled: false
#     simple: true

#   twitter:
#     disabled: false
#     enableDNT: true
#     simple: true

#   instagram:
#     disabled: false
#     simple: true

#   youtube:
#     disabled: false
#     privacyEnhanced: true

services:
  instagram:
    disableInlineCSS: true
  x:
    disableInlineCSS: true
```
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