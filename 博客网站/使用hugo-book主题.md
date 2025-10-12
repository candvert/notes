- [hugo.toml推荐配置](#hugo.toml推荐配置)
- [`_index.md`文件推荐配置](#`_index.md`文件推荐配置)
- [普通md文件推荐配置](#普通md文件推荐配置)

- [可以在hugo.toml中设置的选项](#可以在hugo.toml中设置的选项)
- [可以在front matter中设置的选项](#可以在front%20matter中设置的选项)
- [用来覆盖主题的模板](#用来覆盖主题的模板)
- [其他自定义部分](#其他自定义部分)

默认 content/docs 目录下的文件会被渲染成网页左侧的树状菜单，可以在 config.toml 中设置 BookSection 来决定使用哪个文件夹生成树状菜单

content/docs 目录下的文件夹中要有 `_index.md` 文件才能将该文件夹中的文件显示在树状菜单中

`_index.md` 文件中只有 front matter 时在树状菜单中不可点击，如果其还含有内容，则可以在树状菜单中点击

可以在 front matter 中设置 title 和 weight 来调整其在树状菜单中显示的标题和顺序

评价：该主题可设置内容极少，美观性一般，懒得折腾的话可以使用，使用推荐配置即可
## hugo.toml推荐配置
```toml
baseURL = 'https://example.org/'
title = 'Candvert'
theme = 'hugo-book'

defaultContentLanguage = 'zh'
languageCode = 'zh-cn'

disableKinds = ['taxonomy']

[params]
  BookTheme = 'auto'
  BookLogo = '/a.jpg'
  BookFavicon = '/a.jpg'
```
## `_index.md`文件推荐配置
```go
+++
title = 'HUGO'
weight = 1
bookFlatSection = true
+++
```
## 普通md文件推荐配置
```go
// 只推荐设置 title 和 weight
+++
title = 'hugo'
+++
```
## 可以在hugo.toml中设置的选项
```toml
# (可选) 如果您使用Google Analytics来跟踪网站数据，请设置此项
# 请始终将此配置项置于配置文件顶部，否则将无法正常工作
googleAnalytics = "UA-XXXXXXXXX-X"

# (可选) 如果您提供了Disqus的shortname，将在所有页面上启用评论功能
disqusShortname = "my-site"

# 如果文件名是 Neovim，那么该文件链接就是 http://localhost:1313/docs/Neovim/
# (可选) 如果不设置该选项，如果文件名是 Neovim，链接就是 http://localhost:1313/docs/neovim/，也就是链接变为小写
disablePathToLower = true

# (可选) 将此设置为true可在'doc'类型页面上启用'Last Modified by'日期和git作者信息
enableGitInfo = true

# (可选) 本主题专为文档使用而设计，因此不渲染 taxonomy
# 您可以使用以下配置移除相关文件
disableKinds = ['taxonomy']

[params]
  # (可选, 默认light) 设置颜色主题: light(浅色), dark(深色) 或 auto(自动)
  # 'auto'主题会根据浏览器/操作系统偏好自动切换深色和浅色模式
  BookTheme = 'light'

  # (可选, 默认无) 设置书籍logo的路径。如果logo位于 /static/logo.png，则路径应为'logo.png'
  BookLogo = 'logo.png'

  # (可选, 默认'favicon.png') 设置网站的favicon路径
  # 如果favicon位于/static/custom.svg，则路径应为'custom.svg'
  BookFavicon = 'favicon.png'

  # (可选, 默认docs) 指定要渲染为菜单的内容部分
  # 您也可以将值设置为"*"以将所有部分渲染到菜单中
  BookSection = 'docs'

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

  # (可选, 默认无) 设置页面提交链接的模板。需要启用enableGitInfo
  # 启用后将在页面页脚显示'最后修改'和指向提交的链接
  # 此参数作为模板执行，使用.Site、.Page和.GitInfo作为上下文
  BookLastChangeLink = 'https://github.com/alex-shpak/hugo-book/commit/{{ .GitInfo.Hash }}'

  # (可选, 默认无) 设置编辑页面链接的模板
  # 启用后将在页面页脚显示'编辑此页面'链接
  # 此参数作为模板执行，使用.Site、.Page和.Path作为上下文
  BookEditLink = 'https://github.com/alex-shpak/hugo-book/edit/main/exampleSite/{{ .Path }}'
```
## 可以在front matter中设置的选项
```go
---
# 设置 type 为 'docs' 可以将不是 content/docs 目录下的文档也渲染出来
type = 'docs'

# 设置页面权重以重新排列文件树菜单中的项目顺序
weight = 1

# (可选) 在树状菜单中隐藏
bookHidden = true

# (可选) 用于 _index.md 文件，将 _index.md 文件 front matter 中设置的 title 粗体显示在树状菜单中
bookFlatSection = true

# (可选) 用于 _index.md 文件，点击切换折叠/展开
bookCollapseSection = true

# (可选) 如果您已为网站启用了BookComments，可以针对特定页面禁用它
bookComments = false

# (可选) 将页面从搜索索引中排除
bookSearchExclude = true

# (可选) 点击时会跳转到设置的链接
bookHref = '/ok'
---
```
## 用来覆盖主题的模板
```go
layouts/partials/docs/inject/head.html            在 </head> 之前
layouts/partials/docs/inject/body.html	          在 </body> 之前
layouts/partials/docs/inject/footer.html	      在 footer 内容之后
layouts/partials/docs/inject/menu-before.html	  在 <nav> 菜单块的开头
layouts/partials/docs/inject/menu-after.html	  在 <nav> 菜单块的末尾
layouts/partials/docs/inject/content-before.html  在页面内容之前
layouts/partials/docs/inject/content-after.html	  在页面内容之后
layouts/partials/docs/inject/toc-before.html	  在目录块的开头
layouts/partials/docs/inject/toc-after.html	      在目录块的末尾
```
## 其他自定义部分
```go
static/favicon.png	           覆盖默认 favicon
assets/_custom.scss	           自定义或覆盖 scss 样式
assets/_variables.scss	       覆盖默认 SCSS 变量
assets/_fonts.scss	           用自定义字体替换默认字体
assets/mermaid.json	           替换 Mermaid 初始化配置
assets/katex.json	           替换 KaTeX 初始化配置
```
