```jsx
// https://ui.shadcn.com/docs/blocks 网站的布局
body
	- header
		- div container-wrapper
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
w-full

// border
border-2
border-blue-200

// medium round
rounded-md
rounded-full

top-0

// flex，使用时要这样使用 flex justify-center items-center
// 竖排
flex-col
// 一行/列元素如何分布
justify-center
justify-end
justify-evenly
// 
items-center
space-x-6
space-y-6

// grid，使用时要这样使用 grid grid-cols-3 gap-2
grid-rows-3

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

accent-pink-200

// 字体自适应大小
text-[min(10vw,70px)]

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