- [基本步骤](#基本步骤)
- [layouts目录](#layouts目录)
- [全局布局](#全局布局)
- [创建md文档](#创建md文档)
- [taxonomy.html](#taxonomy.html)
- [section.html](#section.html)
- [`_index.md`作用](#`_index.md`作用)
- [内层的page.html](#内层的page.html)
- [`_partials`](#`_partials`)
- [`_markup`](#`_markup`)
- [`_shortcodes`](#`_shortcodes`)
## 基本步骤
在命令行输入 `./hugo new site my_project`
创建 layouts/home.html 文件
```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>{{ .Site.Title }}</title>
</head>
<body>
    <h1>欢迎</h1>
</body>
</html>
```
在命令行输入 `./hugo server`
访问 http://localhost:1313/
## layouts目录
```go
layouts 目录下的文件被称为模板
layouts/home.html 渲染主页
layouts/404.html 渲染 404 页面
```
## 全局布局
```go
// 全局布局通过 layouts/baseof.html 实现，baseof.html 被称为基本模板
// 基本模板中的 {{ block "main" . }}{{ end }} 部分会被其他模板中的 {{ define "main" }} {{ end }} 部分替换
// 假设有该 baseof.html
<!DOCTYPE html>
<html lang="zh">
<head>
  <title>MySite</title>
</head>
<body>
  <header>
    <h1>Header</h1>
  </header>
  <main>
    {{ block "main" . }}{{ end }}
  </main>
  <footer>
    <h1>Footer</h1>
  </footer>
</body>
</html>

// 假设有该 home.html
{{ define "main" }}
  <h1>What home</h1>
{{ end }}
// 则访问主页时，渲染出的是 baseof.html 中的 {{ block "main" . }}{{ end }} 替换为 <h1>Home</h1> 后的结果



// 其他模板要应用基本模板必须要包含 define
// 如果模板不包含 define，则不会应用基本模板
```
## 创建md文档
```go
// 在命令行输入 ./hugo new content content/posts/first.md
// 或者手动在 content/posts 目录下创建 first.md 文件
// 假设 first.md 有如下内容
+++
title = 'first'
+++
## Go
great


// 常规内容页面会使用 layouts/page.html 渲染
// 创建 layouts/page.html 文件，输入如下内容：
{{ define "main" }}
  {{ .Title }}
  {{ .Content }}
{{ end }}
// .Title 表示 md 文档中 front matter 中的 title 字段
// .Content 表示 md 文档的内容



// 然后便可访问 http://localhost:1313/posts/first
// 也就是说如果创建 content/a.md，那么它的路由就是 http://localhost:1313/a
```
## taxonomy.html
```go
// taxonomy.html 的作用是渲染分类页面

// 首先在 hugo.toml 中定义分类方式，下面两种分类是默认存在的
[taxonomies]
  category = 'categories'
  tag = 'tags'
// 然后创建 taxonomy.html 文件
{{ define "main" }}
  <h1>taxonomy</h1>
{{ end }}
// 此时便可访问 http://localhost:1313/categories 和 http://localhost:1313/tags 了



// 在 md 文档的 front matter 中使用上述的分类
+++
categories = ['vegetarian', 'gluten-free']
title = 'Example'
+++


// 然后在 taxonomy.html 文件中
{{ define "main" }}
  {{ range .Pages }}
    <h2><a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a></h2>
  {{ end }}
{{ end }}

// 此时访问 http://localhost:1313/categories 便会出现 vegetarian 和 gluten-free 的链接
// 需要创建 term.html 来渲染 vegetarian 和 gluten-free 链接的网站




// 在 hugo.toml 中添加如下内容禁用 taxonomy 和 term 页面
disableKinds = ['taxonomy', 'term']
```
## section.html
```go
一个 section 就是 content 目录的第一级目录或者任何包含 _index.md 文件的目录

content/
├── articles/             <-- section (top-level directory)
│       └── article/
│           ├── cover.jpg
│           └── index.md
└── products/             <-- section (top-level directory)
    └── product/          <-- section (has _index.md file)
        ├── benefits/     <-- section (has _index.md file)
        │   ├── _index.md
        │   └── benefit.md
        └── _index.md

articles/article 目录不是 section


当访问 content/articiles 时会优先使用 layouts/articles/section.html 进行渲染，这就是 section.html 的作用
如果不存在 layouts/articles/section.html，则会使用 layouts/section.html，如果没有 layouts/section.html 则会使用 layouts/list.html
```
## `_index.md`作用
```go
// _index.md 在 Hugo 中扮演着特殊角色。它允许你向 home、section、taxonomy、term 页面添加 front matter 和 content

// 比如有该目录结构
content/
├── articles/
│       └── article/
│           ├── cat.md
│           └── _index.md
└── _index.md

// 则在 layouts/articles/section.html 中可以访问 content/articiles/_index.md 中的内容
{{ define "main" }}
  {{ .Content }}
{{ end }}

// 在 layouts/home.html 中可以访问 content/_index.md 中的内容
```
## 内层的page.html
```go
// content/books/flower.md 优先使用 layouts/books/page.html 渲染，然后才是 layouts/page.html
```
## `_partials`
```go
// 可以将一个较长的模板文件分成几个较小的模板文件
// 通过 layouts/_partials 目录下的模板实现

// 比如 home.html
{{ define "main" }}
  {{ partial "header.html" . }}
  <h1>Content</h1>
  {{ partial "footer.html" . }}
{{ end }}
// {{ partial "header.html" . }} 部分会被相应的 layouts/_partials/header.html 中的内容替换
```
## `_markup`
```go
// layouts/_markup 目录下的文件是 render hook 模板
// 功能是覆盖 Markdown 到 HTML 的转换
// 比如将 md 文件中的标题转换为 html 的 <h1><span>GOOD</span></h1>，则创建 render-heading.html 文件：
<h1>
  <span>GOOD</span>
</h1>



// 存在下面这些种类的 render hook 模板
// 比如 render-heading.html 就是用于 <h1> 到 <h6> 的
layouts/
  └── _markup/
      ├── render-blockquote.html
      ├── render-codeblock.html
      ├── render-heading.html
      ├── render-image.html
      ├── render-link.html
      ├── render-passthrough.html
      └── render-table.html

// 可以为不同的页面类型创建 render hook 模板
layouts/
├── books/
│   └── _markup/
│       ├── render-link.html
│       └── render-link.rss.xml
└── films/
    └── _markup/
        ├── render-link.html
        └── render-link.rss.xml
```
## `_shortcodes`
```go
// layouts/_shortcodes 目录下的文件是 shortcode 模板
// 其内容在 md 文件中调用

// 假如有 _shortcodes/audio.html：
{{ with resources.Get (.Get "src") }}
  <audio controls preload="auto" src="{{ .RelPermalink }}"></audio>
{{ end }}

// 则可以在 md 文件中调用：
{{< audio src=/audio/test.mp3 >}}
```