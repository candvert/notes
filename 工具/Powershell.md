## 当前目录下所有文件的行数
```powershell
(Get-ChildItem -Recurse -File | Get-Content | Measure-Object).Count
```
## 设置环境变量
```powershell
$env:GOOS = "windows"
```
## 特定单词的具体行号
```powershell
Get-ChildItem -Path "." -Recurse -File | Select-String -Pattern "你的特定单词"

# 确匹配大小写，使用 -CaseSensitive 参数
Select-String -Pattern "KeyWord" -CaseSensitive

# -Filter 参数限定只搜索特定类型的文件（如只搜索 .log或 .txt文件）
Get-ChildItem -Path "." -Recurse -Filter "*.log"
```
## 打印进程信息
```powershell
# 输出占用 8000 端口的进程详细信息，包括进程名、PID、CPU 和内存使用情况等
Get-Process -Id (Get-NetTCPConnection -LocalPort 8000).OwningProcess

# 结束进程
Stop-Process -Id 12960 -Force

# 查找进程名称中包含 "chrome" 的进程
Get-Process | Where-Object { $_.ProcessName -like "*chrome*" }

# 使用更简洁的语法（Where-Object 的别名是 ?）
Get-Process | ? { $_.ProcessName -like "*chrome*" }
```

```powershell
# 打开配置文件
notepad $PROFILE

# 新增一个变量
$MyWorkspace = "C:\Users\YourName\MyProjects"

# 查看某个变量存不存在
Test-Path variable:MyWorkspace

# 设置命令别名，实现的功能为dd命令切换当前目录到D:\Document\Web
function cd-dir { Set-Location "D:\Document\Web" }
Set-Alias -Name dd -Value cd-dir
```