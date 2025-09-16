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
// main 标签中的 flex-1 是必须的。flex-1 允许 flex item 根据需要增大或缩小，忽略其初始大小
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
实现该布局：
![[tailwind_5.png]]
```ts
<div className="bg-gray-100 min-h-screen flex items-center justify-center p-4">
  <div className="grid grid-cols-2 grid-rows-2 gap-4 w-full max-w-4xl h-96">
	<div className="row-span-2 bg-purple-500 rounded-lg flex items-center justify-center text-white text-2xl font-bold">
	  01
	</div>
	<div className="bg-purple-200 flex items-center justify-center text-gray-800 text-2xl font-bold">
	  02
	</div>
	<div className="bg-purple-500 flex items-center justify-center text-white text-2xl font-bold">
	  03
	</div>
  </div>
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


// 文字水平居中
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

// 圆角
rounded-sm

// font
font-mono
font-medium
font-extrabold

// background
bg-blue-200

// height
h-10
// 只占据包含的元素高度，非常适合和 overflow-auto 一起使用，实现滚动
h-svh
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
// flex-row 和 flex-col 必须和 flex 一起使用才会生效，也就是class="flex flex-row"
// flex-1 可以单独使用，不需要和 flex 一起使用就能生效
// flex 默认每个元素有一个 min-height，可以使用 min-h-0 来覆盖默认，允许元素尺寸小于内容
// class="flex" 和 class="flex flex-row" 效果相同，默认为横向排列
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
// 允许 flex item 根据需要增大或缩小，忽略其初始大小，可用于某一元素占据所有剩余空间
flex-1
// content-<xxx> 多行如何在垂直方向分布
content-start
content-center
content-end
content-between
content-evenly
// 元素填充所有可用空间
content-stretch

// grid，使用时要这样使用 grid grid-rows-2 grid-cols-2 gap-2
// grid-cols-[240px_minmax(0,1fr)] 的作用是创建一个​​两列网格​
// 第一列​​：固定宽度为 240px
// 第二列​​：​​弹性宽度​​，通常会占满第一列剩下的所有空间，同时具备很强的收缩能力
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

```ts
<div class="w-1/2 mx-auto"> <!-- 这个div的宽度是父容器的一半并且水平居中 -->
  居中内容
</div>



// absolute top-1/2 left-1/2将子元素的​​左上角​​定位到父容器​​中心点​​
// transform -translate-x-1/2 -translate-y-1/2然后让子元素​​向左和向上各平移自身宽高的 50%​​，从而使子元素的​​中心点​​与父容器的中心点对齐
// 此方法也适用于 fixed定位的元素，只需将 absolute替换为 fixed，它会相对于浏览器视口居中
<div class="relative h-64"> <!-- 父容器需要设置相对定位和一个高度 -->
  <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
    居中内容
  </div>
</div>




// text-center通过设置 text-align: center;使块级元素内的​​行内内容​​水平居中
// text-center只影响其内部的​​行内内容​​。如果一个块级子元素本身有宽度，它不会在父容器中居中，除非你额外为其添加 mx-auto
<div class="text-center"> <!-- 这个div内的行内内容会水平居中 -->
  <p>这段文字会居中显示。</p>
  <span>这个span元素也会居中。</span>
  <img src="image.jpg" alt="图片也会居中" class="inline"> <!-- 注意图片默认为inline-block -->
</div>
```
## 可以直接使用的博客布局
```ts
<div className="flex flex-col min-h-screen">
    <div className="sticky top-0 flex justify-between items-center py-4 px-4 z-30 h-16 border-b bg-background">
      <Link href="#" className="text-2xl">
        Candvert&apos;s blog
      </Link>
      <div className="flex flex-row justify-between">
        <div>left</div>
        <div>right</div>
      </div>
    </div>
    <div className="flex-1 flex">
      <div className="w-64 overflow-auto scroll-smooth scrollbar-hide h-svh">
        <div>left</div>
      </div>
      <div className="flex-1 flex">
        <div className="flex-1">
          <div className="scroll-smooth h-svh">
            <div>main</div>
          </div>
        </div>
        <div className="w-64 overflow-auto scroll-smooth scrollbar-hide h-svh">
          <div>right</div>
        </div>
      </div>
    </div>
	<div className="h-20">b</div>;
</div>
```
隐藏scrollbar
```ts
/* global.css */
.scrollbar-hide {
  -ms-overflow-style: none;  /* IE 和 Edge */
  scrollbar-width: none;     /* Firefox */
}
.scrollbar-hide::-webkit-scrollbar {
  display: none; /* Chrome, Safari 和 Opera */
}


/* xxx.html */
<div class="overflow-auto scrollbar-hide">
  <!-- 可滚动但无滚动条 -->
</div>
```