- [快速体验](#快速体验)
- [组件](#组件)
- [JSX语法](#JSX语法)
- [添加样式](#添加样式)
- [显示数据](#显示数据)
- [条件渲染](#条件渲染)
- [渲染列表](#渲染列表)
- [响应事件](#响应事件)
- [更新界面](#更新界面)
- [props](#props)
- [使用Hook](#使用Hook)

- [npm命令](#npm命令)
- [App Router](#App%20Router)
# React(version 19)，使用的框架为Next.js
```
全栈框架不需要服务器
Next.js 支持客户端渲染（CSR）、单页应用（SPA）和静态站点生成（SSG）。这些应用可以部署到 CDN 或静态托管服务（无需服务器）。此外，允许你根据实际需求，针对特定路由单独启用服务端渲染。
可以将 Next.js 应用部署到任何支持 Node.js 或 Docker 容器的托管平台，或者部署到你自己的服务器。 Next.js 也支持静态导出，无需服务器。
```
## 快速体验
```typescript
// 创建项目
// npx create-next-app@latest
// npm run dev启动项目，然后便可在浏览器访问http://localhost:3000查看网页
```
## 组件
```typescript
// React 应用程序是由组件组成的。一个组件是 UI（用户界面）的一部分，它拥有自己的逻辑和外观。组件可以小到一个按钮，也可以大到整个页面。
// React 组件是返回标签的 JavaScript 函数：
function MyButton() {
  return (
    <button>我是一个按钮</button>
  );
}
// 至此，你已经声明了 MyButton，现在把它嵌套到另一个组件中
function App() {
    return (
        <MyButton />
    )
}
// React 组件必须以大写字母开头，而 HTML 标签则必须是小写字母。
```
## JSX语法
```typescript
// React 使用的标签语法被称为 JSX
// JSX 比 HTML 更加严格。你必须闭合标签，如 <br />。你的组件也不能返回多个 JSX 标签。你必须将它们包裹到一个共享的父级中，比如 <div>...</div> 或使用空的 <>...</> 包裹：
function AboutPage() {
  return (
    <>
      <h1>关于</h1>
      <p>你好。<br />最近怎么样？</p>
    </>
  );
}
```
## 添加样式
```typescript
// 在 React 中，你可以使用 className 来指定一个 CSS 的 class。它与 HTML 的 class 属性的工作方式相同：
<img className="avatar" />
// 然后，你可以在一个单独的 CSS 文件中为它编写 CSS 规则：
/* 在你的 CSS 文件中修改 */
.avatar {
  border-radius: 50%;
}
```
## 显示数据
```typescript
// 在 JSX 中可以将 JavaScript 的表达式放进大括号 {} 中，这样你就可以从你的代码中嵌入一些变量并展示给用户
return (
  <h1>
    {user.name}
  </h1>
);
// 你还可以这样赋值 JSX 属性
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```
## 条件渲染
```typescript
// React 没有特殊的语法来编写条件语句，因此你使用的就是普通的 JavaScript 代码。例如使用 if 语句根据条件：
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
// 如果你喜欢更为紧凑的代码，可以使用条件 ? 运算符。与 if 不同的是，它工作于 JSX 内部：
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
// 当你不需要 else 分支时，你也可以使用更简短的逻辑 && 语法：
<div>
  {isLoggedIn && <AdminPanel />}
</div>
// 所有这些方法也适用于有条件地指定属性。
```
## 渲染列表
```typescript
// 你将依赖 JavaScript 的特性，例如 for 循环和 array 的 map() 函数来渲染组件列表。
// 假设你有一个产品数组：
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
// 在你的组件中，使用 map() 函数将这个数组转换为 <li> 标签构成的列表:
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
// 注意，<li> 有一个 key 属性。对于列表中的每一个元素，你都应该传递一个字符串或者数字给 key，用于在其兄弟节点中唯一标识该元素。
```
## 响应事件
```typescript
// 你可以通过在组件中声明事件处理函数来响应事件：
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      点我
    </button>
  );
}
// 注意，onClick={handleClick} 的结尾没有小括号！不要调用事件处理函数：你只需把函数传递给事件即可。
```
## 更新界面
```typescript
// 通常你会希望你的组件 “记住” 一些信息并展示出来，比如一个按钮被点击的次数。要做到这一点，你需要在你的组件中添加 state。
// 首先，从 React 引入 useState：
import { useState } from 'react';
// 现在你可以在你的组件中声明一个 state 变量：
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
// 你将从 useState 中获得两样东西：当前的 state（count），以及用于更新它的函数（setCount）。你可以给它们起任何名字，但按照惯例会像 [something, setSomething] 这样为它们命名。
// 第一次显示按钮时，count 的值为 0，因为你把 0 传给了 useState()。当你想改变 state 时，调用 setCount() 并将新的值传递给它。点击该按钮计数器将递增：
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
// 如果你多次渲染同一个组件，每个组件都会拥有自己的 state。你可以尝试点击不同的按钮：
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>独立更新的计数器</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      点了 {count} 次
    </button>
  );
}
```
## props
```typescript
// 在您的 HomePage 组件中，您可以将自定义标题属性传递给 Header 组件，就像传递 HTML 属性一样：
function HomePage() {
  return (
    <div>
      <Header title="React" />
    </div>
  );
}

// 子组件 Header 可以接受这些 props 作为其第一个函数参数：
// 如果您使用 console.log() props，您就会看到它是一个具有 title 属性的对象。
function Header(props) {
  console.log(props); // { title: "React" }
  return <h1>Develop. Preview. Ship.</h1>;
}

// 由于 props 是一个对象，因此您可以使用对象解构来明确命名函数参数中的 props 值：
function Header({ title }) {
  console.log(title);
  return <h1>{title}</h1>;
}
```
## 使用Hook
```typescript
// 以 use 开头的函数被称为 Hook。useState 是 React 提供的一个内置 Hook。你也可以通过组合现有的 Hook 来编写属于你自己的 Hook。
// Hook 比普通函数更为严格。你只能在你的组件（或其他 Hook）的顶层调用 Hook。如果你想在一个条件或循环中使用 useState，请提取一个新的组件并在组件内部使用它。

// 自定义hook，需要以use开头
// useToggle.tsx
import { useState } from 'react';
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  const toggle = () => setValue(prev => !prev);
  return [value, toggle];
}
export default useToggle;
// page.tsx
'use client';
import useToggle from './useToggle';
export default function Home() {
	const [isOn, toggleIsOn] = useToggle(false);
    return (
	    <div>
	      <p>当前状态: {isOn ? '开' : '关'}</p>
	      <button onClick={toggleIsOn}>切换</button>
	    </div>
    );
}
```
## npm命令
```typescript
npm create next-app@latest my-app --yes
// --yes选项跳过对话使用保存的偏好设置或默认的。默认启用TypeScript, Tailwind, App Router, Turbopack和import alias @/*

// 文件目录
app	App Router
pages	Pages Router
public	静态资源
src	Optional application source folder

// Top-level files
next.config.js	Configuration file for Next.js
package.json	Project dependencies and scripts
instrumentation.ts	OpenTelemetry and Instrumentation file
middleware.ts	Next.js request middleware
.env	Environment variables
.env.local	Local environment variables
.env.production	Production environment variables
.env.development	Development environment variables
.eslintrc.json	Configuration file for ESLint
.gitignore	Git files and folders to ignore
next-env.d.ts	TypeScript declaration file for Next.js
tsconfig.json	Configuration file for TypeScript
jsconfig.json	Configuration file for JavaScript

// Routing Files
layout		Layout
page		Page
loading		Loading UI
not-found		Not found UI
error		Error UI
global-error		Global error UI
route		API endpoint
template		Re-rendered layout
default		Parallel route fallback page
```

```typescript
// 在react中，可以通过&&、?:实现基础的条件渲染
{flag && <span>this is span</span>}
{loading ? <span>loading...</span> : <span>this is span</span>}


// 事件绑定
// on + 事件名称 = {事件处理程序}
function App() {
    return (<button onClick={clickHandler}>button</button>);
}
// 传递事件对象和自定义参数
function App() {
    return (<button onClick={(e) => clickHandler(e, 'John')}>button</button>);
}
```
## 标签
```js
// input
const [value, setValue] = useState('')
<input
	type="text"
	value={value}
	onChange={(e) => setValue(e.target.value)}
/>
```

```typescript
npx json-server --watch data/db.json --port 8000
```

```typescript
// useState是一个Hook函数，允许向组件添加一个状态变量，也就是变量变化时，UI也会变化
// useState返回一个数组，第一个元素就是变量，第二个元素是修改变量的函数
import { useState } from 'react';

const [name, setName] = useState('mario');
setName('john')


// useEffect是一个Hook函数，每次有组件渲染都会调用，第二个参数可以指定在哪些变量变化时调用
import { useEffect } from 'react';
useEffect(() => { return () => {} }, [])


// template和component
// Home.js
const Home = () => {
    return (<div>hi</div>);		// template
}
export default Home;
// App.js
import Home from 'Home'
function App() {
    return {
        <div className="App">
            <Home />			// component
        </div>
    }
}


// Router
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
function App() {
    return {
        <Router>
            <div className="App">
                <Navbar />
                <div className="content">
                    <Routes>
                        <Route path="/" element={<Home />} />
                        <Route path="/" element={<Create />} />
                        <Route path="/" element={<BlogLists />} />
                    </Routes>
                </div>
            </div>
        </Router>
    }
}


// Router Link取代html中的a标签
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import { Link } from 'react-router-dom';
<Link to="/creat">New Blogs</Link>


// router传参
// <Route path="/blogs/:id">，参数为:id
import { useParams } from 'react-router-dom';
const a = () => {
    const { id } = useParams()
}


// useNavigate
import { useNavigate } from 'react-router-dom'
const navigate = useNavigate();
navigate('/about');


// fetch进行post
fetch(url, {
    method: 'POST',
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(blog)
})


// AbortController()和fetch一起使用
const abortCont = new AbortController()
fetch(url, { signal: abortCont.signal })
useEffect(() => { return () => abortCont.abort() }, [])
err.name === 'AbortError'


// input标签
const [value, setValue] = useState('');
<input
    type="text"
    required
    value={value}
    onChange={ (e) => setValue(e.target.value) }
/>
```
## 打包并发布到Github Pages
```js
// 1. 安装 gh-pages 插件
npm install gh-pages --save-dev
// 2. 在 package.json 中添加主页字段
"homepage": "https://<username>.github.io/<repo-name>"
// 3. 在 package.json 中添加部署脚本
"scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
}
// 4. 执行部署命令
npm run deploy
```
## 库
```js
// lodash
// classnames
// uuid
// dayjs
// json-server
// axios
// redux
```
## App Router
```
Next.js中的App Router使用文件夹表示
当访问http://localhost:3000时看到的是app文件夹下的page.js：
app
	- page.js
而当访问http://localhost:3000/about时看到的则是app文件夹下的about文件夹下的page.js：
app
	- about
		- pages.js
	- pages.js
如果要使用动态路由，则文件夹的名称为[id]这种形式，其他[]，[projectId]等都可，只要存在[]
所以当访问http://localhost:3000/123时看到的是app文件夹下的id文件夹下的page.js：
app
	- [id]
		- pages.js
	- pages.js
```