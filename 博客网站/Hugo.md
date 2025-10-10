- [下载](#下载)
- [命令](#命令)
- [archetypes](#archetypes)
- [模板](#模板)
- [如何给不同文件选择模板](#如何给不同文件选择模板)
- [模块](#模块)
- [hugo-book主题](#hugo-book主题)

Hugo 是一个用 Go 编写的静态网站生成器
## 下载
```sh
到 https://github.com/gohugoio/hugo/releases 下载
Windows 下载 hugo_extended_xxx_windows-amd64 的

解压后只需要 hugo.exe 文件
可以将 hugo.exe 添加到 path
也可以不添加到 path 直接在命令行用.\hugo xxx
```
## 命令
```sh
查看版本
hugo version

帮助
hugo help
hugo server --help

新建项目，新建项目后要先进入项目目录进行 git init 后才能添加主题
hugo new site <project_name>

添加主题
git submodule add https://github.com/alex-shpak/hugo-book.git themes/hugo-book

在 content 目录创建一个 md 文件
hugo new content content/posts/my-first-post.md

启动服务器
hugo server
-D 选项表示也渲染 front matter 中 draft = true 的文件
hugo server -D

构建网站
hugo
```
## archetypes
```go
// archetypes 就是使用 hugo new content 命令创建新文件时填充文件内容的模板

// 默认的 archetype：
+++
date = '{{ .Date }}'
draft = true
title = '{{ replace .File.ContentBaseName `-` ` ` | title }}'
+++


// 可以为不同的 content types 创建 archetype
archetypes/
├── default.md
└── posts.md
// 比如对于该命令 hugo new content posts/my-first-post.md，查找顺序为：
// 1. archetypes/posts.md
// 2. archetypes/default.md
// 3. themes/my-theme/archetypes/posts.md
// 4. themes/my-theme/archetypes/default.md

// archetype 具有这些上下文：Date、File、Type、Site
```
## 模板
```go
模板就是一个文件，位于 layouts 文件夹中
Hugo 使用 Go 的 text/template 和 html/template 包
页面模板接收一个 Page 对象


HTML 模板示例：
{{ $v1 := 6 }}
{{ $v2 := 7 }}
<p>The product of {{ $v1 }} and {{ $v2 }} is {{ mul $v1 $v2 }}.</p>


在模板中，点.代表当前上下文（下面代码位于 layouts/page.html）
<h2>{{ .Title }}</h2>
在该示例中，点代表 Page 对象，我们调用它的 Titile 方法返回 front matter 中的 title
当前上下文可能会在模板内发生变化，比如将上下文重新绑定到另一个值或对象，例如下面的 range 和 with 块中
<h2>{{ .Title }}</h2>
{{ range slice "foo" "bar" }}
  <p>{{ . }}</p>
{{ end }}
{{ with "baz" }}
  <p>{{ . }}</p>
{{ end }}
上面代码渲染出的结果
<h2>My Page Title</h2>
<p>foo</p>
<p>bar</p>
<p>baz</p>
在 range 和 with 块中可以使用 $ 来访问传入该模板的上下文
{{ with "foo" }}
  <p>{{ $.Title }} - {{ . }}</p>
{{ end }}
上面代码渲染出的结果
<p>My Page Title - foo</p>

通过连字符移除渲染出的空格，换行
{{- $convertToLower := true -}}

通过管道向函数或方法传递参数
{{ strings.ToLower "Hugo" }}
{{ "Hugo" | strings.ToLower }}
将一个函数或方法的结果通过管道传输到另一个
{{ strings.TrimSuffix "o" (strings.ToLower "Hugo") }}
{{ "Hugo" | strings.ToLower | strings.TrimSuffix "o" }}
下面两行效果相同
{{ mul 6 (add 2 5) }}
{{ 5 | add 2 | mul 6 }}

将模板操作拆分为两行或多行
{{ $v := or $arg1 $arg2 }}
{{ $v := or 
  $arg1
  $arg2
}}
将原始字符串拆分为两行或更多行
{{ $msg := "This is line one.\nThis is line two." }}
{{ $msg := `This is line one.
This is line two.`
}}


初始化变量
在 if、range 或 with 块内初始化的变量的作用域为该块
{{ $total := 3 }}
对于表示切片或映射的变量，使用索引函数返回所需的值
{{ $slice := slice "foo" "bar" "baz" }}
{{ index $slice 2 }} → baz

{{ $map := dict "a" "foo" "b" "bar" "c" "baz" }}
{{ index $map "c" }} → baz
不使用索引
{{ $map := dict "a" "foo" "b" "bar" "c" "baz" }}
{{ $map.c }}

{{ $homePage := .Site.Home }}
{{ $homePage.Title }}
如上所示，对象和方法名称均大写


注释
{{/*
This is a block comment.
*/}}

{{- /*
This is a block comment with
adjacent whitespace removed.
*/ -}}


通过如下方式渲染 HTML 注释
{{ "<!-- I am an HTML comment. -->" | safeHTML }}
{{ printf "<!-- This is the %s site. -->" .Site.Title | safeHTML }}




可以在 layouts/_partials 目录中创建 partial templates
```
## 如何给不同文件选择模板
```go
// 假如有该目录
content/
├── about.md
└── contact.md
// about.md 文件
+++
title = 'About'
type = 'miscellaneous'
+++
// contact.md 文件
+++
layout = 'contact'
title = 'Contact'
type = 'miscellaneous'
+++
// 则下面 page 文件夹下的 contact.html 会渲染 contact.md，sigle.html 会渲染 about.md
// miscellaneous 文件夹下的文件为布局
layouts/
└── page/
    └── contact.html  <-- renders contact.md
    └── single.html   <-- renders about.md
└── miscellaneous/
    └── contact.html  <-- renders contact.md
    └── single.html   <-- renders about.md
```
## 模块
```go
Hugo 模块允许你将网站功能拆分为独立、可复用的单元
一个 Hugo 模块本质上是一个独立的单元，能够提供 Hugo 所支持的七种组件类型中的一种或多种：​​静态文件​​（static）、​​内容​​（content）、​​布局模板​​（layouts）、​​数据文件​​（data）、​​资源文件​​（assets）、​​国际化文件​​（i18n）和​​内容原型​​（archetypes）。
你的Hugo项目本身也是一个模块。通过灵活的挂载配置，你可以将这些来自不同模块的组件组合起来，形成一个大型的虚拟联合文件系统，而无需改变原有仓库的目录结构

也就是说如果我的 Hugo 项目我不想编写 i18n 或其他文件夹中的内容，我可以直接使用 github 上的


比如使用主题模块：
先输入该命令：hugo mod init github.com/<your_user>/<your_project>
再在 hugo.toml 文件中添加：
[module]
  [[module.imports]]
    path = 'github.com/<your_user>/<your_project>'
```
## hugo-book主题
```sh
md 文件放置于 content/docs 目录下
# (可选) 如果您使用Google Analytics来跟踪网站数据，请设置此项
# 请始终将此配置项置于配置文件顶部，否则将无法正常工作
googleAnalytics = "UA-XXXXXXXXX-X"

# (可选) 如果您提供了Disqus的shortname，将在所有页面上启用评论功能
disqusShortname = "my-site"

# (可选) 如果您在文件名中使用大写字母，请将此设置为true
disablePathToLower = true

# (可选) 将此设置为true可在'doc'类型页面上启用'Last Modified by'日期和git作者信息
enableGitInfo = true

# (可选) 本主题专为文档使用而设计，因此不渲染 taxonomy
# 您可以使用以下配置移除相关文件
disableKinds = ['taxonomy', 'taxonomyTerm']

[params]
  # (可选, 默认light) 设置颜色主题: light(浅色), dark(深色) 或 auto(自动)
  # 'auto'主题会根据浏览器/操作系统偏好自动切换深色和浅色模式
  BookTheme = 'light'

  # (可选, 默认true) 控制页面右侧目录的可见性
  # 起始和结束级别可以通过markup.tableOfContents设置进行控制
  # 您也可以在页面front matter中单独设置此参数
  BookToC = true

  # (可选, 默认无) 设置书籍logo的路径。如果logo位于 /static/logo.png，则路径应为'logo.png'
  BookLogo = 'logo.png'

  # (可选, 默认'favicon.png') 设置网站的favicon路径
  # 如果favicon位于/static/custom.svg，则路径应为'custom.svg'
  BookFavicon = 'favicon.png'

  # (可选, 默认docs) 指定要渲染为菜单的内容部分
  # 您也可以将值设置为"*"以将所有部分渲染到菜单中
  BookSection = 'docs'

  # (可选, 默认无) 设置页面提交链接的模板。需要启用enableGitInfo
  # 启用后将在页面页脚显示'最后修改'和指向提交的链接
  # 此参数作为模板执行，使用.Site、.Page和.GitInfo作为上下文
  BookLastChangeLink = 'https://github.com/alex-shpak/hugo-book/commit/{{ .GitInfo.Hash }}'

  # (可选, 默认无) 设置编辑页面链接的模板
  # 启用后将在页面页脚显示'编辑此页面'链接
  # 此参数作为模板执行，使用.Site、.Page和.Path作为上下文
  BookEditLink = 'https://github.com/alex-shpak/hugo-book/edit/main/exampleSite/{{ .Path }}'

  # (可选, 默认'January 2, 2006') 配置页面上使用的日期格式
  # - In git information
  # - In blog posts
  # https://gohugo.io/functions/time/format/
  BookDateFormat = 'January 2, 2006'

  # (可选, 默认true) 启用flexsearch搜索功能
  # 索引是即时构建的，因此可能会减慢您的网站速度
  # 索引配置可以按语言在i18n文件夹中进行调整
  BookSearch = true

  # (可选, 默认true) 在页面上启用评论模板
  # 默认情况下partials/docs/comments.html包含Disqus模板
  # 参见 https://gohugo.io/content-management/comments/#configure-disqus
  # 可以通过页面frontmatter中的相同参数进行覆盖
  BookComments = true













# Set type to 'docs' if you want to render page outside of configured section or if you render section other than 'docs'
type = 'docs'

# 设置页面权重以重新排列文件树菜单中的项目顺序
weight = 10

# (可选) 设置为'true'可将页面标记为文件树菜单中的扁平章节
bookFlatSection = false

# (可选) 设置为隐藏该级别的嵌套章节或页面。仅适用于文件树菜单模式
bookCollapseSection = true

# (可选) 设置为true可从侧边菜单中隐藏页面或章节
bookHidden = false

# (可选) 设置为'false'可隐藏页面中的目录(ToC)
bookToC = true

# (可选) 如果您已为网站启用了BookComments，可以针对特定页面禁用它
bookComments = true

# (可选) 设置为'true'可将页面从搜索索引中排除
bookSearchExclude = false

# (可选) 为菜单中此页面设置明确的href属性
bookHref = ''
```