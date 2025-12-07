- [下载](#下载)
- [命令](#命令)
- [archetypes](#archetypes)
- [模板语法](#模板语法)

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
配置文件使用 yaml 格式
hugo new site hug --format yaml

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
## 模板语法
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
