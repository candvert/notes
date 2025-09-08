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
```html
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
```html
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
```html
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
