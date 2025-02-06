- [javascript基本语法](#javascript基本语法)
	- [编写地方](#编写地方)
	- [输入输出](#输入输出)
	- [变量](#变量)
	- [数组](#数组)
	- [常量](#常量)
	- [字符串模板](#字符串模板)
	- [比较](#比较)
	- [添加和循环语句](#添加和循环语句)
	- [...扩展运算符](#...扩展运算符)
	- [?.](#?.)
	- [??空值合并运算符](#??空值合并运算符)
	- [调试](#调试)
	- [函数](#函数)
	- [对象](#对象)
	- [import](#import)
	- [处理json](#处理json)
	- [绑定事件和取消事件](#绑定事件和取消事件)
	- [DOM](#DOM)
	- [window对象](#window对象)
	- [事件](#事件)
	- [worker](#worker)
	- [异步](#异步)
- [dom相关](#dom相关)
	- [操控video](#操控video)

- [api和框架](#api和框架)
	- [内置api](#内置api)
	- [浏览器api](#浏览器api)
	- [Node.js](#Node.js)
	- [electron](#electron)
	- [第三方api](#第三方api)
		- [firebase](#firebase)

# javascript基本语法

关于worker、异步等不熟悉的主题可以在[mozilla](https://developer.mozilla.org/en-US/docs/Learn/JavaScript)上学习

## 编写地方

```html
<!--在html中直接编写js语句，下面这句通常放在</body>前-->
<script> alert("ok") </script>

<!--单独编写一个js文件，再通过下面语句引入到html中，下面这句通常放在<head>标签中-->
<script type="module" src="script.js"></script>
```

## 输入输出

```js
// javascript的输入输出是相对于网页来说的，输入输出会显示在网页上
// 输入
prompt("ok")
// 输出
document.write("<h1>ok</h1>")
alert("ok")
console.log("ok")
```

## 变量

```js
var grade = 10	// var是遗留的，不建议使用
grade = 10	// 不加关键字，和python一样，这是由于var可以先初始化再声明，但是这会造成代码混乱
let name = 'hello', age = 20	// 新标准的写法，推荐使用

// 数据类型有Number、String、Boolean、Array、Object五种
// undefined即变量未赋值
console.log(typeof name)	// 使用typeof关键字来判断类型
let a = Number(name)		// 使用Number()来进行显式类型转换
```

## 数组

```js
let arr = ['hello', 'world', 42, "end"]
let arr = new Array("hello", "world", 42, 'end')
// 添加元素
arr.push("ok")		// 添加到尾部，返回数组新的长度
arr.unshift("ok")	// 添加到首部，返回数组新的长度
// 删除元素
arr.pop()			// 删除尾部元素，返回被删除的元素
arr.shift()			// 删除首部元素，返回被删除的元素
```

## 常量

```js
const i = 42
```

## 字符串模板

```js
let a = "hello"				// 字符串可以用单引号或双引号或反引号括起来，反引号中可以使用字符串模板
alert("hello" + "world")	// 字符串可以通过+号拼接
alert("hello" + a)			// 字符串可以通过+号拼接
alert(`this is ${a} world`)	// 字符串模板${a}，使用反引号会包括换行符，也就是说多行不需要像在单引号和双引号中那样使用\n
```

## 比较

```js
console.log(2 == '2')		// 结果为true，==只测试值是否相同，而不判断数据类型是否相同
console.log(2 === '2')		// 结果为false，比较推荐用===或!==
```

## 添加和循环语句

```js
// if语句，switch语句，while语句，for语句，do...while语句和c++中的相同
// 但范围for语句和c++中的不同
let nums = [23, 35, 67]
for (num of nums) {}	// num为每个元素值
for (index in nums) {}	// index为每个元素下标
// js中也有三元运算符 ? :
```

## ...扩展运算符

```js
// ...是扩展运算符，用于将可迭代对象（如数组或字符串）展开为单个元素
```

## ?.

```js
// ?.在JavaScript中如果调用对象是null，则返回undefined
```

## ??空值合并运算符

```js
// ??是空值合并运算符（Nullish Coalescing Operator）。它的作用是返回其左侧操作数，如果左侧操作数为null或undefined，则返回右侧操作数
```

## 调试

```js
// js的调试是在浏览器中进行的，通过F12调出的窗口有和ide相似的调试功能
```

## typeof判断类型

```js
console.log(typeof 'hi')				// string
```

## 函数

```js
// 形参过多，会自动为undefined
// 实参过多，多余的实参被忽略
function eat(a, b = 0, c) {
    return b
}


// 匿名函数（下面这种形式在javascript中称为arrow function，推荐使用）
let a = () => {alert('hi')}
let a = event => {alert('hi')}		// 如果只有一个参数，可以省略括号
let a = event => alert('hi')		// 如果函数体只有一条语句，可以省略花括号
a()


// 匿名函数
let a = function() {}
a()
// 直接调用
(function(){})()
(function(){}())
```

## 对象

```js
// 对象便是json(JavaScript Object Notation)
let 对象名 = {
    属性名: 属性值，
    方法名: 函数
}
let Person = {
    name: 'John',
    age: 20,
    'grade': 10,
    eat: function() {
        console.log(`${this.name} eat`)
    },
    swim: () => console.log('swim')
    /* eat也可以写成下面的形式，更简洁
    eat() {
    	console.log(`${this.name} eat`)
    }
    */
}
// 构造函数
function Person(name) {
  this.name = name;
  this.introduceSelf = function () {
    console.log(`Hi! I'm ${this.name}.`);
  };
}
// 创建对象
const salva = new Person("Salva");
salva.introduceSelf();
// ---------------------------------------------------------------------------------------------------------


// 原型和原型链
// JavaScript 中的每个对象都有一个内置属性，称为其原型。原型本身就是一个对象，因此原型将拥有自己的原型，形成所谓的原型链。当
// 我们到达一个原型，而该原型的原型为 null 时，该链结束
// 当您尝试访问对象的属性时：如果在对象本身中找不到该属性，则会在原型中搜索该属性。如果仍然找不到该属性，则搜索原型的原型，依
// 此类推，直到找到该属性，或者到达链的末尾，在这种情况下返回 undefined
const myObject = {}
Object.getPrototypeOf(myObject);	// 访问对象的原型
// Object.create() 方法创建一个新对象并允许您指定一个将用作新对象原型的对象。
const personPrototype = {
  greet() {
    console.log("hello!");
  },
};
const carl = Object.create(personPrototype);
carl.greet(); // hello!
// 在JavaScript中，所有函数都有一个名为prototype的属性。设置构造函数的原型，可以确保使用该构造函数创建的所有对象都具有该原型
const personPrototype = {
  greet() {
    console.log(`hello, my name is ${this.name}!`);
  },
};
function Person(name) {
  this.name = name;
}
Object.assign(Person.prototype, personPrototype);
// 直接在对象中定义的属性（如此处的名称）称为自身属性，您可以使用静态 Object.hasOwn() 方法检查属性是否为自身属性
const irma = new Person("Irma");

console.log(Object.hasOwn(irma, "name")); // true
console.log(Object.hasOwn(irma, "greet")); // false
// ---------------------------------------------------------------------------------------------------------


// 在JavaScript中使用class创建类，可以省略构造函数
// 私有数据属性必须在类声明中声明，并且其名称以#开头，比如下面的#year就是私有属性
// 私有方法也以#开头，比如下面的#teach函数
// 继承使用extends关键字，构造函数使用constructor() {}，调用父类构造函数使用super()
class Professor extends Person {
  #year;
  name;

  constructor(name) {
    super(name)
    this.name = name;
  }

  introduceSelf() {
    console.log(`Hi! I'm ${this.name}`);
  }
  #teach() {
     console.log('teach'); 
  }
}
const giles = new Person("Giles");
// ---------------------------------------------------------------------------------------------------------


// 访问属性
Person.name
Person['name']
Person.grade
Person['grade']
// 添加属性
Person.hobby = 'swim'
Person['hobby'] = 'swim'
// 删除属性
delete Person.hobby
delete Person['hobby']

// 使用范围for遍历对象
for (a in Person) {			// a为属性名，类型是字符串，比如name属性返回的是'name'
    console.log(Person[a])	// 通过Person[a]访问属性对应的值
}
```

## import

```js
// 文件中包含import或export语句，则该文件被视为模块，模块导入<script type="module" src="a.js"></script>要有type属性
// import的文件中需要有export，比如math.js中有export const PI = 3.14，则main.js才可以导入math.js中的PI
// 基本语法
import { namedExport1, namedExport2 } from './module.js';
// 导入整个模块并使用一个命名空间
import * as moduleName from './module.js';
// 模块使用 export default 导出一个默认值
import defaultExport from './module.js';
// 同时导入默认导出和命名导出
import defaultExport, { namedExport1, namedExport2 } from './module.js';
// 使用 as 关键字重命名导入的内容，以避免命名冲突
import { namedExport as newName } from './module.js';
// 使用 import() 函数进行动态导入，这在需要按需加载模块时非常有用。动态导入返回一个 Promise
import('./module.js')
    .then(module => {
        // 使用模块
        module.namedExport();
    })
    .catch(err => {
        console.error('Error loading module:', err);
    });
```

## 处理json

```js
// 只包含属性，不包含方法
// JSON 要求在字符串和属性名称周围使用双引号。除了围绕整个 JSON 字符串之外，单引号无效
// JSON 实际上可以采用任何可以包含在 JSON 中的数据类型，而不仅仅是数组或对象。例如，单个字符串或数字就是有效的 JSON
JSON.parse()		// 接受 JSON 字符串作为参数，并返回相应的 JavaScript 对象
JSON.stringify()	// 接受一个对象作为参数，并返回等效的 JSON 字符串
```

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

## 异步

```js
// js中调用异步函数会立即返回一个Promise对象
// 使用Promise.then(() => {})会在异步函数调用成功时调用传进的函数
// 由于Promise.then()返回Promise，因此可以chain在一起，形成Promise chaining
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
)
fetchPromise.then((response) => {console.log(`Received response: ${response.status}`)})
// 使用Promise.catch(() => {})会在异步函数调用失败时调用传进的函数
fetchPromise.then((response) => {console.log(`Received response: ${response.status}`)}).catch(() => {})
// await会等待结果返回，async加在函数前使函数成为异步函数
async function myFunction() {
  const response = await fetch('URL')
}
// ---------------------------------------------------------------------------------------------------------


// 使用Promise.all()
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((responses) => {
    for (const response of responses) {
      console.log(`${response.url}: ${response.status}`);
    }
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });
// ---------------------------------------------------------------------------------------------------------


// 使用Promise.any()
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.any([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((response) => {
    console.log(`${response.url}: ${response.status}`);
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });


// 实现一个基于Promise的函数，resolve即then中传递的函数，reject即catch中传递的函数
function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      throw new Error("Alarm delay must not be negative");
    }
    setTimeout(() => {
      resolve(`Wake up, ${person}!`);
    }, 500);
  });
}
alarm('John', 500).then(() => {}).catch(() => {})
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
arr.map(() => {})					// 对每个元素执行操作，返回新数组
arr.filter(() => {})				// 对每个元素执行操作，如果lambda返回true，则添加到新数组，返回新数组
const dogNames = ["Rocket", "Flash", "Bella", "Slugger"]
dogNames.toString()					// Rocket,Flash,Bella,Slugger
const data = "Manchester,London,Liverpool,Birmingham,Leeds,Carlisle"
const cities = data.split(",")		// 返回新数组
cities.join(',')					// 返回字符串


// 判断变量是否为空
let a
isNaN(a)							// 返回true如果变量为空


// js中的sleep
setTimeout(() => {}, 5000)


// 查看DOM元素的详细信息，显示其所有属性和方法
console.dir(document.querySelector('button'))
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
// require()函数在node中用于导入模块，或导入json文件内容
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
