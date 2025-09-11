- [添加自定义字体next/font](#添加自定义字体next/font)
- [添加图片next/image](#添加图片next/image)
- [route](#route)
- [Link组件](#Link组件)
- [Server Components](#Server%20Components)
- [loading.tsx](#loading.tsx)
- [Route groups](#Route%20groups)
- [React Suspense](#React%20Suspense)
- [use client](#use%20client)
- [layout文件](#layout文件)

npm i next-mdx-remote
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
## route
Next.js 使用文件系统路由，其中​​文件夹用于创建嵌套路由。每个文件夹代表一个映射到 URL 段的路由段。
![[nextjs1.avif]]
您可以使用 layout.tsx 和 page.tsx 文件为每条路线创建单独的 UI。
page.tsx 是一个特殊的 Next.js 文件，它导出一个 React 组件，并且它是让路由可访问所必需的。
![[nextjs2.avif]]
/app/dashboard/page.tsx 与 /dashboard 路径关联。
```typescript
// Next.js的路由使用文件夹表示的，要使路由可以访问，则该文件夹下必须要有page.tsx文件
```

在 Next.js 中，您可以使用特殊的 layout.tsx 文件来创建多个页面共享的 UI。也就是说下面的 page.tsx、customers/page.tsx、invoices/page.tsx 这三个页面共享布局  layout.tsx，可以在该布局中添加诸如导航栏，侧边栏等组件。
![[nextjs3.avif]]
<Layout /> 组件（即 layout.tsx 文件）接收一个 children prop。这个子组件可以是一个页面，也可以是另一个布局。
/app/layout.tsx 称为根布局，每个 Next.js 应用程序都需要它。添加到根布局的任何 UI 都将在应用程序的所有页面之间共享。
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
![[nextjs4.avif]]
通过流式传输，您可以防止缓慢的数据请求阻塞整个页面。这使得用户能够查看页面的各个部分并进行交互，而无需等待所有数据加载完毕后再向用户显示任何 UI。
流式传输与 React 的组件模型配合良好，因为每个组件都可以被视为一个块。
在 Next.js 中，有两种实现流式传输的方式： 
在页面级别，使用loading.tsx文件（它会为你创建 <Suspense />）。 
在组件级别，使用 <Suspense /> 进行更精细的控制。
## loading.tsx
loading.tsx 是一个基于 React Suspense 构建的特殊 Next.js 文件。它允许你创建备用 UI，以便在页面内容加载时替代显示。您在loading.tsx中添加的任何UI都将作为静态文件的一部分嵌入，并首先发送。然后，其余的动态内容将从服务器流式传输到客户端。
## Route groups
在 dashboard 文件夹内创建一个名为 /(overview) 的新文件夹。然后将你的loading.tsx 和 page.tsx 文件移动到该文件夹​​中：
![[nextjs5.avif]]
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