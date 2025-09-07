- [html](#html)
	- [基本结构](#基本结构)
	- [常用标签](#常用标签)
	- [标签的属性](#标签的属性)
	- [图片标签的属性](#图片标签的属性)
	- [超链接标签](#超链接标签)
	- [注释和特殊字符](#注释和特殊字符)
	- [表格](#表格)
	- [列表](#列表)
	- [表单form](#表单form)
	- [块元素、行内元素和行内块元素](#块元素、行内元素和行内块元素)
	- [简便语法](#简便语法)
- [css](#css)
	- [基本语法](#基本语法)
	- [引入方式](#引入方式)
	- [选择器](#选择器)
	- [字体](#字体)
	- [Bootstrap](#Bootstrap)

# html
## 基本结构
```html
<html>
    <head></head>
    <body></body>
</html>
```
vscode默认生成的html文件
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

```
在 <html> 元素上提供一个带有有效语言标记的 lang 属性，将有助于屏幕阅读技术确定要宣告的适当语言。还可以确保页面的 <head> 中包含的重要元数据（如页面的 <title>）也会被正确地宣告。
中文是<html lang="zh">
简体中文是<html lang="zh-cn">
繁体中文是<html lang="zh-tw">







meta标签的属性：
charset
	如果存在该属性，则其值必须是不区分大小写的字符串 "utf-8" ，因为 UTF-8 是 HTML5 文档的唯一有效编码。声明字符编码的 <meta> 元素必须完全位于文档的前 1024 个字节内。
	
<meta> 元素可用于提供名称 - 值对形式的文档元数据，name 属性为元数据条目提供名称，而 content 属性提供值。
CSS 设备适配规范定义了以下元数据名称：
viewport: 为视口的初始大小提供指示。目前仅用于移动设备。
值：
width
	一个正整数，或者字符串 device-width。定义 viewport 的宽度，如果值为正整数，则单位为像素。
initial-scale
	一个 0.0 和 10.0 之间的正数。定义设备宽度（宽度和高度中更小的那个：如果是纵向屏幕，就是 device-width，如果是横向屏幕，就是 device-height）与 viewport 大小之间的缩放比例。
```
## 常用标签
```html
<!-- 标题 -->
<h1> </h1>
<h2> </h2>
<h3> </h3>
<h4> </h4>
<h5> </h5>
<h6> </h6>
<!-- 段落 -->
<p> </p>
<!-- div和span，div和span都是用来布局的，不同的是一个div占一行，而span一行可以放多个 -->
<div> </div>
<span> </span>
<!-- 图片 -->
<img src="URL"/>
<!-- 超链接 -->
<a href="https://baidu.com">百度</a>
<!-- 导航栏 -->
<nav> </nav>
<!-- 换行 -->
<br>
<!-- 分割线 -->
<hr>
<!-- 加粗 -->
<strong> </strong>或者<b> </b>
<!-- 倾斜 -->
<em> </em>或者<i> </i>
<!-- 删除线 -->
<del> </del>或者<s> </s>
<!-- 下划线 -->
<ins> </ins>或者<u> </u>


<!-- 视频 -->
<video controls>
  <source src="myVideo.webm" type="video/webm" />
  <source src="myVideo.mp4" type="video/mp4" />
  <p>
    Your browser doesn't support HTML video. Here is a
    <a href="myVideo.mp4" download="myVideo.mp4">link to the video</a> instead.
  </p>
</video>
<!-- 音频 -->
<audio controls>
  <source src="myAudio.mp3" type="audio/mp3" />
  <p>
    Your browser doesn't support.
  </p>
</audio>
<!-- 网站标签上的小图标 -->
<head>
    <link rel="icon" href="favicon.ico" type="image/x-icon">
    <!-- 如果是PNG格式 -->
    <link rel="icon" href="favicon.png" type="image/png">
</head>
<!-- 表单输入 -->
<input type="text" name="title"/>
<!-- 滑块 -->
<input type="range" min="0" max="100" value="50" step="1">


<!-- html5新增标签 -->
<header></header>
<nav></nav>
<article></article>
<section></section>
<aside></aside>
<footer></footer>
```
## 标签的属性
```html
<!-- 下面的src便是img标签的属性 -->
<img src="URL" />
```
## 图片标签的属性
```html
<!-- src图片路径 -->
<img src="URL" />
<!-- alt当图片无法加载时显示的文字 -->
<img alt="hello" />
<!-- title鼠标放在图像上，显示的文字 -->
<img title="hello" />
<!-- width图像的宽度 -->
<img width="200" />
<!-- height图像的高度 -->
<img height="200" />
<!-- border图像的边框 -->
<img border="20" />
```
## input标签
```html
<!-- type的值有email，url，date，time，month，week，number，tel，search，color -->
<input type="text">
<!-- value是框内的提示文字 -->
<input type="text" value="please input">
```
## 超链接标签
```html
<!-- 语法 -->
<a href="跳转地址" target="弹出窗口的方式"> </a>
<!-- 在当前窗口跳转到百度 -->
<a href="https://www.baidu.com" target="_self"> </a>
<!-- 跳转到当前目录下的page.html文件的页面 -->
<a href="page.html"> </a>
<!-- 点击后会下载image.jpg图片，下载的文件名为download.jpg -->
<a href="./image.jpg" download="download.jpg"> </a>
<!-- 跳转到新窗口，target属性的值为_self表示在当前窗口进行跳转，为_blank时表示跳转到一个新窗口 -->
<a href="https://www.baidu.com" target="_blank"> </a>
<!-- 单独的#表示空链接，不进行跳转 -->
<a href="#"> </a>
<!-- #和id一起使用跳转到指定标签的地方 -->
<h1 id="one"> </h1>
<a href="#one"> </a>
```
## 注释和特殊字符
```html
<!-- 这是注释 -->
<!-- 因为<和>等符号用于表示标签，所以要使用特殊字符，像c++中的转义字符 -->
<p> &lt; &gt; &nbsp; </p>	<!-- &lt;表示>，&gt;表示<，&nbsp;表示空格，&plus;表示+，&minus表示-，&times;表示X -->
```
## 表格
```html
<!-- tr表示一行，th(table head)表示表头单元格，tb(table data)表示一个单元格 -->
<table>
    <tr>
        <th>hello</th>
        <th>world</th>
    </tr>
    <tr>
        <td>hello</td>
        <td>world</td>
    </tr>
</table>

<!-- 表格标签的属性
	 align表示对齐方式，有left、center、right三个值
	 border表示表格单元是否有边框
	 cellpadding表示单元格边沿与其内容之间的空白，默认1像素
	 cellspacing表示单元格之间的空白，默认2像素
	 width，height表示表格的宽度，高度
-->
<table align="center" border="1" cellpadding="0" cellspacing="0" width="400" height="400">
</table>

<!-- thead表示表格的头部，tbody表示表格的主体 -->
<table>
    <thead>
        <tr>
            <th>hello</th>
            <th>world</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>hello</td>
            <td>world</td>
        </tr>
    </tbody>
</table>

<!-- 合并单元格，rowspan表示合并行的数量，colspan表示合并列的数量 -->
<table>
    <tr>
        <th>hello</th>
        <th colspan="2">world</th>		<!-- 该标签表示world占两列 -->
    </tr>
</table>
```
## 列表
```html
<!-- 无序列表 -->
<!-- 下面这段在浏览器的表示为
· hello
· world
-->
<ul>
    <li>hello</li>
    <li>world</li>
</ul>

<!-- 无序列表 -->
<!-- 下面这段在浏览器的表示为
1.hello
2.world
-->
<ol>
    <li>hello</li>
    <li>world</li>
</ol>

<!-- 自定义列表 -->
<!-- 下面这段在浏览器的表示为
ok
	hello
	world
-->
<dl>
    <dt>ok</dt>
    <dd>hello</dd>
    <dd>world</dd>
</dl>
```
## 表单form
```html
<!-- 表单 -->
<form>
    <input type="text">		<!-- input标签中的type属性表示类型是什么，比如button，checkbox，password -->
</form>


<!-- 表单常用的属性
	action用于指定接受表单数据的服务器地址
	method用于设置表单数据的提交方式，值为get或post
	name指定表单的名称
-->
<form action="URL" method="GET" name="表单域名称"> </form>


<!-- label标签通常和input标签一起使用
	当鼠标点击'男'时，也会选中input中表示的单选按钮
-->
<label for="sex">男</label>
<input type="radio" id="sex"/>


<!-- select标签表示下拉列表 -->
<select>
    <option>选项1</option>
    <option>选项2</option>
    <option>选项3</option>
</select>


<!-- input的name属性在提交时会显示在url中，比如url为https://baidu.com/?username=john -->
<input type="text" name="username">


<!-- text标签表示多行文本框 -->
<textarea rows="3" clos="20"> </textarea>
```
## 块元素、行内元素和行内块元素
```html
<!-- 块元素 -->
<!-- 块级元素通常用于组织和布局页面的主要结构和内容，例如段落、标题、列表、表格等。它们用于创建页面的主要部分，将内容分隔
成逻辑块。
	· 块级元素通常会从新行开始，并占据整行的宽度，因此它们会在页面上呈现为一块独立的内容快。
	· 可以包含其他块级元素和行内元素。
	· 常见的块级元素包括<div>, <p>, <h1>到<h6>, <ul>, <ol>, <li>, <table>, <form>等。
-->


<!-- 行内元素 -->
<!-- 行内元素通常用于添加文本样式或为文本中的一部分应用样式。它们可以在文本中插入小的元素，例如超链接、强调文本等。
	· 行内元素通常在同一行内呈现，不会独占一行。
	· 它们只占据其内容所需的宽度，而不是整行的宽度。
	· 行内元素不能包含块级元素，但可以包含其他行内元素。
	· 高、宽直接设置是无效的。
	· 常见的行内元素包括<span>, <a>, <b>、<strong>, <em>, <i>、<del>、<s>、<ins>、<u>、<img>, <br>等。
-->


<!-- 行内块元素 -->
<!-- 行内块元素即既具有块元素的可以调整宽高的功能，又具有行内元素一行可以有多个的功能
	· 常见的行内块元素包括<img/>、<input/>、<td>
-->
```
## 简便语法(Emmet语法)
```html
<!-- 下面的输入之后按tap键 -->

<!-- 生成3个div标签 -->
div*3
<!-- ul包含li -->
ul>li
<!-- div和p处于同级，同时添加 -->
div+p
<!-- 类名为sex的div标签 -->
.sex
<!-- 类名为sex的span标签 -->
span.sex
<!-- id为only的div标签 -->
#only
<!-- id为only的span标签 -->
span#only
<!-- 下面为
	<div class="sex1"></div>
	<div class="sex2"></div>
	<div class="sex3"></div>
-->
.sex$*3
<!-- 下面为<div>hello</div> -->
div{hello}
<!-- 下面为
	<div>1</div>
	<div>2</div>
	<div>3</div>
-->
div{$}*3
<!-- 复合写法 -->
div{hello}*5
```
# css
## 基本语法
```html
<!-- 基本语法
选择器 {
    属性: 值;
    属性: 值;
    属性: 值;
}
-->

<!-- 带-webkit-前缀的属性是作用于chrome的，-moz-作用于firefox -->
```
## 引入方式
```html
<!-- 都是在html中进行引用 -->
<!-- 行内样式表 -->
<p style="color: red;">男</p>

<!-- 内部样式表 -->
<style>
p {
    color: red;
}
</style>

<!-- 外部样式表 -->
<!-- 第一种 -->
<link rel="stylesheet" href="文件路径或URL"/>
<!-- 第二种 -->
<style>
    @import url("https://example.com/styles1.css");
    @import url("https://example.com/styles2.css");
</style>
```
## 选择器
```css
/* 选择器分为基础选择器和复合选择器 */
/* 基础选择器包括标签选择器，类选择器，id选择器，通配符选择器 */
/* 常见的复合选择器包括伪类选择器，并集选择器，子选择器，后代选择器 */

/* 标签选择器
	将所有的段落的字体颜色改为红色
*/
p {
    color: red;
}


/* 类选择器
	将所有属于sex类的字体颜色改为红色
*/
.sex {
    color: red;
}


/* id选择器
	将id为male的标签字体颜色改为红色
*/
#male {
    color: red;
}


/* 通配符选择器
	去掉所有标签的margin和padding
*/
* {
    margin: 0;
	padding: 0;
}


/* 伪类选择器
	将处于鼠标悬浮状态的div元素中的字体置为红色
*/
div:hover {
	color: red;
}
/* 链接伪类选择器
	a:link		选择所有未被访问的链接
	a:visited	选择所有已被访问的链接
	a:hover		选择鼠标指针位于其上的链接
	a:active	选择活动链接（鼠标按下未弹起的链接）
	设置多个时需要按:link、:visited、:hover、:active的顺序设置
*/
/* :focus伪类选择器
	用于选取获得焦点的表单元素
*/
input:focus {
	background-color: red;
}


/* 并集选择器
	改变多个选择器选中的元素
*/
div, a,
p:hover
{
	color: red;
}


/* 子选择器 */
div > a {
	color: red;
}


/* 后代选择器
	将父节点为ol的li标签颜色置为红色
*/
ol li {
	color: red;
}
/* 后代的后代 */
ol li a {
	color: red;
}


/* 选择器的权重
	如果多个选择器选中的同一个元素并指定同一属性，则按权重决定那个生效
	下面按权重越来越高书写
	通配符选择器
	标签选择器
	类、伪类选择器
	id选择器
	行内样式style=""
	!important
*/


/* 多类名
	定义了两个类名，sex和gender
*/
<p class="sex gender">男</p>


/* 将元素转化为块元素、行内元素或行内块元素
display: block;
display: inline:
display: inline-block;
*/
```
## css3新增选择器
```css
/* 属性选择器，结构伪类选择器，伪元素选择器 */

/* 属性选择器 */
<input type="text" value="hello">
<input type="number">
/* 选择具有value属性的元素 */
input[value] {
    
}
/* 选择具有type属性且值为number的元素 */
input[type=number] {
    
}
/* 选择具有type属性且以num开头的元素 */
input[type^=num] {
    
}
/* 选择具有type属性且以ber结尾的元素 */
input[type$=ber] {
    
}
/* 选择具有type属性且值中含有umbe的元素 */
input[type*=umbe] {
    
}


/* 结构伪类选择器 */
div li:first-child {}		/* div标签的第一个子元素 */
div li:last-child {}		/* div标签的最后一个子元素 */
div li:nth-child(3) {}		/* div标签的第3个子元素 */
div li:nth-child(even) {}	/* div标签的偶数子元素 */
div li:nth-child(odd) {}	/* div标签的奇数子元素 */
div li:nth-child(5n) {}		/* div标签的5的倍数的子元素 */
div li:first-of-type {}		/* div标签的第一个子元素 */
div li:last-of-type {}		/* div标签的第一个子元素 */
div li:nth-of-type(n) {}	/* div标签的第一个子元素 */


/* 伪元素选择器 */
/* before和after必须有content属性 */
/* before和after创建的元素分别位于父元素内部的前面和后面，为行内元素 */
/* 创建的元素不存在于文档树上，所以称为伪元素 */

```
## 属性
``` css
/* 使用谷歌的字体样式表 */
/* 先在html中的<head>标签中添加下面代码 */
<link href="https://fonts.googleapis.com/css?family=Open+Sans" rel="stylesheet" />
/* 再在css中加入下面代码 */
选择器 {
    font-family: "Open Sans";
}

/* 常见属性 */
html {
    font-size: 10px;
    font-family: "Open Sans";
    font-family: "Microsoft YaHei", Arial;	/* 如果有第一种字体则使用，依次类推，都没有就会使用浏览器默认字体 */
    font-weight: bold;
    font-style: italic;
    
    color: red;				/* 文本颜色 */
    color: #00ff00;
    color: rgb(255,0,0);
    text-align: center;
    text-decoration: none;
    text-decoration: underline;
    text-indent: 20px;		/* 只有首行缩进 */
    text-indent: 2em;		/* em相对与当前文字宽度 */
    line-height: 8px;
    
    background-size: 20px 40px;
    background-color: transparent;
    background-color: #00539f;
    background: rgba(0,0,0,0.3);
    background-image: url(./image.jpg);
    background-repeat: no-repeat;	/* 如果标签大小大于图片大小，默认图片会平铺，该选项可以取消平铺 */
    background-position: top right;	/* 即x y */
    background-position: 20px 40px;
    background-position: 20px center;
    background-attachment: fixed;	/* 可以使背景图片不随页面向下滚动 */
    
    border-width: 5px;
    border-style: dotted;
    border-color: black;
    border: 5px dotted black;	/* 没有先后顺序 */
    border-top: 5px dotted black;
    border-collapse: collapse;		/* 合并border */
    border-radius: 8px;
    border-radius: 50%;
    border-top-left-radius: 8px;
    
    padding-left: 20px;
    padding: 5px;
    padding: 5px 10px;		/* 上下5px，左右10px */
    padding: 5px 10px 20px;		/* 上5px，左右10px，下20px */
    padding: 0 20px 20px 20px;		/* 上右下左，顺时针 */
    
    /* text-shadow: h-shadow v-shadow blur color; 前两个是必须的 */
    text-shadow: 3px 3px 1px black;
    /* box-shadow: h-shadow v-shadow blur spread color inset; 前两个是必须的 */
    box-shadow: 1px 3px 5px rgba(0,0,0,0.1);
    
    margin-left: 20px;
    margin: 10px;
    margin: auto;
    margin: 0 auto;
    
    position: fixed;
    display: block;
    display: none;		/* 隐藏元素，并且不再占有原来位置 */
    float: left;
    z-index: 1;
    visibility: hidden;		/* 隐藏元素，保留原来位置 */
    visibility: visible;
    overflow: hidden;
    overflow: visible;
    box-sizing: border-box;		/* width就是最终宽度，margin和padding不会撑大盒子 */
    cursor: pointer;
    outline: none;
    resize: none;
    text-overflow: ellipsis;
    max-width: 500px;
    letter-spacing: 1px;
    transform: scale(2);		/* 放大两倍 */
    transform: scale(2, 2);		/* 放大两倍 */
    transform: rotate(60deg);	/* 旋转60度 */
    transform-origin: left bottom;	/* 旋转的中心点 */
    transform-origin: 10px 20px;	/* 旋转的中心点 */
};


/* css属性常用书写顺序：布局定位属性(display)、自身属性(width,height)、文本属性(color)、其他属性 */


/* font的复合属性 */
/* font: font-style font-weight font-size/line-height font-family; */
/* 必须按上面的顺序写，其中font-size和font-family必须要有 */
font: italic bold 32px/8px 'Microsoft YaHei';
font: 32px 'Microsoft YaHei';


/* background的复合属性 */
/* background: 背景颜色 背景图片 背景平铺 背景图像滚动 背景图片位置 */
/* 没有次序要求 */
background: #00ff00 url(./image.jpg) no-repeat fixed top right;


/* 浮动 */
/* 包括none，left，right三种 */
/* 应先用标准流的父元素排列上下位置，之后内部子元素使用浮动排列左右位置 */
float: none;
float: left;
float: right;
/* 清除浮动 */
clear: left;
clear: right;
clear: both;
overflow: hidden;


/* 定位 */
/* 定位配合偏移使用，也就是top、left等属性，可以top: 10px;也可以top: 10%; */
/* 只有定位的元素才有z-index属性，来控制前后次序 */
/* 静态定位是默认方式，即标准流方式 */
position: static;
/* 相对定位 */
/* 偏移是相对于原位置的，偏移后仍保留原来位置 */
position: relative;
/* 绝对定位 */
/* 偏移是相对于祖先元素的，祖先元素需要有除静态定位的其他定位，如果没有祖先元素，则相对于document偏移。偏移后不占有原来位置 */
position: absolute;
/* 固定定位 */
/* 不随浏览器滚动而消失，固定在屏幕上 */
/* 固定定位不占有原来位置 */
position: fixed;


/* transition属性 */
/* 鼠标移动到元素上的动画过渡效果 */
/* transition: 要过渡的属性 时间 运动曲线 开始时间，前两个是必须的 */
transition: width 0.2s ease 0.1s;
transition: all 0.2s linear 0.1s;
transition: width 0.2s, height 0.1s;	/* 过渡width和height两个属性 */
```
## flex布局
```css
display: flex;
/* 任何容器都可以使用flex布局 */
/* flex布局默认不换行，即所有子元素在一行上 */
/* flex: 2;表示子元素所占的份数为2份，flex: 1 auto;表示左右空间平分 */
/* 当父元素为flex布局，子元素的float、clear和vertical-align属性失效 */

/* 用于父元素的属性 */
/* flex-direction设置主轴的方向，即按行还是按列 */
/* flex-direction属性四个值为row，row-reverse，column，column-reverse，默认为row */
flex-direction: column;
/* justify-content设置主轴上的子元素排列方式 */
/* justify-content属性值为flex-start，flex-end，center，space-around，space-between */
justify-content: center;
/* flex-wrap设置子元素是否换行 */
flex-wrap: wrap;
/* align-content设置侧轴上的子元素的排列方式（多行） */
/* align-content属性值为flex-start，flex-end，center，space-around，space-between，stretch */
align-content: center;
/* align-items设置侧轴上的子元素排列方式（单行） */
/* align-items属性值为flex-start，flex-end，center，stretch */
align-items: center;
/* flex-flow复合属性，相当于同时设置flex-direction和flex-wrap */
flex-flow: row wrap;
```
## 实现弹跳动画效果
```css
/* html */
<img class="is-bottom" src="./message.svg" width="20px" height="20px">
/* css */
@keyframes bounce {
    0% {
        transform: translateY(0);
    }

    50% {
        transform: translateY(-5px);
    }

    100% {
        transform: translateY(0);
    }
}

.is-bottom:hover {
    animation-name: bounce;
    animation-duration: 0.5s;
    /*
    animation-timing-function: ease;	// 动画的速度曲线
    animation-delay: 1s;
    animation-iteration-count: 2;	// 动画播放的次数
    animation-direction: alternater;
    animation-play-state: paused;
    animation-fill-mode: forwards;
    */
}
```
## css的特性
```css
/* 继承性 */
/* 子标签会继承父标签的某些样式，text-, font-, line-开头的可以继承，以及color属性 */
<div>
	<span>hello</span>
</div>
```
## Bootstrap
```html
// 栅格系统
<div class="container">
    <div class="row">
        <div class="col-md-4">hello</div>
        <div class="col-md-4">world</div>
        <div class="row">
            <div class="col-md-6">hello</div>
            <div class="col-md-6">world</div>
        </div>
    </div>
</div>
```
