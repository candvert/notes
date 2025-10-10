## 基本步骤
首先在命令行输入 `./hugo new site my_project`
然后创建 layouts/home.html 文件
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
layouts/home.html 渲染主页的内容
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
// 则访问主页时，渲染出的是 baseof.html 中的 {{ block "main" . }}{{ end }} 替换为 <h1>Home</h1> 后的结果
{{ define "main" }}
  <h1>What home</h1>
{{ end }}



// 其他模板要应用基本模板需要满足以下两个条件：
//	  必须包含至少一个 define action
//	  只能包含 define action、空格和注释
// 如果模板不满足所有这些条件，Hugo 将严格按照提供的内容执行，而不应用基本模板
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
// .Title 表示 front matter 中的 title 字段
// .Content 表示 md 文档的内容



// 然后便可访问 http://localhost:1313/posts/first
// 也就是说如果创建 content/a.md，那么它的路由就是 http://localhost:1313/a
```