## 下载安装
可以从microsoft store下载
![[window_01.png]]
## 修改默认设置
之后windows上打开的命令行工具和powershell都会变为Windows Terminal。

打开Windows Terminal之后按Ctrl + ,或点击下图的图标打开设置。
![[window_02.png]]



修改透明度和半透明纹理
![[window_03.png]]
![[window_04.png]]
效果：
![[window_05.png]]



设置专注模式快捷键（即隐藏标题栏）
![[window_06.png]]
![[window_07.png]]



设置启动时窗口大小
![[window_08.png]]


## 快捷键
```
win + `                                            显示/隐藏Quark窗口
ctrl + 滚轮                                         缩放字体
F11                                                全屏
```
## oh-my-posh美化
打开Windows Terminal后按win + x选择“终端管理员”。输入下列命令。
```shell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
```
作用​​：允许运行本地脚本。

通过下列命令验证是否生效。
```shell
Get-ExecutionPolicy -List
```
输出中CurrentUser应为RemoteSigned。




安装oh-my-posh
```shell
winget install JanDeDobbeleer.OhMyPosh --source winget --scope user --force
```

之后输入下列命令（默认使用的shell是powershell，如果是其他shell则要输入其他命令）
```shell
New-Item -Path $PROFILE -Type File -Force
notepad $PROFILE
```

然后在记事本中输入下行文字
```
oh-my-posh init pwsh | Invoke-Expression
```
重启Windows Terminal之后便生效了。

更换其他主题
```
notepad $PROFILE
```
然后在记事本中输入下行文字
```
oh-my-posh init pwsh --config 'C:\Users\leiyu\AppData\Local\Programs\oh-my-posh\themes\M365Princess.omp.json' | Invoke-Expression
```
M365Princess.omp.json是主题名，默认安装的主题可以在C:\Users\leiyu\AppData\Local\Programs\oh-my-posh\themes目录找到。
主题预览可以前往https://ohmyposh.dev/docs/themes
名称中带有minimal的主题不需要Nerd字体（Nerd字体需要另外安装，可以显示一些图标）