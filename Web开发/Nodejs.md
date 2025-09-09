- [fs模块](#fs模块)
- [path模块](#path模块)
- [dns模块](#dns模块)

- [gray-matter依赖](#gray-matter依赖)

Node.js 是一个开源且跨平台的 JavaScript 运行时环境。它几乎适用于任何类型的项目！Node.js 在浏览器之外运行 V8 JavaScript 引擎（Google Chrome 的核心）。这使得 Node.js 性能非常出色。
运行javascript文件，只需在 shell 中输入 node xxx.js
```js
node server.js
```

```js
// require()函数在node中用于导入模块，或导入json文件内容。一般只在node中使用，因为浏览器和主流标准使用import/export语法
const path = require('path')
```
## fs模块
```js
// fs(file system)模块
// 假设D:\dir目录的结构：
// dir
//	  - child_dir
//		  - ok.js
//	  - hi.js
// 导入
const fs = require('fs')
// 写入数据（异步）
fs.writeFile('D:\a.txt', 'hello')
// 写入数据（同步）
file.writeFileSync('D:\a.txt', 'hello')
// 追加数据
file.appendFile('D:\a.txt', 'hello')
file.appendFileSync('D:\a.txt', 'hello')
// 创建写入流对象
const ws = fs.createWriteStream('D:\a.txt')
ws.write('hello')
// 读取文件
const data = fs.readFile('D:\a.txt')
const data = fs.readFileSync('D:\a.txt')

const rs = fs.createReadStream('D:\a.txt')
rs.on('data', (chunk) => {
    console.log(chunk)
})
// 重命名文件或移动
fs.rename('D:\a.txt', 'D:\b.txt')
fs.renameSync('D:\a.txt', 'D:\b.txt')
// 删除文件
fs.rm('D:\a.txt')
fs.rmSync()
file.unlink('D:\a.txt')
fs.unlinkSync('D:\a.txt')
// 创建文件夹
fs.mkdir('D:\dir')
fs.mkdirSync('D:\dir')
// 读取文件夹（异步）
fs.readdir('D:\dir')
// 读取文件夹（同步），返回['child_dir', 'hi.js']
fs.readdirSync('D:\dir')
// 删除文件夹
fs.rmdir('D:\dir')
// 访问文件属性
fs.stat('D:\a.txt')
// 目录绝对路径
__dirname
// 文件绝对路径
__filename


// 类型别名
fs.PathOrFileDescriptor
```
## path模块
```js
// 导入
const path = require('path')
// 得到绝对路径
path.resolve(__dirname, 'a.txt')
// 获取文件名
path.basename('nodejs.md', '.md')
// 获取扩展名
path.extname('nodejs.md')
// 获取目录名
path.dirname('nodejs.md')
```

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
## dns模块
```js
dns.resolveMx('string')
```
## gray-matter依赖
```js
// example.html文件的内容：
---
title: Hello
slug: home
---
<h1>Hello world!</h1>

// 使用gray-matter
const fs = require('fs');
const matter = require('gray-matter');
const str = fs.readFileSync('example.html', 'utf8');
console.log(matter(str));

// 输出的结果
{
  content: '<h1>Hello world!</h1>',
  data: { 
    title: 'Hello', 
    slug: 'home' 
  }
}
```