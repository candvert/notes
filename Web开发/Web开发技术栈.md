```sh
Web 开发离不开 JavaScript 引擎
目前最流行的两个 JavaScript 引擎是 V8（由 Google 开发）和 JavaScriptCore（由 Apple 开发）。两者都是开源的。
V8 是 Google 的开源高性能 JavaScript 和 WebAssembly 引擎，采用 C++ 编写。它被 Chrome、Node.js 等浏览器所使用。它实现了 ECMAScript 和 WebAssembly。V8 可以嵌入到任何 C++ 应用程序中。


Nodejs 基于 V8 引擎，于 2009 年发布，是一个开源、跨平台的 JavaScript 运行时环境

Deno 是一个 JavaScript、TypeScript 和 WebAssembly 运行时，于 2020 年发布，它基于 V8、Rust。Deno 运行时原生支持 TypeScript、JSX 和现代 ECMAScript 功能，无需任何配置。构建、测试和部署应用程序所需的基本工具均已开箱即用。


Fresh 是 Deno 官方开发的全栈框架

Nextjs 全栈框架的后端使用 Nodejs，前端使用 React


时间 2025/09，全栈推荐使用 Nextjs，因为 Fresh 还远未成熟。后端推荐 Deno，因为其是取代 Nodejs 的新一代 JavaScript 运行时。
```

```sh
时间 2025/09，开发网页最推荐 Next.js，使用 Typescript 语言和 Tailwind 样式，使用 Shadcn UI 组件库，页面布局使用 Tailwind 中的 flex 和 grid

Vscode 中 Javascript 和 Typescript 的插件使用 ESLint，Prettier

UI 组件库有 Shadcn UI，Material UI，Ant Design。推荐使用 Shadcn UI

桌面应用使用 Electron，其可以使用前端框架 React 进行开发

移动应用使用 React Native

CSS 使用 Tailwind
```