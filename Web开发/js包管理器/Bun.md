- [安装](#安装)
- [运行单个文件](#运行单个文件)
- [新建项目](#新建项目)
- [安装和删除依赖](#安装和删除依赖)

Bun 旨在替代 Node.js。
可以直接执行 .jsx、.ts 和 .tsx 文件。
Bun 不仅仅是一个运行时。它的长期目标是成为一个用于构建 JavaScript/TypeScript 应用程序的紧密结合的基础架构工具包，包括包管理器、转译器、打包器、脚本运行器、测试运行器等。
## 安装
```sh
# PowerShell (Windows):
powershell -c "irm bun.sh/install.ps1|iex"
```
## 运行单个文件
```sh
bun index.ts
```
## 新建项目
```sh
bun init <project_name>
```
## 安装和删除依赖
```sh
bun add zod
bun remove zod
```