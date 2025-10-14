- [目录结构](#目录结构)
- [front matter](#front%20matter)
- [右上角菜单](#右上角菜单)
- [md文件中的代码块](#md文件中的代码块)

官方示例网站：[https://imfing.github.io/hextra/zh-cn/](https://imfing.github.io/hextra/zh-cn/)
在 github pages 上部署：[https://imfing.github.io/hextra/zh-cn/docs/guide/deploy-site/](https://imfing.github.io/hextra/zh-cn/docs/guide/deploy-site/)
## 目录结构
```go
// Hextra 为不同类型的内容提供了三种布局：
// content/docs/	适合结构化文档
// content/blog/	用于博客文章，包含列表和详细文章视图
// 其他所有目录	    单页文章视图，无侧边栏

content
├── _index.md                         // <- /
├── docs
│   ├── _index.md                     // <- /docs/
│   ├── getting-started.md            // <- /docs/getting-started/
│   └── guide
│       ├── _index.md                 // <- /docs/guide/
│       └── organize-files.md         // <- /docs/guide/organize-files/
└── blog
    ├── _index.md // <- /blog/
    └── post-1.md // <- /blog/post-1/
```
## front matter
```go
---
# 设置 BreadCrumb 显示的内容
linkTitle: Foo Bar
# 隐藏 BreadCrumb
breadcrumbs: false
# 侧边栏的排序
weight: 2
---
```
## 右上角菜单
```yaml
menu:
  main:
    - name: 文档
      pageRef: /docs
      weight: 1
    - name: 博客
      pageRef: /blog
      weight: 2
    - name: 关于
      pageRef: /about
      weight: 3
    - name: 搜索
      weight: 4
      params:
        type: search
    - name: GitHub
      weight: 5
      url: "https://github.com/imfing/hextra"
      params:
        icon: github
```
## md文件中的代码块
```go
// 通过设置 filename 属性可为代码块添加文件名或标题：
// ```python {filename="hello.py"}
// def say_hello():
//     print("Hello!")
// ```


// 通过 base_url 属性可设置基础 URL，该 URL 会与 filename 组合生成可点击的链接
// ```go {base_url="https://github.com/",filename="candvert"}
// go 1.20
// ```


// 设置 linenos=table 可启用行号，并通过 linenostart 指定起始行号：
// ```python {linenos=table,linenostart=42}
// def say_hello():
//     print("Hello!")
// ```


// 通过 hl_lines 属性可高亮指定行号（支持数组格式）：
// ```python {linenos=table,hl_lines=[2,4],linenostart=1,filename="hello.py"}
// def say_hello():
//     print("Hello!")
// def main():
//     say_hello()
// ```
```