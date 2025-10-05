## 下载
```sh
到 https://github.com/gohugoio/hugo/releases 下载
Windows 下载 hugo_extended_xxx_windows-amd64 的

解压后只需要 hugo.exe 文件
可以将 hugo.exe 添加到 path
也可以不添加到 path 在命令行直接用.\hugo xxx
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
添加文件
hugo new content content/posts/my-first-post.md
启动服务器
hugo server
hugo server -D
构建网站
hugo
```
## 基本使用
```sh
必须在 hugo.toml 中指定下面三项
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'

如果要使用主题，在 hugo.toml 中添加
theme = 'hugo-book'

在 content/posts 目录中添加博客，比如有 content/posts/go.md 文件，则访问 http://localhost:1313/posts/go 便可访问该博客
```
## hugo.toml
```yaml
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = 'hugo-book'


可配置的键有：HTTPCache, build, caches, cascade, contentTypes, deployment, frontmatter, imaging, languages, markup, mediaTypes, menus, minify, module, outputFormats, outputs, page, pagination, params, permalinks, privacy, related, security, segments, server, services, sitemap, taxonomies, uglyURLs
```
## 文件目录结构
```
assets 目录包括图像、CSS、Sass、JavaScript 和 TypeScript 等资源
config 目录包含您的站点配置，可能分为多个子目录和文件。对于配置最少的项目，项目根目录中名为 hugo.toml 的单个配置文件就足够了
content 目录包含构成您网站内容的标记文件（通常为 Markdown）和页面资源
data 目录包含用于扩充内容、配置、本地化和导航的数据文件（JSON、TOML、YAML 或 XML）
i18n 目录包含多语言网站的翻译表
layouts 目录包含用于将内容、数据和资源转换为完整网站的模板
public 目录包含已发布的网站，该目录在您运行 hugo 或 hugo server 命令时生成
static 目录包含您在构建网站时将被复制到公共目录的文件。例如：favicon.ico、robots.txt 以及用于验证网站所有权的文件。
themes 目录包含一个或多个主题，每个主题都有自己的子目录
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