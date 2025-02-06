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

## .gitignore
```
/build/ 忽略当前目录下的文件夹
```