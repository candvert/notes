- [下载安装](#下载安装)
- [打开设置](#打开设置)
- [设置背景图片和不透明度](#设置背景图片和不透明度)
- [设置启动时窗口大小](#设置启动时窗口大小)
- [隐藏滚动条](#隐藏滚动条)
- [快捷键](#快捷键)
- [oh-my-posh美化](#oh-my-posh美化)

- [快捷键](#快捷键)
## 下载安装
可以从microsoft store下载
![](/images/window_01.png)
## 打开设置
之后windows上的命令行工具和powershell都会变为Windows Terminal。

打开Windows Terminal之后按Ctrl + ,或点击下图的图标打开设置。
![](/images/window_02.png)
## 设置背景图片和不透明度
![](/images/window_03.png)

![](/images/window_09.png)

## 设置启动时窗口大小
![](/images/window_08.png)

## 隐藏滚动条
![](/images/window_10.png)
## oh-my-posh美化
打开Windows Terminal后按win + x选择“终端管理员”。输入下列命令
```sh
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
```
作用​​：允许运行本地脚本

通过下列命令验证是否生效。
```sh
Get-ExecutionPolicy -List
```
输出中CurrentUser应为RemoteSigned

从microsoft store下载oh-my-posh
![](/images/window_11.png)
打开powershell后输入下列命令
```sh
New-Item -Path $PROFILE -Type File -Force
notepad $PROFILE
```

然后在记事本中输入下行文字
```sh
oh-my-posh init pwsh | Invoke-Expression
```
重启Windows Terminal之后便生效了。

更换其他主题
```sh
notepad $PROFILE
```
然后在记事本中输入下行文字
```sh
oh-my-posh init pwsh --config 'C:\Users\leiyu\AppData\Local\Programs\oh-my-posh\themes\M365Princess.omp.json' | Invoke-Expression
```
M365Princess.omp.json是主题名，默认安装的主题可以在C:\Users\leiyu\AppData\Local\Programs\oh-my-posh\themes目录找到。
主题预览可以前往https://ohmyposh.dev/docs/themes
名称中带有minimal的主题不需要Nerd字体（Nerd字体需要另外安装，可以显示一些图标）


## 快捷键
```sh
F11                                                全屏
ctrl + 滚轮                                         缩放字体
ctrl + tab                                         切换窗口
ctrl + shift + t                                   新建窗口
ctrl + shift + w                                   关闭窗口



win + `                                            显示/隐藏Quark窗口
```