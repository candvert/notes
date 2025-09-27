- [安装](#安装)
- [运行单个文件](#运行单个文件)
- [新建项目](#新建项目)
- [安装和删除依赖](#安装和删除依赖)
- [运行测试](#运行测试)
- [格式化文件](#格式化文件)
- [HTTP服务器](#HTTP服务器)

Deno 是一个 JavaScript、TypeScript 和 WebAssembly 运行时。Deno 运行时原生支持 TypeScript、JSX 和现代 ECMAScript 功能，无需任何配置。构建、测试和部署应用程序所需的基本工具均已开箱即用。

Deno 提供了一个用 TypeScript 编写的标准库
包和标准库使用 [JSR](https://jsr.io/) 仓库
## 安装
```sh
# PowerShell (Windows):
irm https://deno.land/install.ps1 | iex
```
## 运行单个文件
```sh
deno run main.ts
```
## 新建项目
```sh
deno init <project_name>

# 文件目录结构
# deno.json 是配置文件
# main_test.ts 是测试文件
# main.ts 是主文件
<project_name>
├── deno.json
├── main_test.ts
└── main.ts

# Deno 还支持 package.json 文件，以兼容 Node.js 项目
```
## 安装和删除依赖
```sh
deno add jsr:@std/fs
deno remove @std/fs

# 安装 npm 包
deno add npm:mysql
```
## 运行测试
```sh
deno test
```
## 格式化文件
```sh
# 格式化当前目录所有文件
deno fmt
```
## HTTP服务器
```ts
// main.ts 文件中写入如下内容
Deno.serve(handler);
// handler 函数返回当前目录下的 index.html
async function handler(request: Request): Promise<Response> {
  const url = new URL(request.url);
  // 处理根路径，返回HTML页面
  if (url.pathname === "/" && request.method === "GET") {
    const html = await Deno.readTextFile("./index.html");
    return new Response(html, {
      headers: { "content-type": "text/html" },
    });
  }
  return new Response("Not Found", { status: 404 });
}




// 运行
// deno run --allow-net --allow-read main.ts

// 可以指定端口
Deno.serve({ port: 4242 }, handler);
```