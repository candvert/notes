- [安装](#安装)
- [初始化项目](#初始化项目)
- [创建虚拟环境](#创建虚拟环境)
- [包管理操作](#包管理操作)
- [运行Python脚本](#运行Python脚本)
- [运行特定命令](#运行特定命令)
- [依赖锁定](#依赖锁定)
- [导出依赖](#导出依赖)
- [从requirements.txt安装依赖](#从requirements.txt安装依赖)
- [验证环境状态](#验证环境状态)
## 安装
```sh
pip install uv
```
## 初始化项目
```sh
uv init myproject

# 初始化当前文件夹
uv init
```
## 创建虚拟环境
```sh
uv venv
```
## 包管理操作
```sh
# 安装单个包
uv add pandas

# 安装开发依赖
uv add --dev black
uv add black --dev

# 移除包
uv remove pandas

# 移除开发依赖
uv remove --dev black
uv remove black --dev
```
## 运行Python脚本
```sh
uv run python main.py
```
## 运行特定命令
```sh
uv run pytest tests/
```
## 依赖锁定
```sh
# 生成锁定文件
uv lock
```
## 导出依赖
```sh
uv pip freeze > requirements.txt
```
## 从requirements.txt安装依赖
```sh
uv pip compile requirements.txt -o requirements.lock
uv sync
```
## 验证环境状态
```sh
# 检查当前 Python 环境
which python
which pip

# 检查安装的包（仅显示当前环境的包）
pip list

# 检查虚拟环境变量
echo $VIRTUAL_ENV
```