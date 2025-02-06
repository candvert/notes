# Electron

```js
// 示例
const { app, BrowserWindow } = require('electron')

function createWindow() {
    let mainWin = new BrowserWindow({
        width: 800,
        height: 400
    })
    
    mainWin.loadFile('index.html')
}

app.on('ready', createWindow)

// 用webContents来操纵DOM
mainWin.webContents.on('did-finish-load', ()=>{})
```

## 窗口属性

```js
let mainWin = new BrowserWindow({
    width: 800,
    height: 400,
    webPreferences: {
        preload: path.join(__dirname, 'preload.js')				// 预加载脚本
    }
})
```

## contextBridge连接主进程和渲染进程

```js
const { contextBridge } = require('electron')

contextBridge.exposeInMainWorld('versions', {
  node: () => process.versions.node,
  chrome: () => process.versions.chrome,
  electron: () => process.versions.electron
  // 除函数之外，我们也可以暴露变量
})
```

## ipcMain.handle和ipcRenderer.invoke进行进程间通信

```js
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('versions', {
  node: () => process.versions.node,
  chrome: () => process.versions.chrome,
  electron: () => process.versions.electron,
  ping: () => ipcRenderer.invoke('ping')
  // 除函数之外，我们也可以暴露变量
})
```

