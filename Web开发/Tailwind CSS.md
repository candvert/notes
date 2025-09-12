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