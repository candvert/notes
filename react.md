# react(version 18)

## 起始工作

```react
// 创建项目
// npx create-react-app local
// local为项目名

// npm start启动项目

// src目录下只需保留App.js和index.js两个文件，index.js是react项目的起点，一般在App.js文件中编写代码
// 下面是两个文件应保留的内容
// App.js
function App() {
  return (
    <div className="App">
      this is App
    </div>
  );
}

export default App;
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

## JSX语法

```react
// react使用的语法叫JSX
// 将JSX编译为js的工具为Babel
// 在JSX中可以通过大括号{}识别JavaScript中的表达式


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


// 组件
// 一个组件就是用户界面的一部分
// 定义组件
function Button() {
    return <button>click</button>
}
// 使用组件
function App() {
    return (
        <Button/>
        <Button></Button>
    )
}


// react中正常使用css文件，区别是class为className
import './style.css'
function App() {
    return (<button className='btn'>button</button>);
}


// 列表中的每一项应该有key属性
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);
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



```react
npx json-server --watch data/db.json --port 8000
```

```react
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


// prop
// App.js
import Home from 'Home'
const name = 'john'
function App() {
    return {
        <div className="App">
            <Home name={name} age={20}/>			// prop
        </div>
    }
}
// Home.js
const Home = (props) => {				// prop
    const name = props.name
    const age = props.age
    return (<div>hi</div>);
}
export default Home;


// 自定义hook，需要以use开头
// useFetch.js
const useFetch = () => {
    const [name, setName] = useState('mario');
    const changeName = () => setName('John');
    return {name, changeName};
}
export default useFetch;


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



