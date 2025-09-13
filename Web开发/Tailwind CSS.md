```jsx
// https://ui.shadcn.com/docs/blocks 网站的布局
body
	- div relative z-10 flex min-h-svh flex-col
		- header sticky top-0 z-50 w-full
			- div container-wrapper 3xl:fixed:px-0 px-6
				- div 3xl:fixed:container
					- 各个 element
		- main
			- div container-wrapper
				- div sidebar-wrapper
					- div sidebar
						- div sidebar-content
							- 各个 element
					- div h-full w-full
						- div docs
							- div flex min-w-0 flex-1 flex-col
								- div mx-auto flex w-full max-w-2xl
									- 各个 element
							- div sticky top-[calc(var(--header-height)+1px)]
								- 各个 element
		- footer
<body class="text-foreground group/body overscroll-none font-sans antialiased [--footer-height:calc(var(--spacing)*14)] [--header-height:calc(var(--spacing)*14)] xl:[--footer-height:calc(var(--spacing)*24)] __variable_5cfdac __variable_224a03 __variable_e8ce0c theme-default">
<div class="bg-background relative z-10 flex min-h-svh flex-col">
	<header class="bg-background sticky top-0 z-50 w-full">
		<div class="container-wrapper 3xl:fixed:px-0 px-6">
			<div class="3xl:fixed:container flex h-(--header-height) items-center gap-2 **:data-[slot=separator]:!h-4">
				nav
			</div>
		</div>
	</header>
	<main class="flex flex-1 flex-col">
		<div class="container-wrapper flex flex-1 flex-col px-2">
			<div data-slot="sidebar-wrapper" class="group/sidebar-wrapper has-data-[variant=inset]:bg-sidebar flex w-full 3xl:fixed:container 3xl:fixed:px-3 min-h-min flex-1 items-start px-0 [--sidebar-width:220px] [--top-spacing:0] lg:grid lg:grid-cols-[var(--sidebar-width)_minmax(0,1fr)] lg:[--sidebar-width:240px] lg:[--top-spacing:calc(var(--spacing)*4)]" style="--sidebar-width: 16rem; --sidebar-width-icon: 3rem;">
				<div data-slot="sidebar" class="text-sidebar-foreground w-(--sidebar-width) flex-col sticky top-[calc(var(--header-height)+1px)] z-30 hidden h-[calc(100svh-var(--footer-height)+2rem)] bg-transparent lg:flex">
					sidebar
				</div>
				<div class="h-full w-full">
					<div data-slot="docs" class="flex items-stretch text-[1.05rem] sm:text-[15px] xl:w-full">
						<div class="flex min-w-0 flex-1 flex-col">
							<div class="h-(--top-spacing) shrink-0">
								<div class="mx-auto flex w-full max-w-2xl min-w-0 flex-1 flex-col gap-8 px-4 py-6 text-neutral-800 md:px-0 lg:py-8 dark:text-neutral-300">
									mainarea
								</div>
							</div>
						</div>
						<div class="sticky top-[calc(var(--header-height)+1px)] z-30 ml-auto hidden h-[calc(100svh-var(--footer-height)+2rem)] w-72 flex-col gap-4 overflow-hidden overscroll-none pb-8 xl:flex">
							right sidebar
						</div>
					</div>
				</div>
			</div>
		</div>
	</main>
	<footer class="group-has-[.section-soft]/body:bg-surface/40 3xl:fixed:bg-transparent group-has-[.docs-nav]/body:pb-20 group-has-[.docs-nav]/body:sm:pb-0 dark:bg-transparent">
	</footer>
</div>
</body>
```

## flex布局和grid布局
```
flex​​ 擅长在​​一个方向​​（水平或垂直）上排列和对齐元素，它让元素具有弹性，能根据可用空间动态调整大小和顺序。它非常适合处理​​组件内部的布局

grid​​ 则是一个​​二维系统​​，它允许你同时定义​​行和列​​，从而实现对整体结构的精确控制。它非常适合用于构建​​页面的宏观骨架和复杂的二维布局​​

混合使用 flex 和 grid 正成为最佳实践：grid 构建宏观骨架，flex 处理微观排列​​：这是最常见的组合方式。使用 ​​grid 定义页面的整体结构​​（例如，划分出页头、侧边栏、主内容区、页脚等区域），然后在​​各个 grid 区域内部，使用 flex 来排列其子元素​​（例如，在导航区域内水平排列链接，在卡片区域内垂直排列图文内容）。这样，grid 负责大局的排兵布阵，flex 负责内部的精细调整。
这种分工使代码逻辑更清晰。修改整体布局时，只需调整 grid；修改组件内部结构时，只需调整 flex，两者耦合度低，更易于维护和扩展。
```
实现该布局：
![[tailwind_3.png]]
```ts
<div className="flex flex-col min-h-screen">
	<header>
		<p className="bg-amber-700">a</p>
	</header>
	<main className="flex-1 grid grid-cols-12 gap-4">
		<p className="col-span-2 bg-blue-700">b</p>
		<p className="col-span-8 bg-yellow-500">c</p>
		<p className="col-span-2 bg-cyan-600">d</p>
	</main>
	<footer>
		<p className="bg-purple-700">e</p>
	</footer>
</div>
```

```js
sticky top-0 可以让元素固定在顶部，只有 sticky 而没 top-0 就没有固定的效果，保留原来位置
fixed 会导致悬浮，也就是说其他元素会和该元素在同一位置，没有“实体”，不占有原来位置
relative 偏移是相对于原位置的，偏移后仍保留原来位置
absolute 偏移是相对于祖先元素的，祖先元素需要有除静态定位的其他定位，如果没有祖先元素，则相对于document偏移。偏移后不占有原来位置
​​
data-[slot=separator]​​: 这是一个​​属性选择器​​。它匹配所有具有 slot属性、且该属性值​​等于​​ separator 的 HTML 元素。例如，它会匹配：<div data-slots="separator">或 <span slot="separator">。
```
## 移除文字覆盖
导航栏没有设置 bg-background
![[tailwind_1.png]]
导航栏设置 bg-background
![[tailwind_2.png]]

```js
在global.css中添加
@import "tailwindcss";


m{t|r|b|l}-{size}
p{t|r|b|l}-{size}


// text
text-center
// text-large
text-lg
text-2xl
text-blue
text-blue-400
text-[13px]
text-[#FFEF89]

// 上下左右都应用
m-2
// margin-top
mt-2
// margin-bottom
mb-2
// margin-x
mx-2
// margin-y
my-2
// margin-left
ml-2
// margin-right
mr-2

// padding
p-2

// font
font-mono
font-medium
font-extrabold

// background
bg-blue-200

// height
h-10
// 提升优先级
!h-10
w-full

// border
border-2
border-blue-200

// medium round
rounded-md
rounded-full

top-0

// 在下方加一条分界线
border-b
border-l

// flex，使用时要这样使用 flex justify-center items-center
// flex 默认元素有一个 min-height，可以使用 min-h-0 来覆盖默认，允许元素尺寸小于内容
// 所有元素水平排列
flex-row
// 所有元素垂直排列
flex-col
// justify-<xxx> 一行中的所有元素如何分布
// 一行元素位于中间
justify-center
// 一行所有元素放在开头
justify-start
// 一行所有元素放在末尾
justify-end
// 每个元素占据相同空间
justify-evenly
// 第一个元素放在开头，最后一个元素放在末尾，剩余均匀分布
justify-between
// content-<xxx> 多行如何在垂直方向分布
content-start
content-center
contend-end
content-between
content-evenly
// 元素填充所有可用空间
content-stretch

// grid，使用时要这样使用 grid grid-rows-2 grid-cols-2 gap-2
grid-rows-2
grid-cols-2
row-span-2
col-span-2
// 行之间的间距
gap-x-4
// 列之间的间距
gap-y-4

// 不同设备显示
// 默认隐藏，若设备大于 medium，则显示
// 大小顺序 sm md lg xl 2xl
md:block hidden
sm:bg-blue-200 md:bg-red-200
max-sm:bg-blue-200 max-md:bg-red-200
md:flex
min-[320px]:text-center
max-[320px]:text-center

// 暗色模式
dark:bg-black text-black dark:text-white

// 阴影
shadow-xl

// 使元素跨越视口的整个高度
h-screen

accent-pink-200

// 字体自适应大小
text-[min(10vw,70px)]

items-center
space-x-6
space-y-6

// 
selection:bg-green-200

// 字体间距
tracking-tight
```

```js
// 放在css文件中
@theme {
	// 定义颜色，使用<h1 class="text-chestnut">hello</h1>
	--color-chestnut: #973F29
}

// 应用到所有h3标签上
@layer base {
	h3 {
		@apply text-base font-medium tracking-tight
	}
}

@layer components {
	.card {
		// 使用<h1 class="card">hello</h1>
		@apply m-10 rounded-lg bg-white px-6
	}
}

// 使用<h1 class="flex-center">hello</h1>
@utility flex-center {
	@apply flex justify-center items-center
}
```