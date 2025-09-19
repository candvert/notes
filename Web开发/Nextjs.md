- [添加自定义字体next/font](#添加自定义字体next/font)
- [添加图片next/image](#添加图片next/image)
- [文件系统路由Routes](#文件系统路由Routes)
- [文件系统api](#文件系统api)
- [Link组件](#Link组件)
- [Server Components](#Server%20Components)
- [loading.tsx](#loading.tsx)
- [Route groups](#Route%20groups)
- [React Suspense](#React%20Suspense)
- [use client](#use%20client)
- [layout文件](#layout文件)

- [处理md/mdx文件](#处理md/mdx文件)

nextjs 具有基于文件的路由，基于文件的 api （即GET，POST等请求），基于文件的 MD/MDX （将 md/mdx 文件解析为组件并显示）
## 添加自定义字体next/font
```typescript
// 当浏览器最初以系统字体呈现文本，然后在字体网络请求并加载后将其换成自定义字体时，就会发生布局转变。可能会导致文本大小、间距或布局发生变化，并移动周围的元素。
// 使用 next/font 模块时，Next.js 会自动优化应用程序中的字体。它会在构建时下载字体文件，并将其与其他静态资源一起托管。这意味着当用户访问您的应用程序时，不会有影响性能的额外字体网络请求。

// import Inter字体，/app/ui/fonts.ts
import { Inter } from 'next/font/google';
export const inter = Inter({ subsets: ['latin'] });
// 使用字体，/app/layout.tsx
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```
## 添加图片next/image
```typescript
// <Image> 组件带有自动图像优化功能，例如：
// 加载图像时自动防止布局偏移。
// 调整图像大小以避免将大图像传送到视口较小的设备。
// 默认情况下延迟加载图像（图像进入视口时加载）。
// 当浏览器支持时，以现代格式（如 WebP 和 AVIF）提供图像。

import Image from 'next/image';
export default function Page() {
  return (
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
  );
}
// 为了避免布局偏移，最好设置图片的宽度和高度。应该具有与源图像相同的纵横比。
// class hidden用于从移动屏幕的 DOM 中删除图像，而 md:block 用于在桌面屏幕上显示图像。
```
## 文件系统路由Routes
Next.js 使用文件系统路由，其中​​文件夹用于创建嵌套路由。每个文件夹代表一个映射到 URL 段的路由段。
![](/images/nextjs1.avif)
您可以使用 layout.tsx 和 page.tsx 文件为每条路线创建单独的 UI。
page.tsx 是一个特殊的 Next.js 文件，它导出一个 React 组件，并且它是让路由可访问所必需的。
![](/images/nextjs2.avif)
/app/dashboard/page.tsx 与 /dashboard 路径关联。
```typescript
// Next.js的路由使用文件夹表示的，要使路由可以访问，则该文件夹下必须要有page.tsx文件
```

在 Next.js 中，您可以使用特殊的 layout.tsx 文件来创建多个页面共享的 UI。也就是说下面的 page.tsx、customers/page.tsx、invoices/page.tsx 这三个页面共享布局  layout.tsx，可以在该布局中添加诸如导航栏，侧边栏等组件。
![](/images/nextjs3.avif)
<Layout /> 组件（即 layout.tsx 文件）接收一个 children prop。这个子组件可以是一个页面，也可以是另一个布局。
/app/layout.tsx 称为根布局，每个 Next.js 应用程序都需要它。添加到根布局的任何 UI 都将在应用程序的所有页面之间共享。
## 文件系统api
```ts
// Route Handlers 仅在 app 目录内可用
// Route Handlers 在 app 目录内的 route.js|ts 文件中定义：
// route.js 文件不能与 page.js 文件位于同一路由，也就说 URL 不能相同
// 支持以下 HTTP 方法：GET、POST、PUT、PATCH、DELETE、HEAD 和 OPTIONS

// 使用
import { cookies } from 'next/headers'
 
export async function GET(request: NextRequest) {
  const cookieStore = await cookies()
 
  const a = cookieStore.get('a')
  const b = cookieStore.set('b', '1')
  const c = cookieStore.delete('c')
}
// 使用
import { cookies } from 'next/headers'
 
export async function GET(request: Request) {
  const cookieStore = await cookies()
  const token = cookieStore.get('token')
 
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { 'Set-Cookie': `token=${token.value}` },
  })
}
// 使用
import { type NextRequest } from 'next/server'
 
export async function GET(request: NextRequest) {
  const token = request.cookies.get('token')
}
// 重定向
import { redirect } from 'next/navigation'
 
export async function GET(request: Request) {
  redirect('https://nextjs.org/')
}
```
## Link组件
<Link /> 组件允许您使用 JavaScript 进行客户端导航。
每当 <Link /> 组件出现在浏览器的视口中时，Next.js 都会在后台自动预取链接路由的代码。当用户点击链接时，目标页面的代码已经在后台加载，这使得页面转换几乎是即时的！
```typescript
import Link from 'next/link';
<Link
	key={link.name}
	href={link.href}
	className="flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3"
>	
	<LinkIcon className="w-6" />
	<p className="hidden md:block">{link.name}</p>
</Link>
```
使用clsx
```typescript
import { usePathname } from 'next/navigation';
import clsx from 'clsx';
const pathname = usePathname();
            className={clsx(
              'flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3',
              {
                'bg-sky-100 text-blue-600': pathname === link.href,
              },
            )}
```
## Server Components
默认情况下，Next.js 应用程序使用 React Server Components。
```typescript
import postgres from 'postgres';
 
const sql = postgres(process.env.POSTGRES_URL!, { ssl: 'require' });
```
通过静态渲染，数据提取和渲染在构建时（部署时）或重新验证数据时在服务器上进行。每当用户访问您的应用程序时，都会提供缓存的结果。静态渲染有几个好处：
更快的网站 - 预渲染内容在部署到 Vercel 等平台时，可以缓存并进行全局分发。这确保全球用户能够更快、更可靠地访问您的网站内容。
降低服务器负载 - 由于内容已缓存，您的服务器无需为每个用户请求动态生成内容。这可以降低计算成本。
SEO - 预渲染内容更容易被搜索引擎爬虫索引，因为页面加载时内容已经可用。这可以提高搜索引擎排名。
静态渲染对于没有数据或数据在用户之间共享的 UI（例如静态博客文章或产品页面）非常有用。它可能不太适合包含定期更新的个性化数据的仪表板。


使用动态渲染，内容会在请求时（用户访问页面时）在服务器上为每个用户渲染。动态渲染有几个好处：
实时数据 - 动态渲染允许您的应用程序显示实时或频繁更新的数据。这对于数据频繁变化的应用程序来说是理想的选择。
用户特定内容 - 更容易提供个性化内容，例如仪表板或用户配置文件，并根据用户交互更新数据。
请求时间信息 - 动态渲染允许您访问只能在请求时知道的信息，例如 cookie 或 URL 搜索参数。
使用动态渲染，您的应用程序的显示速度最快只能达到最慢的数据获取速度。


流式传输是一种数据传输技术，它允许您将路由分解为更小的“块”，并在它们准备就绪时逐步将它们从服务器流式传输到客户端。
![](/images/nextjs4.avif)
通过流式传输，您可以防止缓慢的数据请求阻塞整个页面。这使得用户能够查看页面的各个部分并进行交互，而无需等待所有数据加载完毕后再向用户显示任何 UI。
流式传输与 React 的组件模型配合良好，因为每个组件都可以被视为一个块。
在 Next.js 中，有两种实现流式传输的方式： 
在页面级别，使用loading.tsx文件（它会为你创建 <Suspense />）。 
在组件级别，使用 <Suspense /> 进行更精细的控制。
## loading.tsx
loading.tsx 是一个基于 React Suspense 构建的特殊 Next.js 文件。它允许你创建备用 UI，以便在页面内容加载时替代显示。您在loading.tsx中添加的任何UI都将作为静态文件的一部分嵌入，并首先发送。然后，其余的动态内容将从服务器流式传输到客户端。
## Route groups
在 dashboard 文件夹内创建一个名为 /(overview) 的新文件夹。然后将你的loading.tsx 和 page.tsx 文件移动到该文件夹​​中：
![](/images/nextjs5.avif)
Route groups允许您将文件组织成逻辑组，而不会影响 URL 路径结构。当您使用括号 () 创建新文件夹时，其名称不会包含在 URL 路径中。因此，/dashboard/(overview)/page.tsx 会变成 /dashboard。
## React Suspense
您还可以使用 React Suspense 更加细粒度地传输特定组件。
Suspense 允许你将应用程序的某些部分延迟渲染，直到满足某些条件（例如，数据加载完成）。你可以将动态组件包装在 Suspense 中。然后，将一个 fallback 组件传递给 Suspense，以便在动态组件加载时显示。
```typescript
import { Suspense } from 'react';
import { RevenueChartSkeleton } from '@/app/ui/skeletons';
  return (
    <main>
        <Suspense fallback={<RevenueChartSkeleton />}>
          <RevenueChart />
        </Suspense>
    </main>
  );
```
## use client
"use client"，这是一个客户端组件，这意味着您可以使用event listeners和hooks。
```typescript
import { useSearchParams } from 'next/navigation';
const searchParams = useSearchParams();
const params = new URLSearchParams(searchParams);





import { useSearchParams, usePathname, useRouter } from 'next/navigation';
  const pathname = usePathname();
  const { replace } = useRouter();
  replace(`${pathname}?${params.toString()}`);
```
## layout文件
`layout` 文件用于定义 Next.js 应用程序中的布局。
A root layout is the top-most layout in the root app directory.
布局组件应该接受并使用 `children` prop。
```typescript
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```
## 处理md/mdx文件
使用官方提供的工具
```ts
// 目前无法实现动态路由，其他都没有问题。虽然可以 import Welcome from '@/markdown/welcome.mdx' 然后直接 <Welcome />。但无法实现根据当前路由读取 md 文档再显示的功能，因为无法动态 import
// 先安装nextjs官方推荐和维护的包
npm install @next/mdx @mdx-js/loader @mdx-js/react @types/mdx
// 修改 next.config.ts
// This allows .mdx files to act as pages, routes, or imports in your application.
import createMDX from '@next/mdx'
 
const nextConfig = {
  // Configure `pageExtensions` to include markdown and MDX files
  pageExtensions: ['js', 'jsx', 'md', 'mdx', 'ts', 'tsx'],
  // Optionally, add any other Next.js config below
}
 
const withMDX = createMDX({
  // Add markdown plugins here, as desired
  // 默认是只处理mdx文件，要处理md文件，则添加该行代码
  extension: /\.(md|mdx)$/,
})
 
// Merge MDX config with Next.js config
export default withMDX(nextConfig)




// 和 app 目录同一目录层级创建 mdx-components.tsx 文件，定义全局 MDX 组件
// mdx-components.tsx 是将 @next/mdx 与 App Router 一起使用所必需的，没有它，它将无法工作
// 在 mdx-components.tsx 中添加样式和组件将影响应用程序中的所有 MDX 文件
import type { MDXComponents } from 'mdx/types'
 
const components: MDXComponents = {}
 
export function useMDXComponents(): MDXComponents {
  return components
}




// 当使用基于文件的路由时，您可以像使用任何其他页面一样使用 MDX 页面
  my-project
  ├── app
  │   └── mdx-page
  │       └── page.(mdx/md)
  |── mdx-components.(tsx/js)
  └── package.json
// page.(mdx/md) 文件，也就是说必须以文件名必须为page.(mdx/md)
// /mdx-page 路由显示 MDX 页面
import { MyComponent } from 'my-component'
 
# Welcome to my MDX page!
 
This is some **bold** and _italics_ text.
 
This is a list in markdown:
 
- One
- Two
- Three
 
Checkout my React component:
 
<MyComponent />
// 使用 imports
  .
  ├── app/
  │   └── mdx-page/
  │       └── page.(tsx/js)
  ├── markdown/
  │   └── welcome.(mdx/md)
  ├── mdx-components.(tsx/js)
  └── package.json
// mdx-page.(tsx/js)文件
import Welcome from '@/markdown/welcome.mdx'
 
export default function Page() {
  return <Welcome />
}


// 设置局部样式
import Welcome from '@/markdown/welcome.mdx'
 
function CustomH1({ children }) {
  return <h1 style={{ color: 'blue', fontSize: '100px' }}>{children}</h1>
}
 
const overrideComponents = {
  h1: CustomH1,
}
 
export default function Page() {
  return <Welcome components={overrideComponents} />
}


// mdx-layout.tsx
```
## 使用 remark 和 rehype 生态
```ts
// unified 本身是一个相当小的模块，充当统一处理不同内容格式的接口。围绕某种格式，存在一个生态系统。例如 Markdown 的 Remark，Html 的 Rehype
// remark-parse 定义了如何将 markdown 作为输入并将其转换为语法树
// remark-gfm 用于启用 GitHub 使用 GFM 添加的 markdown 扩展：自动链接文字（www.x.com）、脚注（[^1]）、删除线（~~stuff~~）、表格（| cell |...）和任务列表（* [x]）
// remark-rehype 可以从 remark（markdown 生态系统）切换到 rehype（HTML 生态系统）。它通过将当前 markdown（mdast）语法树转换为 HTML（hast）语法树来实现这一点。remark 插件处理 mdast，rehype 插件处理 hast，因此 remark-rehype 之后使用的插件必须是 rehype 插件。
// rehype-stringify 将语法树作为输入并将其转换为序列化的 HTML


// npm i to-vfile rehype-stringify remark-gfm remark-rehype rehype-pretty-code shiki rehype-autolink-headings rehype-slug
// process(await read("example.md")); 的 read 函数是基于根目录的，
// 即package.json所在目录
import rehypeStringify from "rehype-stringify";
import remarkGfm from "remark-gfm";
import remarkParse from "remark-parse";
import remarkRehype from "remark-rehype";
import rehypePrettyCode, { Options } from "rehype-pretty-code";
import rehypeSlug from 'rehype-slug'
import rehypeAutolinkHeadings from 'rehype-autolink-headings'
import { read } from "to-vfile";
import { unified } from "unified";
import rehypeAddH2Class from "@/utils/md-styles";

const prettyCodeOptions: Options = {
  theme: 'github-dark',
};

async function ok() {
  const file = await unified()
    .use(remarkParse)
    .use(remarkGfm)
    .use(remarkRehype)
    .use(rehypeSlug)
    .use(rehypeAutolinkHeadings)
    .use(rehypeAddH2Class)
    .use(rehypePrettyCode, prettyCodeOptions)
    .use(rehypeStringify)
    .process(await read("javascript基本语法.md"));

    return String(file);
}

export default async function Home() {
  const c = await ok();
  return <div dangerouslySetInnerHTML={{ __html: c }} />;
}



// rehypeAddH2Class.ts，作用是添加样式
import { visit } from "unist-util-visit";
import type { Node } from "unist";
import type { Element, Properties } from "hast";

interface ExtendedProperties extends Properties {
  className?: string[];
}

export default function rehypeAddH2Class() {
  return (tree: Node) => {
    visit(tree, "element", (node: Element) => {
      if (node.tagName === "h2") {
        node.properties = node.properties || {};
        const props = node.properties as ExtendedProperties;
        props.className = props.className || [];
        props.className.push("text-2xl py-2");
      } else if (node.tagName === "p") {
        node.properties = node.properties || {};
        const props = node.properties as ExtendedProperties;
        props.className = props.className || [];
        props.className.push("text-xl");
      } else if (node.tagName === "li") {
        node.properties = node.properties || {};
        const props = node.properties as ExtendedProperties;
        props.className = props.className || [];
        props.className.push("py-1.5");
      } else if (node.tagName === "a") {
        node.properties = node.properties || {};
        const props = node.properties as ExtendedProperties;
        props.className = props.className || [];
        props.className.push("text-blue-600");
      }
    });
  };
}


// globals.css，作用是添加样式
pre {
  overflow-x: auto;
  padding: 1rem 0;
  border-radius: 0.75rem;
}

pre [data-line] {
  padding: 0 1rem;
}
/* 平滑滚动 */
html {
  scroll-behavior: smooth;
}
/* 为所有标题设置滚动边距 */
h1, h2, h3, h4, h5, h6 {
  scroll-margin-top: 56px; /* 你的偏移量 */
}
```

```ts
当浏览器发送请求时，如果 URL 中包含中文或其他特殊字符，浏览器会自动使用 encodeURIComponent对其进行编码，转换成 %xx的形式
Next.js 服务器或路由机制接收到的是编码后的字符串
Next.js 不会自动为你解码这些参数，因此你需要手动调用 decodeURIComponent来将其还原为原始字符串
```

```ts
直接将字符串转换为 react 组件
export default function Home() {
	const html = "<h1>hi<h1>";
	return <div dangerouslySetInnerHTML={{ __html: html }} />;
}
```