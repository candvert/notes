## 和Nextjs一起使用
```shell
步骤：
首先安装依赖
npm install prisma --save-dev
npm install @prisma/client
再通过该命令初始化创建必要的文件
npx prisma init
之后便是在prisma/schema.prisma文件中添加表的相关代码
之后运行npx prisma migrate dev --name init会生成PrismaClient在并在服务器数据库创建表
创建lib/prisma.ts文件，并复制官网代码，使PrismaClient成为全局实例
然后便可以在代码中使用从lib/prisma.ts导入的prisma变量进行数据库的CRUD

所有数据库操作都应在 API 路由中完成，prisma的api只能在node.js中使用，也就是说不要直接在tsx文件中用prisma，会报错

npx prisma migrate reset可以重置服务器数据库

npx prisma db push在​​开发环境​​中快速同步服务器数据库​​，而无需生成迁移文件，快速、简单​​

npx prisma studio在本地打开数据库网页


如果使用的是sqlite数据库
npx prisma init --datasource-provider sqlite
该命令会通过.env文件中的DATABASE_URL创建数据库文件，DATABASE_URL以prisma文件夹为工作目录
npx prisma migrate dev --name init


.env文件中的DATABASE_URL便是与服务器进行连接的url

每次修改prisma/schema.prisma文件（即添加或修改表结构）后，运行npx prisma migrate dev，该命令有两个作用，一是与服务器上的数据库进行同步（同步表结构），二是生成PrismaClient（schema.prisma 中定义的每一个 model、每一个字段、每一个关系，都会转化为 PrismaClient 中对应的 TypeScript 类型和方法。不需要手动编写 SQL并有类型安全的保障）

npx prisma migrate dev --name <migration_name>
```
**总结：核心关系与工作流程**
1. ​​schema.prisma 是“单一事实来源”​​：它是整个流程的​​源头和核心​​。你通过修改它来定义或变更数据结构。
2. prisma migrate​​：基于 ​​Schema​​ 生成并执行同步​​数据库​​结构的 ​​SQL​​。​​目的是让数据库“匹配”Schema定义。​
3. Next.js 应用是最终的消费者​​：通过导入和使用生成的 ​​PrismaClient​​，在运行时与 ​​Database​​ 交互，实现数据的持久化。
4. 类型安全是核心优势​​：整个链条由 Schema 驱动，使得数据库操作在编译时就能得到类型检查，极大减少了运行时错误。
## lib/prisma.ts
```ts
// 文件
import prisma from "../../prisma/lib/prisma";

const globalForPrisma = global as unknown as { 
    prisma: PrismaClient
}

const prisma = globalForPrisma.prisma || new PrismaClient()

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma

export default prisma
```
## api
```ts
// schema.prisma中的表结构
model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String?
  posts Post[]
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int
}
// 添加数据
const user = await prisma.user.create({
	data: {
	  name: 'Alice',
	  email: 'alice@prisma.io',
	},
})
// 获取所有记录
const users = await prisma.user.findMany()
// 添加数据（也向关联的Post表中添加记录）
const user = await prisma.user.create({
	data: {
	  name: 'Bob',
	  email: 'bob@prisma.io',
	  posts: {
		create: [
		  {
			title: 'Hello World',
			published: true
		  },
		  {
			title: 'My second post',
			content: 'This is still a draft'
		  }
		],
	  },
	},
})
// 获取所有记录（将关联的Post表中的记录也获取到）
const usersWithPosts = await prisma.user.findMany({
	include: {
	  posts: true,
	},
})
```
