- [基础](#基础)
	- [基础知识](#基础知识)
	- [初始化项目](#初始化项目)
	- [显示网页](#显示网页)
	- [打包应用程序](#打包应用程序)
- [预加载脚本](#预加载脚本)
- [进程间通信](#进程间通信)
- [代码签名](#代码签名)

- [移除默认菜单栏](#移除默认菜单栏)
- [自定义标题栏](#自定义标题栏)
- [实现F11调用chrome开发工具](#实现F11调用chrome开发工具)
- [启动应用后全屏显示](#启动应用后全屏显示)

## 基础
## 基础知识
前端使用 Chromium 浏览器引擎进行渲染，后端则使用 Node.js 运行时环境

主进程运行在 Node.js 环境中，负责控制应用程序的生命周期、显示原生界面、执行特权操作以及管理渲染进程

每个由主进程创建的 BrowserWindow 或 Webview 标签都会对应一个独立的渲染进程。这些进程运行在 Chromium 渲染引擎提供的环境中

出于安全考虑，默认情况下渲染进程无法直接访问 Node.js 或 Electron 的主进程模块
渲染进程需要通过进程间通信向主进程发送消息，来请求执行系统级操作或获取数据
## 初始化项目
使用 Bun
```sh
$env:ELECTRON_MIRROR = "https://npmmirror.com/mirrors/electron/"
bun add electron -d
```
package.json 中需要配置的：
```json
{
  "name": "my-electron-app",
  "version": "1.0.0",
  "description": "Hello World!",
  "main": "main.js",
  "scripts": {
	"start": "electron .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Jane Doe",
  "license": "MIT"
}
```
## 显示网页
在 Electron 中，每个窗口都会显示一个网页，该网页可以从本地 HTML 文件或远程网址加载
首先在项目根目录创建一个 index.html 文件
```js
<h1>Hello from Electron renderer!</h1>
```
然后在 main.js 中将其加载到 BrowserWindow 中
```js
const { app, BrowserWindow } = require('electron')

const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600
  })

  win.loadFile('index.html')
}

app.whenReady().then(() => {
  createWindow()
})

// 在 windows 和 linux 上，使用下面代码，实现关闭所有窗口时完全退出应用程序
// app.on('window-all-closed', () => {
//   if (process.platform !== 'darwin') app.quit()
// })
```
应用程序在窗口中显示的每个网页都将在一个单独的进程中运行，该进程称为渲染进程
运行 Electron 应用：
```sh
bun start
```
## 打包应用程序
```sh
$env:ELECTRON_MIRROR = "https://npmmirror.com/mirrors/electron/"
bun add @electron-forge/cli -d
bun x electron-forge import
bun run make
```
## 预加载脚本
可以使用预加载脚本安全地将特权 API 暴露给渲染进程
为了将 Electron 的主进程和渲染进程连接起来，需要使用一个名为 preload 的特殊脚本

将只能在后端访问的 process.versions.node 等变量暴露给渲染进程的 preload.js 脚本
```js
const { contextBridge } = require('electron')

contextBridge.exposeInMainWorld('versions', {
  node: () => process.versions.node,
  chrome: () => process.versions.chrome,
  electron: () => process.versions.electron
  // we can also expose variables, not just functions
})
```
要将此脚本附加到渲染器进程，需要修改 main.js：
```js
const path = require('node:path')
const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
	// 添加该内容
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })

  win.loadFile('index.html')
}
```
修改 index.html 文件，导入 renderer.js 文件
```html
<script src="./renderer.js"></script>
```
在 renderer.js 中就可以通过 window.versions 或直接使用 versions 访问此变量
```js
console.log(versions.chrome())
```
## 进程间通信
可以使用 Electron 的进程间通信模块在主进程和渲染进程之间进行通信

通过向渲染器添加一个叫做 ping() 的全局函数来演示这一点。这个函数将返回一个从主进程翻山越岭而来的字符串
首先使用 ipcMain.handle 设置一个主进程处理程序，修改 main.js 文件
```js
app.whenReady().then(() => {
  // 添加这一行
  ipcMain.handle('ping', () => 'pong')
  createWindow()
})
```
然后，在预处理脚本中设置 invoke 调用，修改 preload.js 文件
```js
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('versions', {
  node: () => process.versions.node,
  chrome: () => process.versions.chrome,
  electron: () => process.versions.electron,
  // 添加这一行
  ping: () => ipcRenderer.invoke('ping')
})
```
现在就可以在 renderer.js 文件中使用
```js
const func = async () => {
  const response = await window.versions.ping()
  console.log(response) // 打印 'pong'
}
func()
```
## 代码签名
代码签名是一种安全技术，用于证明桌面应用程序是由已知来源创建的
## 移除默认菜单栏
```js
// 确保导入 Menu
const { app, BrowserWindow, Menu } = require('electron')

const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600
  })

  win.loadFile('index.html')

  // 完全移除菜单栏
  Menu.setApplicationMenu(null);
}
```
## 自定义标题栏
首先修改 main.js 文件
```js
const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
	// 隐藏原生标题栏
    titleBarStyle: "hidden",
	// 启用并自定义系统窗口控制按钮的位置和样式
    titleBarOverlay: {
      color: "#ffffff", // 标题栏区域背景色
      symbolColor: "#333333", // 控件图标颜色
      height: 40, // 可选：指定标题栏区域高度
    },
  });

  win.loadFile("index.html");
};
```
隐藏原生标题栏后，窗口将无法通过拖动来移动。需要在 HTML 页面中创建一个元素作为自定义标题栏，并为其添加 `-webkit-app-region: drag` 的 CSS 样式，使其可拖拽
```html
<body>
  <!-- 自定义标题栏 -->
  <header id="custom-title-bar">
    <div class="title">我的 Electron 应用</div>
  </header>
  
  <main>
    <!-- 应用的主要内容 -->
  </main>
</body>
```
CSS 样式
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

#custom-title-bar {
  -webkit-app-region: drag; /* 允许通过这个区域拖动窗口 */
  position: sticky;
  top: 0;
  left: 0;
  width: 100%;
  height: 40px;
  /* 更美观
  display: flex;
  align-items: center;
  padding-left: 8px;
  */
}

/* 需要将自定义标题栏内的其他元素排除在拖拽区域外 */
#custom-title-bar button {
  -webkit-app-region: no-drag;
}
```
## 实现F11调用chrome开发工具
```js
const { app, BrowserWindow, globalShortcut } = require('electron');

app.whenReady().then(() => {
  createWindow();

  // 注册全局快捷键 F11
  globalShortcut.register('F11', () => {
    // 切换开发者工具的开关状态
    if (win.webContents.isDevToolsOpened()) {
      win.webContents.closeDevTools();
    } else {
      win.webContents.openDevTools();
    }
  });
});

// 应用退出时，注销所有快捷键
app.on('will-quit', () => {
  globalShortcut.unregisterAll();
});
```
## 启动应用后全屏显示
```js
function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
	show: false, // 先不显示窗口，等全屏后再显示
  });

  win.maximize(); // 全屏窗口
  win.show(); // 显示窗口

  win.loadFile('index.html');
}
```