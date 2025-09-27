- [fs模块](#fs模块)
- [path模块](#path模块)
- [dns模块](#dns模块)

- [gray-matter依赖](#gray-matter依赖)
- [zod依赖](#zod依赖)
- [swr依赖](#swr依赖)
- [next-mdx-remote依赖](#next-mdx-remote依赖)

Node.js 是一个开源且跨平台的 JavaScript 运行时环境。Node.js 在浏览器之外运行 V8 JavaScript 引擎（Google Chrome 的核心）。这使得 Node.js 性能非常出色。
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


// 在 nextjs 中 process.cwd() 输出的是根目录，即 package.json 所在目录
path.join(process.cwd(), 'content', 'post.md')
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
// 作用：提取 mdx 文档的元数据
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
## zod依赖
```typescript
// 作用：验证数据类型和格式
// 先定义一个模式
import * as z from "zod"; 
 
const Player = z.object({ 
  username: z.string(),
  xp: z.number()
});
// 如果数据类型和格式正确，则返回参数的深拷贝
Player.parse({ username: "billie", xp: 100 }); 
// => returns { username: "billie", xp: 100 }
// 异步使用该函数
await Player.parseAsync({ username: "billie", xp: 100 }); 
// 当验证失败时，.parse() 方法将抛出一个 ZodError 实例，其中包含有关验证问题的详细信息
try {
  Player.parse({ username: 42, xp: "100" });
} catch(error){
  if(error instanceof z.ZodError){
    error.issues; 
  }
}
// 如果不想用try/catch，可以使用safeParse函数
const result = Player.safeParse({ username: 42, xp: "100" });
if (!result.success) {
  result.error;   // ZodError instance
} else {
  result.data;    // { username: string; xp: number }
}
// 异步使用该函数
await schema.safeParseAsync("hello");




// 提取类型
const Player = z.object({ 
  username: z.string(),
  xp: z.number()
});
 
// extract the inferred type
type Player = z.infer<typeof Player>;
 
// use it in your code
const player: Player = { username: "billie", xp: 100 };






// 存在的类型： https://zod.dev/api
// primitive types
z.string();
z.number();
z.bigint();
z.boolean();
z.symbol();
z.undefined();
z.null();

// 限制条件
z.string().max(5);
z.string().min(5);
z.string().length(5);
z.string().regex(/^[a-z]+$/);
z.string().startsWith("aaa");
z.string().endsWith("zzz");
z.string().includes("---");
z.string().uppercase();
z.string().lowercase();
z.string().trim(); // trim whitespace
z.string().toLowerCase(); // toLowerCase
z.string().toUpperCase(); // toUpperCase
z.string().normalize(); // normalize unicode characters

// 常见的字符串类型，如email
z.email();
z.uuid();
z.url();
z.httpUrl();       // http or https URLs only
z.hostname();
z.emoji();         // validates a single emoji character
z.base64();
z.base64url();
z.hex();
z.jwt();
z.nanoid();
z.cuid();
z.cuid2();
z.ulid();
z.ipv4();
z.ipv6();
z.cidrv4();        // ipv4 CIDR block
z.cidrv6();        // ipv6 CIDR block
z.hash("sha256");  // or "sha1", "sha384", "sha512", "md5"
z.iso.date();
z.iso.time();
z.iso.datetime();
z.iso.duration();
```
## swr依赖
```typescript
// 用于数据获取的 React Hooks 库
// 在这个例子中，useSWR hook 接受一个 key 字符串和一个 fetcher 函数。key 是数据的唯一标识符（通常是 API URL），将传递给 fetcher。fetcher 可以是任何返回数据的异步函数，您可以使用原生的 fetch 或 Axios 之类的工具。根据请求的状态，该钩子返回 3 个值：data、isLoading 和 error。
import useSWR from 'swr'

function Profile() {
  const { data, error, isLoading } = useSWR('/api/user', fetcher)

  if (error) return <div>failed to load</div>
  if (isLoading) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```
## next-mdx-remote
```typescript
// 将下面内容复制到next.config.js中
const nextConfig = {
  transpilePackages: ['next-mdx-remote'],
}
// 使用
import { serialize } from 'next-mdx-remote/serialize'
import { MDXRemote } from 'next-mdx-remote'

import Test from '../components/test'

const components = { Test }

export default function TestPage({ source }) {
  return (
    <div className="wrapper">
      <MDXRemote {...source} components={components} />
    </div>
  )
}

export async function getStaticProps() {
  // MDX text - can be from a local file, database, anywhere
  const source = 'Some **mdx** text, with a component <Test />'
  const mdxSource = await serialize(source)
  return { props: { source: mdxSource } }
}
```