## 安装
```sh
从 https://github.com/microsoft/WSL 下载安装
安装完之后以管理员打开 PowerShell，输入 wsl --install
默认安装的是 Ubuntu
```
## 文件系统
![](/images/wsl_02.png)
## 命令
```sh
# 列出所有可下载的 Linux 发行版
wsl --list --online
wsl -l -o

# 列出已安装的 Linux 发行版
wsl --list --verbose
wsl -l -v

# 安装 Linux 发行版
wsl --install -d <Distribution Name>

# 卸载 Linux 分发版
wsl --unregister <Distribution Name>

# 设置默认使用的 Linux 发行版
wsl --set-default <Distribution Name>

# 不更改默认使用的 Linux 发行版的情况下从 PowerShell 中运行特定的 Linux 发行版
wsl --distribution <Distribution Name>
wsl --distribution <Distribution Name> --user <Username>

# 终止所有正在运行的发行版
wsl --shutdown

# 终止指定的发行版
wsl --terminate <Distribution Name>

# 检查 WSL 状态
wsl --status

# 检查 WSL 版本
wsl --version

# 以特定用户身份运行
wsl --user <Username>

# 更改发行版的默认用户
# ubuntu config --default-user johndoe 将 Ubuntu 的默认用户更改为“johndoe”用户
<Distribution Name> config --default-user <Username>

# 将指定发行版的快照导出为新的分发文件。 默认为 tar 格式
# 例如 wsl --export Ubuntu ubuntu.tar
wsl --export <Distribution Name> <FileName>

# 将指定的 tar 文件导入为新的发行版
# 例如 wsl --import My_Ubuntu D:/wsl C:\ubuntu.tar
wsl --import <Distribution Name> <InstallLocation> <FileName>
```