- [初始配置](#初始配置)
- [设置全局代理](#设置全局代理)
- [将本地仓库和远程仓库关联起来](#将本地仓库和远程仓库关联起来)
- [常用指令](#常用指令)
- [远程提交更新且本地有修改，进行merge并提交](#远程提交更新且本地有修改，进行merge并提交)
- [git remote](#git%20remote)
- [git branch](#git%20branch)
- [git push](#git%20push)
- [git status](#git%20status)
- [git diff](#git%20diff)
- [.gitignore](#.gitignore)

# 初始配置
```
git config --global user.name "candvert"
git config --global user.email leiyue159@gmail.com
验证是否配置成功
git config --global --list
```

下面是在windows上进行的配置
```
生成ssh密钥与github关联
ssh-keygen -t rsa -b 4096 -C "leiyue159@gmail.com"
生成的密钥位于C:\Users\leiyu\.ssh\id_rsa.pub
```
进入github的设置页
![[git_01.png]]
添加密钥
![[git_02.png]]
将之前生成的id_rsa.pub中的文本填入下图（Title可以随意取）
![[git_03.png]]
在git配置文件中添加一条命令别名，作用是切换目录并执行git.sh
```
cd
vim .bashrc
alias cda='cd /d/Document/markdown && ./git.sh'
source .bashrc


git.sh文件的内容为：
#!/bin/bash

git add .
git commit -m "a"
git push
```
有时 git push 会失败，若出错消息是 ssh，则在密钥所在目录创建一个 config 文件，并写入下面内容。作用是通过 HTTPS 端口进行 SSH 连接
```
Host github.com
  HostName ssh.github.com
  Port 443
  User git
  IdentityFile C:\Users\leiyu\.ssh\id_rsa   # 替换为你的私钥路径
```
## 设置全局代理
```
需要注意该设置默认你开启了Clash，若没有开启Clash会出错
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```
## 将本地仓库和远程仓库关联起来
```
需要注意本地仓库必须至少commit一次才能关联
git init
git add .
git commit -m "comment"

git remote add origin git@github.com:candvert/Schedule.git
git branch -M main
git push -u origin main
```
# 常用命令
```shell
git init
git pull
git add
git commit -m "first commit"
git push
git status
git diff
git log
```
## 远程提交更新且本地有修改，进行merge并提交
```shell
git add .
git commit -m " "
发生错误后，对冲突的文件要进行手动修改
然后再
git add .
git commit -m " "
git push
```
## git remote
```shell
# 添加仓库，将其命名为origin
git remote add origin git@github.com:candvert/Schedule.git
# 删除仓库
git remote remove origin
# 显示已添加仓库
git remote
```
## git branch
```shell
# -m或--move选项用于重命名当前分支或指定的分支
# 重命名当前分支
git branch -m ok
# 重命名master分支
git branch -m master ok

# -M选项用于强制重命名分支
git branch -M ok
```
## git push
```shell
# -u或--set-upstream选项用于设置上游（upstream）分支。这意味着它将当前分支与远程分支关联，使得未来的git pull和git push命令
# 可以省略远程和分支名称
# 将远程的main分支和本地的master分支关联起来
git push -u origin master:main
# 将远程的main分支和本地的main分支关联起来
git push -u origin main
```
## git fetch
```shell
git fetch 可获取远程所有最新更新，但不会自动合并到本地
```
## git diff
```shell
// 查看本地和远程仓库 main.txt文件的区别
git diff main.txt
```
## .gitignore
```
/build/ 忽略当前目录下的文件夹
```
