## 当前目录下所有文件的行数
```powershell
(Get-ChildItem -Recurse -File | Get-Content | Measure-Object).Count
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