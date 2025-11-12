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