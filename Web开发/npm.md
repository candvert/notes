- [主要功能](#主要功能)
- [package.json是什么](#package.json是什么)
- [package-lock.json是什么](#package-lock.json是什么)
- [npm和package.json的关系](#npm和package.json的关系)
- [npm和package-lock.json的关系](#npm和package-lock.json的关系)
- [npm的常用命令](#npm的常用命令)
- [npm的其他命令](#npm的其他命令)

npm 是 nodejs 的内置包管理工具，但是是不同组织开发的。
## 主要功能
下载和安装别人写的 JavaScript 库/工具。
管理项目依赖。
运行脚本（如启动项目、构建、测试）。
发布自己的包到 npm 仓库。
## package.json是什么
package.json 是一个 JSON 格式的配置文件，用来描述一个 Node.js 项目的基本信息和依赖。
主要内容：
项目基本信息（name、version、description）
依赖的库（dependencies、devDependencies）
脚本命令（scripts）
其他配置（license、repository 等）
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}

```
## package-lock.json是什么
package-lock.json 是 npm 自动生成和维护的文件（不需要开发者编写和修改）。
它记录了项目中 每个依赖的确切版本和下载地址（包括子依赖）。
**作用**：
锁定依赖版本：避免团队中或 CI/CD 部署时出现“同一个依赖装出了不同版本”的情况。
加快安装速度：因为 npm 可以直接按照 package-lock.json 精确安装，而不用再去解析所有依赖。
## npm和package.json的关系
**npm 读取并使用 package.json**：当你运行 npm install 时，npm 会查看 package.json，然后安装里面定义的依赖。
**运行脚本**：在 package.json 里定义 scripts，然后可以用 npm run xxx 来执行。
## npm和package-lock.json的关系
如果有 package-lock.json，npm 会优先按照它来安装，保证安装结果和之前一致。
如果没有 lock 文件，则根据 package.json 的依赖规则来安装，并生成新的 package-lock.json。
## npm的常用命令
```shell
# 查看版本
npm -v
# 初始化项目，生成一个 package.json 文件（-y 表示用默认值）
npm init -y
# 安装依赖
npm i <包名>
# 安装指定版本的依赖
npm i <包名>@1.12.3
# 安装最新版本的依赖
npm i <包名>latest
# 安装开发依赖，只在开发环境用，不会打包到生产环境
npm i <包名> --save-dev
npm i -D <包名>    # -D是--save-dev的简写
# 安装全部依赖，根据 package.json 或 package-lock.json 安装项目需要的所有依赖
npm i
# 卸载依赖
npm uninstall <包名>
# 更新所有依赖
npm update
# 更新某个依赖
npm update <包名>
# 运行脚本，执行 package.json 里定义的 scripts
npm run xxx
```
## npm的其他命令
```shell
# 查看局部安装路径
npm root
# 查看全局安装路径
npm root -g
# 查看全局已安装的包
npm list -g --depth=0
# 全局路径下安装一个package
npm i package_name -g
# 查看项目本地安装的包
npm list
# 
npm config list
# 
npm config get prefix
#
npm config get cache
#
npm config get registry
#
npm config set registry https://registry.npm.taobao.org
#
npm view <包名> versions
#
npm cache clean --force
```
