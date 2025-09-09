完整教程：https://nextjs.org/learn/react-foundations




首先创建一个 index.html 文件
```html
<html>
  <body>
    <div id="app"></div>
  </body>
</html>
```
在该文件中添加 `<script>` 标签
```html
<html>
  <body>
    <div id="app"></div>
    <script type="text/javascript"></script>
  </body>
</html>
```
在 `<script>` 标签内，使用 DOM 方法 `getElementById()`，通过 id 选择 `<div>` 元素
```html
<html>
  <body>
    <div id="app"></div>
    <script type="text/javascript">
      const app = document.getElementById('app');
    </script>
  </body>
</html>
```
继续使用 DOM 方法来创建新的 `<h1>` 元素：
```html
<html>
  <body>
    <div id="app"></div>
    <script type="text/javascript">
		const app = document.getElementById('app');
		const header = document.createElement('h1');
		const text = 'Develop. Preview. Ship.';
		const headerContent = document.createTextNode(text);
		header.appendChild(headerContent);
		app.appendChild(header);
    </script>
  </body>
</html>
```
要在新创建的项目中使用 React，加载两个 React 脚本：
```html
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script type="text/javascript">
      const app = document.getElementById('app');
      const header = document.createElement('h1');
      const text = 'Develop. Preview. Ship.';
      const headerContent = document.createTextNode(text);
      header.appendChild(headerContent);
      app.appendChild(header);
    </script>
  </body>
</html>
```
删除之前添加的 DOM 方法，并添加 `ReactDOM.createRoot()` 方法
```typescript
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script>
      const app = document.getElementById('app');
      const root = ReactDOM.createRoot(app);
      root.render(<h1>Develop. Preview. Ship.</h1>);
    </script>
  </body>
</html>
```
如果您尝试在浏览器中运行此代码，您将收到语法错误。这是因为 `<h1>...</h1>` 不是有效的 Javascript。这段代码是 JSX。
JSX 是 JavaScript 的语法扩展，允许你使用熟悉的类似 HTML 的语法来描述 UI。JSX 的优点在于，除了遵循[三个 JSX 规则](https://react.dev/learn/writing-markup-with-jsx#the-rules-of-jsx)之外，您不需要学习 HTML 和 JavaScript 之外的任何新符号或语法。
但是浏览器不能直接理解 JSX，因此需要一个 JavaScript 编译器（例如 Babel）将您的 JSX 代码转换为常规 JavaScript。
要将 Babel 添加到你的项目中，只需复制
```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```
还需要通过将 `<script>` 类型更改为 `type=text/jsx` 来告知 Babel 要转换什么代码。
```typescript
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel Script -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/jsx">
      const domNode = document.getElementById('app');
      const root = ReactDOM.createRoot(domNode);
      root.render(<h1>Develop. Preview. Ship.</h1>);
    </script>
  </body>
</html>
```
使用 React 的 components
```typescript
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel Script -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/jsx">
		function Header() {
		  return <h1>Develop. Preview. Ship.</h1>;
		}
		 
		function HomePage() {
		  return (
		    <div>
		      <Header />
		    </div>
		  );
		}
		 
		const root = ReactDOM.createRoot(app);
		root.render(<HomePage />);
    </script>
  </body>
</html>
```
使用 React 的 props 进行传值并迭代列表
```typescript
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel Script -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/jsx">
		function Header({ title }) {
		  return <h1>{title ? title : 'Default title'}</h1>;
		}
		
		function HomePage() {
		  const names = ['Ada Lovelace', 'Grace Hopper', 'Margaret Hamilton'];
		 
		  return (
		    <div>
		      <Header title="Develop. Preview. Ship." />
		      <ul>
		        {names.map((name) => (
		          <li key={name}>{name}</li>
		        ))}
		      </ul>
		    </div>
		  );
		}
		
		const root = ReactDOM.createRoot(app);
		root.render(<HomePage />);
    </script>
  </body>
</html>
```
使用 event handlers ，名称采用驼峰命名法
```typescript
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel Script -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/jsx">
		function Header({ title }) {
		  return <h1>{title ? title : 'Default title'}</h1>;
		}
		
		function HomePage() {
		  const names = ['Ada Lovelace', 'Grace Hopper', 'Margaret Hamilton'];

		function handleClick() {
		console.log("increment like count")
		}
		 
		return (
		    <div>
		      <Header title="Develop. Preview. Ship." />
		      <ul>
		        {names.map((name) => (
		          <li key={name}>{name}</li>
		        ))}
		      </ul>
			  <button onClick={handleClick}>Like</button>
		    </div>
		);
		}
		
		const root = ReactDOM.createRoot(app);
		root.render(<HomePage />);
    </script>
  </body>
</html>
```
使用 React 的 hooks
You can pass the state information to children components as props, but the logic for updating the state should be kept within the component where state was initially created.
```typescript
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel Script -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/jsx">
		function Header({ title }) {
		  return <h1>{title ? title : 'Default title'}</h1>;
		}
		
		function HomePage() {
		  const names = ['Ada Lovelace', 'Grace Hopper', 'Margaret Hamilton'];
		  const [likes, setLikes] = React.useState(0);

		function handleClick() {
			setLikes(likes + 1);
		}
		 
		return (
		    <div>
		      <Header title="Develop. Preview. Ship." />
		      <ul>
		        {names.map((name) => (
		          <li key={name}>{name}</li>
		        ))}
		      </ul>
			  <button onClick={handleClick}>Like({likes})</button>
		    </div>
		);
		}
		
		const root = ReactDOM.createRoot(app);
		root.render(<HomePage />);
    </script>
  </body>
</html>
```



下载使用 Next.js
在与 index.html 文件相同的目录中创建一个名为 package.json 的新文件，其中包含一个空对象 {}。
```json
{}
```
在终端中，在项目根目录中运行以下命令：
```
npm install react@latest react-dom@latest next@latest
```
安装完成后，您应该能够在 package.json 文件中看到项目依赖项列表：
```json
{
  "dependencies": {
    "next": "^14.0.3",
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  }
}
```
您还会注意到一个名为 package-lock.json 的新文件，其中包含有关每个包的确切版本的详细信息。
删除 index.html 中的部分代码并将 `import { useState } from 'react';` 添加到文件顶部：
```typescript
import { useState } from 'react';
 
function Header({ title }) {
  return <h1>{title ? title : 'Default title'}</h1>;
}
 
function HomePage() {
  const names = ['Ada Lovelace', 'Grace Hopper', 'Margaret Hamilton'];
 
  const [likes, setLikes] = useState(0);
 
  function handleClick() {
    setLikes(likes + 1);
  }
 
  return (
    <div>
      <Header title="Develop. Preview. Ship." />
      <ul>
        {names.map((name) => (
          <li key={name}>{name}</li>
        ))}
      </ul>
 
      <button onClick={handleClick}>Like ({likes})</button>
    </div>
  );
}
```
index.html 文件中唯一剩下的代码是 JSX，因此您可以将文件类型从 .html 更改为 .js 或 .jsx。
以下是如何在 Next.js 中创建第一个页面的方法：
1. 创建一个名为 app 的新文件夹并将 index.js 文件移到其中。
2. 将 index.js 文件重命名为 page.js。这将是应用程序的主页面。
3. 将 export default 添加到您的 `<HomePage>` 组件，以帮助 Next.js 区分哪个组件要渲染为页面的主要组件。
```typescript
import { useState } from 'react';
 
function Header({ title }) {
  return <h1>{title ? title : 'Default title'}</h1>;
}
 
export default function HomePage() {
  // ...
}
```
接下来，运行开发服务器。向您的 package.json 文件添加“next dev”脚本：
```json
{
  "scripts": {
    "dev": "next dev"
  },
  "dependencies": {
    "next": "^14.0.3",
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  }
}
```
在终端中运行 npm run dev
当您访问 localhost:3000 时，您应该会看到以下错误：
![[react_nextjs1.avif]]
这是因为 Next.js 默认使用了 React 服务器组件，这是一个允许 React 在服务器端渲染的新功能。服务器组件不支持 useState，因此你需要使用客户端组件。
app 文件夹中自动创建了一个名为 layout.js 的新文件。这是应用程序的主布局。您可以使用它来添加所有页面共享的 UI 元素（例如导航、页脚等）。
```typescript
export const metadata = {
  title: 'Next.js',
  description: 'Generated by Next.js',
};
 
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

正如您在上一章中了解到的，Next.js 默认使用服务器组件 - 这是为了提高应用程序的性能。
回顾浏览器中的错误，Next.js 警告你正在尝试在服务器组件中使用 useState。你可以将 "Like" button 移至客户端组件来解决这个问题。
在应用程序文件夹内创建一个名为 like-button.js 的新文件：
要将 LikeButton 设置为客户端组件，请在文件顶部添加 React 的“use client”指令。这会告诉 React 在客户端上渲染该组件。
```typescript
'use client';
 
import { useState } from 'react';
 
export default function LikeButton() {
  const [likes, setLikes] = useState(0);
 
  function handleClick() {
    setLikes(likes + 1);
  }
 
  return <button onClick={handleClick}>Like ({likes})</button>;
}
```
回到您的 page.js 文件，将 LikeButton 组件导入到您的页面中：
```typescript
import LikeButton from './like-button';
 
function Header({ title }) {
  return <h1>{title ? title : 'Default title'}</h1>;
}
 
export default function HomePage() {
  const names = ['Ada Lovelace', 'Grace Hopper', 'Margaret Hamilton'];
 
  return (
    <div>
      <Header title="Develop. Preview. Ship." />
      <ul>
        {names.map((name) => (
          <li key={name}>{name}</li>
        ))}
      </ul>
      <LikeButton />
    </div>
  );
}
```
保存这两个文件，然后在浏览器中查看你的应用。现在没有错误了。