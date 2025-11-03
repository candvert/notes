- [dom相关](#dom相关)
	- [操控video](#操控video)
	- [DOMContentLoaded事件](#DOMContentLoaded事件)
	- [弹出对话框](#弹出对话框)
	- [绑定事件和取消事件](#绑定事件和取消事件)
	- [DOM](#DOM)
	- [window对象](#window对象)
	- [事件](#事件)
- [api和框架](#api和框架)
	- [内置api](#内置api)
		- [Date模块](#Date模块)
	- [浏览器api](#浏览器api)
	- [Node.js](#Node.js)
	- [electron](#electron)
	- [第三方api](#第三方api)
		- [firebase](#firebase)
## 绑定事件和取消事件
```html
<!-- 通过html属性进行绑定，不建议 -->
<button onclick="click_event()">点击事件</button>
<script>
    function click_event() {
        alert('点击事件')
    }
</script>

<!-- 通过DOM属性进行绑定 -->
<button>按钮</button>
<script>
    let btn = document.getElementsByTagName('button')[0]
    btn.onclick = function() {
        alert('按钮')
    }
</script>

<!-- 通过addEventListener方法进行绑定，推荐用法 -->
<button>按钮</button>
<script>
    let btn = document.getElementsByTagName('button')[0]
    btn.addEventListener('click', function() {alert('按钮')})
</script>

<!-- 常用事件
	click、mouseover、mouseout、change（文本内容改变）、select、focus、blur（移开光标）、dblclick（双击）
-->


<!-- 取消事件 -->
<!-- 第一种方法 -->
<script>
	btn.removeEventListener('click', func)		<!-- 需要两个参数，第二个参数为绑定的事件处理函数 -->
</script>

<!-- 第二种方法 -->
<script>
	const controller = new AbortController();
	btn.addEventListener("click", () => {}, { signal: controller.signal });
    controller.abort(); /* removes any/all event handlers associated with this controller */
</script>
```
## DOM
```js
// 获取DOM元素
const body = document.querySelector('body')		//参数为css选择器，推荐使用
const body = document.querySelectorAll('body')
const btn = document.getElementById('btn')
const p = document.getElementsByTagName('p')
const p = document.getElementsByClassName('paragraph')

document.defaultView	// 返回window对象

// 生成新的Node
const divNode = document.createElement('div')
// 添加Node
body.appendChild(divNode)

// 刷新当前页面，从缓存中加载
document.location.reload();
// 强制从服务器重新加载页面
document.location.reload(true);
```
## window对象
```js
// window包含DOM
window.document		// 返回DOM

// 打开一个新tab
window.open("URL")
```
## 事件
```js
// 取消浏览器提交表单时刷新页面的默认行为
// 使用preventDefalut()
let btn = document.querySelector('button')
function func(event) {
	event.preventDefault()
	console.log('cancel')
}
btn.addEventListener('click', func)

// 事件冒泡
// 意思是子元素的事件会向上传递，比如<div><button></button></div>，如果在div和button上注册事件处理函数，那么button上触发的事件会调用button和div的事件处理函数
// 可以在button的事件处理函数中使用stopPropagation()函数阻止事件向上传递
function func(event) { event.stopPropagation() }

// 事件捕获（默认关闭）
// 事件捕获和事件冒泡相反，由最外层向最内层传递，事件捕获默认是关闭的，可以通过下面形式来开启事件捕获
btn.addEventListener("click", () => {}, { capture: true })

// event.target.tagName可以获取触发事件的标签名，event.currentTarget.tagName可以获取正在处理事件的标签名
```
## worker
```js
// main.js
// Create a new worker, giving it the code in "generate.js"
const worker = new Worker("./generate.js");

// When the user clicks "Generate primes", send a message to the worker.
// The message command is "generate", and the message also contains "quota",
// which is the number of primes to generate.
document.querySelector("#generate").addEventListener("click", () => {
  const quota = document.querySelector("#quota").value;
  worker.postMessage({
    command: "generate",
    quota,
  });
});

// When the worker sends a message back to the main thread,
// update the output box with a message for the user, including the number of
// primes that were generated, taken from the message data.
worker.addEventListener("message", (message) => {
  document.querySelector("#output").textContent =
    `Finished generating ${message.data} primes!`;
});

document.querySelector("#reload").addEventListener("click", () => {
  document.querySelector("#user-input").value =
    'Try typing in here immediately after pressing "Generate primes"';
  document.location.reload();
});




// generate.js
// Listen for messages from the main thread.
// If the message command is "generate", call `generatePrimes()`
addEventListener("message", (message) => {
  if (message.data.command === "generate") {
    generatePrimes(message.data.quota);
  }
});

// Generate primes (very inefficiently)
function generatePrimes(quota) {
  function isPrime(n) {
    for (let c = 2; c <= Math.sqrt(n); ++c) {
      if (n % c === 0) {
        return false;
      }
    }
    return true;
  }

  const primes = [];
  const maximum = 1000000;

  while (primes.length < quota) {
    const candidate = Math.floor(Math.random() * (maximum + 1));
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }

  // When we have finished, send a message to the main thread,
  // including the number of primes we generated.
  postMessage(primes.length);
}
```
# dom相关
## 操控video
```js
const video = document.querySelector('video')

// 调整播放速度
video.playbackRate = 2
// 快进5秒
video.currentTime += 5
// 后退5秒
video.currentTime -= 5
// 暂停
video.pause()
// 播放
video.play()
// 调整音量（范围为[0, 1]）
video.volume = 0.6 
```
getBoundingClientRect
```js
let elem = document.querySelector("div");
let rect = elem.getBoundingClientRect();
react.left
react.right
react.top
react.bottom
```
MouseEvent
```js
const handleMouseMove = (e: MouseEvent) => {
	const x = e.clientX;
	const x2 = e.screenX;
	const y = e.clientY;
	const y2 = e.screenY;
}
```
## DOMContentLoaded事件
```js
// DOMContentLoaded事件在HTML文档解析完成，DOM树构建完毕时触发
// 无需等待样式表，图片等外部资源加载完成
document.addEventListener('DOMContentLoaded', function() {})
```
## 弹出对话框
```js
const name = prompt("What's your name?");
```
# api和框架
## 内置api
```js
// 关于数字类型Number
2 ** 4							  // js自带指数运算符，也可以使用Math.pow(2, 4)
10 / 3							  // 值为3.33333333，也就是说不像其他语言，js的除法结果是浮点数
const a = Math.random()				// 随机值，范围为[0,1)
const a = Math.floor(2.1)			// 2
const a = Math.ceil(2.1)
const a = Math.round(2.6)
const a = Math.sqrt(4)				// 返回平方根
const f = 1.766584958675746364;
const f2 = f.toFixed(2);			// 保留两位小数
const num = Number('42')			// 转化为Number类型


// 关于字符串类型String
let s = 'hello'
s.length
s[0]
s.include('ll')						// 返回true或false
s.startsWith('he')					// 返回true或false
s.endsWith('lo')					// 返回true或false
s.indexOf('e', 0)					// 返回字符串出现的索引，没找到返回-1。参数2是可选的，表示哪个索引开始
s.slice(0, 2)						// 返回字符串，范围是[)
s.toUpperCase()
s.toLowerCase()
s.replace('he', 'wo')				// 返回一个字符串，不会改变s，只会改变第一次出现的地方
s.replaceAll('he', 'wo')			// 返回一个字符串，不会改变s


// 关于数组类型Array
let arr = ['hello', 'world', 42, "end"]
arr.length
arr[0]
arr.indexOf(42, 0)					// 返回字符串出现的索引，没找到返回-1。参数2是可选的，表示哪个索引开始
arr.push("ok")						// 添加到尾部，返回数组新的长度
arr.unshift("ok")					// 添加的首部，返回数组新的长度
arr.pop()							// 删除尾部元素，返回被删除的元素
arr.shift()							// 删除首部元素，返回被删除的元素
arr.splice(0, 2)					// 删除arr中的元素，范围是[)
arr.map(() => {})					// 对每个元素执行操作，返回新数组，不会改变原来数组
arr.filter(() => {})				// 对每个元素执行操作，如果lambda返回true，则添加到新数组，返回新数组
const dogNames = ["Rocket", "Flash", "Bella", "Slugger"]
dogNames.toString()					// Rocket,Flash,Bella,Slugger
const data = "Manchester,London,Liverpool,Birmingham,Leeds,Carlisle"
const cities = data.split(",")		// 返回新数组
cities.join(',')					// 返回字符串

// flatMap() 方法对数组中的每个元素应用给定的回调函数，然后将结果展开一级，返回一个新数组。它等价于在调用 map() 方法后再调用深度为 1 的 flat() 方法
const arr1 = [1, 2, 1];
const result = arr1.flatMap((num) => (num === 2 ? [2, 2] : 1));
console.log(result);
// Expected output: Array [1, 2, 2, 1]


// 判断变量是否为空
let a
isNaN(a)							// 返回true如果变量为空


// js中的sleep
setTimeout(() => {}, 5000)


// 查看DOM元素的详细信息，显示其所有属性和方法
console.dir(document.querySelector('button'))
```
## Date模块
```js
// 如果没有输入任何参数，则 Date 的构造器会依据系统设置的当前时间来创建一个 Date 对象。
let now = new Date();
// 返回完整年份（四位数年份）
now.getFullYear()
// 返回月份（0–11），0 表示一年中的第一月
now.getMonth()
// 返回一个月中的哪一日（1-31）
now.getDate()
```
## 浏览器api
```js
// localStorage在用户的浏览器中存储数据，数据可以永久保留，直到显式删除
// 存储数据
localStorage.setItem('key', 'value');
// 获取数据
const value = localStorage.getItem('key');
// 删除数据
localStorage.removeItem('key');
// 清空所有数据
localStorage.clear();

// sessionStorage仅在当前会话中存储，关闭浏览器后数据会被删除
// 存储数据
sessionStorage.setItem('key', 'value');
// 获取数据
const value = sessionStorage.getItem('key');
// 删除数据
sessionStorage.removeItem('key');
// 清空所有数据
sessionStorage.clear();
```
## Node.js
```js
// require()函数在node中用于导入模块，或导入json文件内容。一般只在node中使用，因为浏览器和主流标准使用import/export语法
const path = require('path')
const j = require('./j.json')
// 常用模块
fs, path, http, url, event, util
// module.exports用于导出模块
module.exports = eat
exports.eat = eat

// 顶级对象为global
console.log(global)



// 使用Buffer模块分配内存
const b = Buffer.alloc()
const b = Buffer.allocUnsafe()
const b = Buffer.from('hello')
const b = Buffer.from([1, 2, 3])
b[0] = 22



// fs(file system)模块
// 导入
const fs = require('fs')
// 写入数据（异步）
fs.writeFile('a.txt', 'hello')
// 写入数据（同步）
file.writeFileSync('a.txt', 'hello')
// 追加数据
file.appendFile('a.txt', 'hello')
file.appendFileSync('a.txt', 'hello')
// 创建写入流对象
const ws = fs.createWriteStream('a.txt')
ws.write('hello')
// 读取文件
const data = fs.readFile('a.txt')
const data = fs.readFileSync('a.txt')

const rs = fs.createReadStream('a.txt')
rs.on('data', (chunk) => {
    console.log(chunk)
})
// 重命名文件或移动
fs.rename('a.txt', 'b.txt')
fs.renameSync('a.txt', 'b.txt')
// 删除文件
fs.rm('a.txt')
fs.rmSync()
file.unlink('a.txt')
fs.unlinkSync('a.txt')
// 创建文件夹
fs.mkdir('dir')
fs.mkdirSync('dir')
// 读取文件夹
fs.readdir('dir')
// 删除文件夹
fs.rmdir('dir')
// 访问文件属性
fs.stat('a.txt')
// 目录绝对路径
__dirname
// 文件绝对路径
__filename



// path模块
// 导入
const path = require('path')
// 得到绝对路径
path.resolve(__dirname, 'a.txt')
// 获得文件名，目录名，扩展名
path.basename()
path.dirname()
path.extname()



// http模块和url模块
// 导入
const http = require('http')
const url = require('url')
// createServer()
const server = http.createServer((request, response) => {})
server.listen(9000, () => {})

request.method
request.httpVersion
request.url
request.headers

response.statusCode = 200
response.statusMessage = 'OK'	// 无需手动设置
response.setHeader('Connection', 'keep-Alive')
response.write('hello')
response.end('hello')
```
## electron
```js
// 示例
const { app, BrowserWindow } = require('electron')
function createWindow() {
    let mainWin = new BrowserWindow({
        width: 800,
        height: 400
    })
    mainWin.loadFile('index.html')
    mainWin.on('close', () => console.log('closed'))
}
app.on('ready', createWindow)

// 用webContents来操纵DOM
mainWin.webContents.on('did-finish-load', ()=>{})
```
## 第三方api
### firebase
```js
import { initializeApp } from 'firebase/app'
import {
  getFirestore, collection, onSnapshot,
  addDoc, deleteDoc, doc,
  query, where,
  orderBy, serverTimestamp,
  updateDoc
} from 'firebase/firestore'
import {
  getAuth,
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword, signOut,
  onAuthStateChanged
} from 'firebase/auth'

const firebaseConfig = {
  apiKey: "AIzaSyDmXgb_58lO7aK_ujN37pGlNxzWGEU0YpI",
  authDomain: "fb9-sandbox.firebaseapp.com",
  projectId: "fb9-sandbox",
  storageBucket: "fb9-sandbox.appspot.com",
  messagingSenderId: "867529587246",
  appId: "1:867529587246:web:dc754ab7840c737f47bdbf"
}

// init firebase
const app = initializeApp(firebaseConfig)

// init services
const db = getFirestore(app)
const auth = getAuth()

// collection ref
const colRef = collection(db, 'books')

// queries
const q = query(colRef, where("author", "==", "patrick rothfuss"), orderBy('createdAt'))

// realtime collection data
const unsubCol = onSnapshot(q, (snapshot) => {
  let books = []
  snapshot.docs.forEach(doc => {
    books.push({ ...doc.data(), id: doc.id })
  })
  console.log(books)
})

// adding docs
const addBookForm = document.querySelector('.add')
addBookForm.addEventListener('submit', (e) => {
  e.preventDefault()

  addDoc(colRef, {
    title: addBookForm.title.value,
    author: addBookForm.author.value,
    createdAt: serverTimestamp()
  })
  .then(() => {
    addBookForm.reset()
  })
})

// deleting docs
const deleteBookForm = document.querySelector('.delete')
deleteBookForm.addEventListener('submit', (e) => {
  e.preventDefault()

  const docRef = doc(db, 'books', deleteBookForm.id.value)

  deleteDoc(docRef)
    .then(() => {
      deleteBookForm.reset()
    })
})

// fetching a single document (& realtime)
const docRef = doc(db, 'books', 'gGu4P9x0ZHK9SspA1d9j')

const unsubDoc = onSnapshot(docRef, (doc) => {
  console.log(doc.data(), doc.id)
})

// updating a document
const updateForm = document.querySelector('.update')
updateForm.addEventListener('submit', (e) => {
  e.preventDefault()

  let docRef = doc(db, 'books', updateForm.id.value)

  updateDoc(docRef, {
    title: 'updated title'
  })
  .then(() => {
    updateForm.reset()
  })
})

// signing users up
const signupForm = document.querySelector('.signup')
signupForm.addEventListener('submit', (e) => {
  e.preventDefault()

  const email = signupForm.email.value
  const password = signupForm.password.value

  createUserWithEmailAndPassword(auth, email, password)
    .then(cred => {
      console.log('user created:', cred.user)
      signupForm.reset()
    })
    .catch(err => {
      console.log(err.message)
    })
})

// logging in and out
const logoutButton = document.querySelector('.logout')
logoutButton.addEventListener('click', () => {
  signOut(auth)
    .then(() => {
      console.log('user signed out')
    })
    .catch(err => {
      console.log(err.message)
    })
})

const loginForm = document.querySelector('.login')
loginForm.addEventListener('submit', (e) => {
  e.preventDefault()

  const email = loginForm.email.value
  const password = loginForm.password.value

  signInWithEmailAndPassword(auth, email, password)
    .then(cred => {
      console.log('user logged in:', cred.user)
      loginForm.reset()
    })
    .catch(err => {
      console.log(err.message)
    })
})

// subscribing to auth changes
const unsubAuth = onAuthStateChanged(auth, (user) => {
  console.log('user status changed:', user)
})

// unsubscribing from changes (auth & db)
const unsubButton = document.querySelector('.unsub')
unsubButton.addEventListener('click', () => {
  console.log('unsubscribing')
  unsubCol()
  unsubDoc()
  unsubAuth()
})
```
## DOM的api
```js
// 有Window、Navigator、Document三种对象

const form = document.forms['add']	// 返回html中id为add的form元素
document.addEventListener("DOMContentLoaded", ()=>{})

// 改变内容
const link = document.querySelector("a");		//参数为css选择器
link.textContent = "Mozilla Developer Network";
link.href = "https://developer.mozilla.org";

// 添加节点
const sect = document.querySelector("section");
const para = document.createElement("p");
para.textContent = "We hope you enjoyed the ride.";
sect.appendChild(para);
const text = document.createTextNode(
  " — the premier source for web development knowledge.",
);
const linkPara = document.querySelector("p");

// 移动和删除节点
linkPara.appendChild(text);		// 移动节点
sect.removeChild(linkPara);
linkPara.remove();

// 改变节点的css
para.style.color = "white";
para.style.backgroundColor = "black";
para.style.padding = "10px";
para.style.width = "250px";
para.style.textAlign = "center";
para.setAttribute("class", "highlight");

// 设置节点的class
para.className = "paragraph"
para.classList.add("paragraph")
para.classList.remove("paragraph")

// 设置attribute
para.getAttribute("href")
para.setAttribute("href", "style.css")
para.hasAttribute("href")
para.removeAttribute("href")

// 刷新当前页面，从缓存中加载
document.location.reload()
// 强制从服务器重新加载页面
document.location.reload(true)

// 创建一个文档片段，可以将多个元素添加到文档片段中，然后一次性将片段添加到 DOM 中
const fragment = document.createDocumentFragment()
document.body.appendChild(fragment)

const arr = document.querySelector("a");
arr.textContent = "hello"
arr.innerHTML = "<h1>hello</h1>"
arr.nodeType
arr.nodeName
arr.parentNode
arr.parentElement
arr.children
arr.childNodes
arr.nextSibling
arr.nextElementSibling
arr.previousSibling
arr.previousElementSibling
arr.hasChildNodes()
arr.cloneNode(true)
arr.insertBefore(node, child)
arr.childElementCount
```
## fetch的api
```js
fetch(url).then().catch()
```
